---
title: dnsmasq
date: 2019-04-25 22:17:00
tags: 
categories: 
top:
---

# dnsmasq

```yml
version: "3"
services:
  dnsmasq:
    image: jpillora/dnsmasq:latest
    restart: always
    container_name: dnsmasq
    ports:
      - "53:53/udp"
      - "53:53/tcp"
      - "8100:8080"
    volumes:
      - ./conf/dnsmasq.conf:/etc/dnsmasq.conf:rw
      - ./conf/resolv.conf:/etc/resolv.conf:ro
      - ./conf/hosts:/etc/hosts:ro
    cap_add:
      - NET_ADMIN
    environment:
      - HTTP_USER=admin
      - HTTP_PASS=admin
```