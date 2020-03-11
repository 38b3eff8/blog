---
title: 使用 Docker 启动常见服务
date: 2019-08-28 13:33:39
tags:
  - Docker
  - RabbitMQ
  - Neo4j
  - 图数据库
  - Grafana
  - 数据可视化
---

## 创建 RabbitMQ 服务器

```bash
docker run -d -p 15672:15672  -p 5672:5672 --hostname my-rabbit --name some-rabbit rabbitmq
```

链接：amqp://guest:guest@localhost:5672

## 创建 Neo4j 服务器

```bash
docker run -d --publish=7474:7474 --publish=7687:7687 --volume=$HOME/data/neo4j/data:/data neo4j
```

服务启动后，浏览器访问 [http://localhost:7474/browser/](http://localhost:7474/browser/)

## 创建 grafana 服务器

```bash
docker run -d -p 4000:3000 -v $HOME/data/grafana:/var/lib/grafana grafana/grafana
```

服务启动后，浏览器访问 [http://localhost:4000](http://localhost:4000)
