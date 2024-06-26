# 快速部署

**ElasticSearch 能够基于 Docker 实现快速部署**

镜像介绍请见官网：[https://hub.docker.com/\_/elasticsearch](https://hub.docker.com/\_/elasticsearch)

快速部署命令：

```bash
docker run -d --name elasticsearch --net host -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:5
```

之后，ES 服务就会在宿主机的 9200 端口开始监听了。

![elasticsearch](https://raw.githubusercontent.com/SataQiu/awesome-research/master/images/elasticsearch-preview.png)

不过默认条件下，9300 端口未对外开发，对于某些服务会造成影响。

**开放 9300 端口的方法如下：**

1.  编写 ElasticSearch 配置文件 `build/elasticsearch.yml`

    \`\`\`yml cluster.name: elasticsearch transport.host: 0.0.0.0 http.host: 0.0.0.0

````
# Uncomment the following lines for a production cluster deployment
#transport.host: 0.0.0.0
#discovery.zen.minimum_master_nodes: 1
```
````

1.  编写 Dockerfile `build/Dockerfile`

    ```
    FROM elasticsearch:5.6.4

    COPY elasticsearch.yml /usr/share/elasticsearch/config/

    EXPOSE 9200 9300
    ```
2.  编译镜像

    ```bash
    # cd build
    # docker build -t elasticsearch:5.6.4-custom .
    ```
3.  在正式运行之前需要检查主机配置（`/etc/sysctl.conf`） 确保包含如下配置：

    ```bash
    vm.max_map_count=262144
    ```
4.  运行镜像

    ```bash
     # docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:5.6.4-custom
    ```

**附加信息**

1.  该镜像已经上传至 DockerHub \[shidaqiu/elasticsearch:5.6.4-custom]

    ```bash
    # docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" shidaqiu/elasticsearch:5.6.4-custom
    ```
2.  可用查询工具： [sense-chrome-plugin](https://chrome.google.com/webstore/detail/elasticsearch-toolbox/focdbmjgdonlpdknobfghplhmafpgfbp?hl=zh\_CN)

    ![sense](https://raw.githubusercontent.com/SataQiu/awesome-research/master/images/elasticsearch-sense.jpg)
