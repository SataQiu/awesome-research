# 存储支持

SkyWalking 存储是插拔式的，可以通过配置 `application.yml` 来对接不同的后端存储。

原生支持：

- H2
- ElasticSearch 6
- MySQL
- TiDB

## H2

[H2](http://www.h2database.com) 是一种简单的内存数据库，也是发布版默认使用的后端存储数据库，不能应用于生产环境。查看 [Database URL Overview](http://www.h2database.com/html/features.html#database_url) 可获取 url 配置的详细说明。

配置示例：

```yaml
storage:
  h2:
    driver: org.h2.jdbcx.JdbcDataSource
    url: jdbc:h2:mem:skywalking-oap-db
    user: sa
```

## ElasticSearch 6

**需要 ElasticSearch 6.3.0 或更高版本；使用 HTTP RestHighLevelClient 连接服务器。**

配置示例：

```yaml
storage:
  elasticsearch:
    # nameSpace: ${SW_NAMESPACE:""}  # 索引前缀（设置后，所有索引都会加上这个前缀）
    # user: ${SW_ES_USER:""} # 用户名
    # password: ${SW_ES_PASSWORD:""} # 密码
    clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:localhost:9200} # ES 地址
    indexShardsNumber: ${SW_STORAGE_ES_INDEX_SHARDS_NUMBER:2} # 索引分片数量
    indexReplicasNumber: ${SW_STORAGE_ES_INDEX_REPLICAS_NUMBER:0} # 索引备份数量
    bulkActions: ${SW_STORAGE_ES_BULK_ACTIONS:2000} # 每 2000 个请求批量执行一次 bulk 动作
    bulkSize: ${SW_STORAGE_ES_BULK_SIZE:20} # 每达到 20 mb 执行一次 flush
    flushInterval: ${SW_STORAGE_ES_FLUSH_INTERVAL:10} # 每 10 秒执行一次 flush
    concurrentRequests: ${SW_STORAGE_ES_CONCURRENT_REQUESTS:2} # 并发请求数
```

- 目前 SkyWalking 只支持 ElsaticSearch 的 Basic Authentication 模式；

- 分片类似于数据库的表分区概念，分片后查询会在各个分片并行执行，从而提升效率；
  然而，分片也不能太多，因为分片本身存在资源开销，而且分片并发查询，可能引起资源竞争；
  建议：每一个分片数据文件小于 30 GB；每一个索引中的一个分片对应一个节点；分片数小于等于节点数

## MySQL

注意：官方发布版本未直接包含 MySQL 驱动程序，需要手动下载 MySQL 驱动，然后放置到 `oap-libs` 目录。

配置示例：

```yaml
storage:
  mysql:
```

具体配置定义在文件 `datasource-settings.properties` 中。

## TiDB

TiDB 是一款开源分布式数据库，兼容 MySQL，支持无限水平扩展，具备强一致性和高可用性，云原生产品。

配置示例：

```yaml
storage:
  mysql:
```

具体配置定义在文件 `datasource-settings.properties` 中。