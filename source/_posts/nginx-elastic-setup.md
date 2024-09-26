---
title: 使用 ElasticSearch 处理 Nginx 日志
date: 2023-07-06 14:54:19
tags:
  - docker
  - nginx
categories:
  - linux
top:
---

自己的服务器日志只是简单的做了按天分割的处理，要想从日志中找些什么确实是一个很难完成的事情，于是想到了对日志文件的处理，对于这样的任务，经常在服务中使用的 ELK 成为了我的首选。

> 以下所有服务的搭建都基于 Docker Compose

### 服务搭建

首先便是搭建 Kibana、Elastic Search、Filebeat 和 Metricbeat 四个服务。

```yaml
---
version: '3.8'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.11
    restart: unless-stopped
    container_name: elasticsearch
    environment:
      - "discovery.type=single-node"
    networks:
      - elastic
    ports:
      - 9200:9200
      - 9300:9300
  kibana:
    image: docker.elastic.co/kibana/kibana:7.17.11
    restart: unless-stopped
    container_name: kibana
    environment:
      - "ELASTICSEARCH_HOSTS=http://elasticsearch:9200"
    networks:
      - elastic
    ports:
      - 5601:5601
  metricbeat:
    image: docker.elastic.co/beats/metricbeat:7.17.11
    restart: unless-stopped
    container_name: metricbeat
    user: root
    networks:
      - elastic
    command: metricbeat -e -E setup.kibana.host=kibana:5601 -E output.elasticsearch.hosts=["elasticsearch:9200"]
    volumes:
      - ./metricbeat/metricbeat.yml:/usr/share/metricbeat/metricbeat.yml
      - ./metricbeat/modules.d:/usr/share/metricbeat/modules.d
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /sys/fs/cgroup:/hostfs/sys/fs/cgroup:ro
      - /proc:/hostfs/proc:ro
      - /:/hostfs:ro
  filebeat:
    image: docker.elastic.co/beats/filebeat:7.17.11
    restart: unless-stopped
    container_name: filebeat
    user: root
    networks:
      - elastic
    command: filebeat -e --strict.perms=false -E setup.kibana.host=kibana:5601 -E output.elasticsearch.hosts=["elasticsearch:9200"]
    volumes:
      - ./filebeat/filebeat.yml:/usr/share/filebeat/filebeat.yml:ro
      - ./filebeat/modules.d:/usr/share/filebeat/modules.d
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /var/lib/docker/containers:/var/lib/docker/containers:ro
  nginx:
    image: nginx
    restart: unless-stopped
    container_name: elastic_nginx
    networks:
      - elastic
    ports:
      - 80:80
networks:
  elastic:

```

这里使用的版本是 7.17.11 版本，当前最新版本为 8.8，在当前版本增强了安全特性，导致测试不是太过友好，所以这次搭建选择了较老的版本，新版本以后有机会再试吧。

可以看到，使用我们将所有的服务全部放在了同一个 Network 下，并且将 `/var/run/docker.sock` 映射到了 `filebeat` 与 `metricbeat` 两个服务中，便于获取 `nginx` 的信息。

### metricbeat 配置

#### 启用 Nginx 模块

metricbeat 默认包含许多预配置项，对于 nginx 直接启用即可。

```shell
docker-compose exec metricbeat metricbeat modules enable nginx
```

#### 自动发现

由于我们的服务是基于 Docker 无法像传统服务执行本机路径，所以需要配置自动发现。

配置文件为 `metricbeat.yml `。

```yaml
metricbeat.autodiscover:
  providers:
    - type: docker
      templates:
        - condition:
            contains:
              docker.container.image: nginx
          config:
            - module: nginx
              metricsets: ["stubstatus"]
              hosts: "${data.host}:${data.port}"
```

### Filebeat 配置

#### 启用 Nginx 模块

```shell
docker-compose exec filebeat filebeat modules enable nginx
```

#### 自动发现

```yaml
filebeat.autodiscover:
 providers:
   - type: docker
     templates:
       - condition:
         contains:
           docker.container.image: nginx
         config:
           - module: nginx
             access:
               input:
                 type: docker
                 containers.ids:
                   - "${data.docker.container.id}"
             error:
               input:
                 type: docker
                 containers.ids:
                   - "${data.docker.container.id}"
```

配置结束后便可以启动服务。

### Kibana 和 Elastic Search

访问 http://127.0.0.1:9200/ 可以检查 ES 的启动是否成功。

```json
{
   "name":"6c6c3a287390",
   "cluster_name":"docker-cluster",
   "cluster_uuid":"d77Jxv4TRIWYBrIMWKA8Dw",
   "version":{
      "number":"7.17.11",
      "build_flavor":"default",
      "build_type":"docker",
      "build_hash":"eeedb98c60326ea3d46caef960fb4c77958fb885",
      "build_date":"2023-06-23T05:33:12.261262042Z",
      "build_snapshot":false,
      "lucene_version":"8.11.1",
      "minimum_wire_compatibility_version":"6.8.0",
      "minimum_index_compatibility_version":"6.0.0-beta1"
   },
   "tagline":"You Know, for Search"
}
```

使用 http://127.0.0.1:5601/ 访问 Kibana。

点击左侧 **Observability** 可以查看可视化数据，点击 Logs 可以查看 Nginx 日志。

![image-20230706182402842](https://images.orkva.com/images/2023/07/06/image-20230706182402842.png)

> 参考：
>
> * [如何使用 Elastic Stack 监测 Nginx Web 服务器](https://www.elastic.co/cn/blog/how-to-monitor-nginx-web-servers-with-the-elastic-stack)
