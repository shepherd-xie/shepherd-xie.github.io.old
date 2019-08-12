---
title: ELK
date: 2019-04-25 22:17:00
tags: 
categories: 
top:
---

# ELK

```yml
version: '3'
services:
  elasticsearch: 
#    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.0
    image: elasticsearch:6.4.0
    restart: always
    container_name: elasticsearch
    dns: 47.93.241.176
    environment:
      - discovery.type=single-node
#      - cluster.name=esCluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:    
      - 9200:9200
      - 9300:9300
    volumes:
      - ./esdata/:/usr/share/elasticsearch/data/
      - ./plugins/:/usr/share/elasticsearch/plugins/
  kibana:
#    image: docker.elastic.co/kibana/kibana:6.4.0
    image: kibana:6.4.0
    restart: always
    container_name: kibana
    dns: 47.93.241.176
    links:
      - elasticsearch
    depends_on:
      - elasticsearch
#    environment:
#      - SERVER_NAME=kibana
#      - ELASTICSEARCH_URL=http://elasticsearch:9200
#      - ELASTICSEARCH_USERNAME=elastic
#      - ELASTICSEARCH_PASSWORD=changeme
    ports:
      - 5601:5601
    volumes:
      - ./kibana.yml:/usr/share/kibana/config/kibana.yml
  logstash:
#    image: docker.elastic.co/kibana/logstash:6.4.0
    image: logstash:6.4.0
    restart: always
    container_name: logstash
    dns: 47.93.241.176
    command: logstash -f /usr/share/logstash/config/logstash-mysql.conf
    depends_on:
      - elasticsearch
    volumes:
      - ./config/:/usr/share/logstash/config/
```