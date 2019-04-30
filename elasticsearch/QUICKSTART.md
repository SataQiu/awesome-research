# 快速部署

**ElasticSearch 能够基于 Docker 实现快速部署**

镜像介绍请见官网：[https://hub.docker.com/_/elasticsearch](https://hub.docker.com/_/elasticsearch)

快速部署命令：

```sh
docker run -d --name elasticsearch --net host -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:5
```

之后，ES 服务就会在宿主机的 9200 端口开始监听了（9300 是集群管理端口，默认只绑定 127.0.0.1）。

![elasticsearch-preview](https://github.com/SataQiu/awesome-research/blob/master/images/elasticsearch-preview.png)