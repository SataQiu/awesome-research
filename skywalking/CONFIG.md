# 配置解读

以 6.0.0-GA 版本配置为例：

```yaml
# 集群配置：配置 OAP Server 的运行模式(单节点/集群)
# 集群模式需要依赖第三方后端协调，分别是 zookeeper、kubernetes 以及 consul
# 单节点、zookeeper、kubernetes、consul 选择一种配置生效
cluster:

  # 单节点模式（默认运行模式，开发测试环境使用）
  standalone:

  # zookeeper 集群模式
  # 支持 3.5+ 版本，兼容 3.4.x
  zookeeper:
     # zk 命名空间(集群元数据存储的顶级目录)
     nameSpace: ${SW_NAMESPACE:""}
     # zk 连接地址，多个地址可以使用逗号分隔，用于初始化 curator client
     hostPort: ${SW_CLUSTER_ZK_HOST_PORT:localhost:2181}
     # 重试的初始等待时间(毫秒)，用于初始化 curator client
     baseSleepTimeMs: ${SW_CLUSTER_ZK_SLEEP_TIME:1000}
     # 最大重试次数，用于初始化 curator client
     maxRetries: ${SW_CLUSTER_ZK_MAX_RETRIES:3}

   # kubernetes 集群模式(仅在基于 k8s 部署时才能使用)
   kubernetes:
     # 客户端 watch 超时时间(默认 60 秒)
     watchTimeoutSeconds: ${SW_CLUSTER_K8S_WATCH_TIMEOUT:60}
     # 集群运行命名空间
     namespace: ${SW_CLUSTER_K8S_NAMESPACE:default}
     # 标签选择器（根据标签判断哪些 Pod 是集群成员)
     labelSelector: ${SW_CLUSTER_K8S_LABEL:app=collector,release=skywalking}
     # 该值默认从 metadata.uid 取得(参见 k8s 环境下 oap 部署文档)
     uidEnvName: ${SW_CLUSTER_K8S_UID:SKYWALKING_COLLECTOR_UID}

   # consul 集群模式
   consul:
     # 服务名称
     serviceName: ${SW_SERVICE_NAME:"SkyWalking_OAP_Cluster"}
     # consul 连接地址，多个地址使用逗号分隔
     hostPort: ${SW_CLUSTER_CONSUL_HOST_PORT:localhost:8500}

# 核心配置
core:
  default:
    # role 定义 oap 的工作模式，即能够接收哪些上报数据
    # 默认的 Mixed 模式接收所有类似的上报数据(包括 agent、一级聚合节点、二级聚合节点)
    # Mixed: Receive agent data, Level 1 aggregate, Level 2 aggregate
    # Receiver: Receive agent data, Level 1 aggregate
    # Aggregator: Level 2 aggregate
    role: ${SW_CORE_ROLE:Mixed} # Mixed/Receiver/Aggregator
    # 少量不支持 gPRC 的 agent 使用 rest 上报数据
    # UI 使用 rest 访问数据
    # rest 服务绑定 IP
    restHost: ${SW_CORE_REST_HOST:0.0.0.0}
    # rest 服务绑定端口
    restPort: ${SW_CORE_REST_PORT:12800}
    # rest 服务访问地址前缀(上下文)
    restContextPath: ${SW_CORE_REST_CONTEXT_PATH:/}
    # 使用 gPRC 能带来更好的性能（大多 agent 使用该模式）
    # gRPC 绑定 IP
    gRPCHost: ${SW_CORE_GRPC_HOST:0.0.0.0}
    # gRPC 绑定端口
    gRPCPort: ${SW_CORE_GRPC_PORT:11800}
    # 采样汇总统计维度
    downsampling:
      - Hour
      - Day
      - Month
    # Set a timeout on metrics data.
    # After the timeout has expired, the metrics data will automatically be deleted.
    recordDataTTL: ${SW_CORE_RECORD_DATA_TTL:90} # Unit is minute
    minuteMetricsDataTTL: ${SW_CORE_MINUTE_METRIC_DATA_TTL:90} # Unit is minute
    hourMetricsDataTTL: ${SW_CORE_HOUR_METRIC_DATA_TTL:36} # Unit is hour
    dayMetricsDataTTL: ${SW_CORE_DAY_METRIC_DATA_TTL:45} # Unit is day
    monthMetricsDataTTL: ${SW_CORE_MONTH_METRIC_DATA_TTL:18} # Unit is month

# 存储配置
storage:

  # 使用最多最普遍的方式是 ES
  elasticsearch:
    # 该命名空间要与 ES 配置文件中的命名空间保持一致
    nameSpace: ${SW_NAMESPACE:""}
    # ES 连接地址，多个地址用逗号分隔
    clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:localhost:9200}
    # 用户名
    user: ${SW_ES_USER:""}
    # 密码
    password: ${SW_ES_PASSWORD:""}
    # 索引分片数量，增加分片数量，能降低每个分片的大小，加快查询速度
    # 但如果分片数量过多，不能均匀分布到各个 ES 节点，势必造成查询时的资源竞争
    # 因此建议分片数量不大于 ES 集群节点数，每个分片数据量不超过 30G 即可
    indexShardsNumber: ${SW_STORAGE_ES_INDEX_SHARDS_NUMBER:2}
    # 索引备份数量，考虑容灾需要进行配置，过大会增加 ES 存储压力
    indexReplicasNumber: ${SW_STORAGE_ES_INDEX_REPLICAS_NUMBER:0}
    # 批处理设置, https://www.elastic.co/guide/en/elasticsearch/client/java-api/5.5/java-docs-bulk-processor.html
    # 可适当降低对 ES 的读写压力
    bulkActions: ${SW_STORAGE_ES_BULK_ACTIONS:2000} # Execute the bulk every 2000 requests
    bulkSize: ${SW_STORAGE_ES_BULK_SIZE:20} # flush the bulk every 20mb
    flushInterval: ${SW_STORAGE_ES_FLUSH_INTERVAL:10} # flush the bulk every 10 seconds whatever the number of requests
    # A value of 0 means that only a single request will be allowed to be executed.
    # A value of 1 means 1 concurrent request is allowed to be executed while accumulating new bulk requests.
    # 该值越大吞吐量越高，但对 ES 的压力会增大，可以初始值设置为 1 ，如果 ES 压力不大，逐步上调
    concurrentRequests: ${SW_STORAGE_ES_CONCURRENT_REQUESTS:2} # the number of concurrent requests
    # 设置 metadata 最大查询数量
    metadataQueryMaxSize: ${SW_STORAGE_ES_QUERY_MAX_SIZE:5000}
    # 设置 segment 最大查询数量
    segmentQueryMaxSize: ${SW_STORAGE_ES_QUERY_SEGMENT_SIZE:200}

  # h2 不适合应用于生产环境，仅供开发测试环境使用
  h2:
    driver: ${SW_STORAGE_H2_DRIVER:org.h2.jdbcx.JdbcDataSource}
    url: ${SW_STORAGE_H2_URL:jdbc:h2:mem:skywalking-oap-db}
    user: ${SW_STORAGE_H2_USER:sa}
    metadataQueryMaxSize: ${SW_STORAGE_H2_QUERY_MAX_SIZE:5000}

  # 对 mysql 的支持情况（待调研）
  mysql:
    metadataQueryMaxSize: ${SW_STORAGE_H2_QUERY_MAX_SIZE:5000}



# Receiver 共享配置
# 定义所有 Receiver 的监听地址(如果为空则使用 core 的配置)
# 注意：如果进行了覆盖，必须确保与 core 配置地址不同，因为 UI 和 OAP 组件内部通讯仍需要基于 core 配置的地址
receiver-sharing-server:
  default:
    restHost: ${SW_SHARING_SERVER_REST_HOST:0.0.0.0}
    restPort: ${SW_SHARING_SERVER_REST_PORT:12800}
    restContextPath: ${SW_SHARING_SERVER_REST_CONTEXT_PATH:/}
    gRPCHost: ${SW_SHARING_SERVER_GRPC_HOST:0.0.0.0}
    gRPCPort: ${SW_SHARING_SERVER_GRPC_PORT:11800}

# 接收服务/端点注册数据
receiver-register:
  default:

# 接收链路追踪数据
receiver-trace:
  default:
    bufferPath: ${SW_RECEIVER_BUFFER_PATH:../trace-buffer/}  # Path to trace buffer files, suggest to use absolute path
    bufferOffsetMaxFileSize: ${SW_RECEIVER_BUFFER_OFFSET_MAX_FILE_SIZE:100} # Unit is MB
    bufferDataMaxFileSize: ${SW_RECEIVER_BUFFER_DATA_MAX_FILE_SIZE:500} # Unit is MB
    bufferFileCleanWhenRestart: ${SW_RECEIVER_BUFFER_FILE_CLEAN_WHEN_RESTART:false}
    # 采样率，适当降低采样率能够减轻 ES 压力
    sampleRate: ${SW_TRACE_SAMPLE_RATE:10000} # The sample rate precision is 1/10000. 10000 means 100% sample in default.
    # 定义超过多少 ms 就判定为 slowDBAccess
    slowDBAccessThreshold: ${SW_SLOW_DB_THRESHOLD:default:200,mongodb:100} # The slow database access thresholds. Unit ms.

# 接收 JVM 度量数据
receiver-jvm:
  default:
# 接收 .NET clr 度量数据
receiver-clr:
  default:

# 接收 service-mesh 数据
service-mesh:
  default:
    bufferPath: ${SW_SERVICE_MESH_BUFFER_PATH:../mesh-buffer/}  # Path to trace buffer files, suggest to use absolute path
    bufferOffsetMaxFileSize: ${SW_SERVICE_MESH_OFFSET_MAX_FILE_SIZE:100} # Unit is MB
    bufferDataMaxFileSize: ${SW_SERVICE_MESH_BUFFER_DATA_MAX_FILE_SIZE:500} # Unit is MB
    bufferFileCleanWhenRestart: ${SW_SERVICE_MESH_BUFFER_FILE_CLEAN_WHEN_RESTART:false}
# 接收 istio 遥测数据
istio-telemetry:
  default:
# 接收 envoy 度量数据
envoy-metric:
  default:

# 接收 zipkin 度量数据（支持度不高，不能在生产环境启用分析特性）
receiver_zipkin:
  default:
    host: ${SW_RECEIVER_ZIPKIN_HOST:0.0.0.0}
    port: ${SW_RECEIVER_ZIPKIN_PORT:9411}
    contextPath: ${SW_RECEIVER_ZIPKIN_CONTEXT_PATH:/}
    needAnalysis: false
# 接收 jaeger 度量数据（支持度不高，仅支持 Tracing Mode）
receiver_jaeger:
  default:
    gRPCHost: ${SW_RECEIVER_JAEGER_HOST:0.0.0.0}
    gRPCPort: ${SW_RECEIVER_JAEGER_PORT:14250}

# 
query:
  graphql:
    path: ${SW_QUERY_GRAPHQL_PATH:/graphql}

# 告警配置，具体在 `alarm-settings.yml` 进行配置
alarm:
  default:

# 开放 OAP 遥测端口，供 Prometheus 采集数据
telemetry:
  prometheus:
    host: ${SW_TELEMETRY_PROMETHEUS_HOST:0.0.0.0}
    port: ${SW_TELEMETRY_PROMETHEUS_PORT:1234}

# Prometheus exporter
exporter:
  grpc:
    targetHost: ${SW_EXPORTER_GRPC_HOST:127.0.0.1}
    targetPort: ${SW_EXPORTER_GRPC_PORT:9870}
```

#### 最佳实践

- 所有配置项都可以通过环境变量覆盖，这给容器化启动的动态配置带来了便利；
- 所有配置项都可以通过传参进行覆盖，例如：`-Dcore.default.restHost=172.0.4.12`；
- 多实例并发操作 ES 时，为避免 ES 操作阻塞，启动时请使用 Init Mode (oapServiceInit.sh)，
  该模式会只启动一个示例做数据初始化工作，初始化完成后，该 Init 实例退出，启动多个真正的工作实例；
- 生产环境建议配置为集群模式，以提供高吞吐量和 HA 支持


#### 附加信息

- OAP 支持三种启动模式：Default/Init/no-init
  * Default Mode：初始化 -> 运行 (/bin/oapService.sh、startup.sh)；
  * Init Mode：单实例初始化 -> 运行 (/bin/oapServiceInit.sh)；
  * No-init Mode：直接运行 (/bin/oapServiceNoInit.sh)；