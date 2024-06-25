# 实验记录

## 1. ES 重启导致 SW 无法正常工作？

### 1.1 实验环境

操作系统 Ubuntu:18.04 amd64

SW 版本：5.0.0-GA

ES 版本：5.6.4

### 1.2 实验关键配置

SkyWalking 连接 ES 配置示例：

```yaml
storage:
  elasticsearch:
    clusterName: elasticsearch
    clusterTransportSniffer: true
    clusterNodes: 10.20.14.224:9300
    indexShardsNumber: 2
    indexReplicasNumber: 0
    highPerformanceMode: true
    bulkActions: 2000
    bulkSize: 20
    flushInterval: 10
    concurrentRequests: 2
    traceDataTTL: 90
    minuteMetricDataTTL: 90
    hourMetricDataTTL: 36
    dayMetricDataTTL: 45
    monthMetricDataTTL: 18
```

### 1.3 实验记录

| 实验编号 | 实验步骤                                                            | 结果描述                                    | 结论 |
| ---- | --------------------------------------------------------------- | --------------------------------------- | -- |
| 01   | 首先关闭 ES，间隔任意长时间后启动 ES                                           | SW 在 ES 关停后停止服务，ES 正常后恢复服务，ES 关停期间的数据丢失 | 正常 |
| 02   | 首先关闭 ES，然后重启微服务应用程序，间隔任意长时间后启动ES                                | SW 在 ES 关停后停止服务，ES 正常后恢复服务，ES 关停期间的数据丢失 | 正常 |
| 03   | 首先关闭 SW，间隔任意长时间后启动 SW                                           | SW 关停期间数据无法采集，重启后恢复数据采集                 | 正常 |
| 04   | 首先关闭 SW，然后重启微服务应用程序，间隔任意长时间后启动 SW                               | SW 关停期间数据无法采集，重启后恢复数据采集                 | 正常 |
| 05   | 首先关闭微服务应用程序，间隔任意长时间后启动微服务应用程序                                   | 微服务关闭后没有数据上报，重新开启后恢复数据上报                | 正常 |
| 06   | 首先关闭微服务应用程序，然后重启 SW，间隔任意长时间后启动微服务应用程序                           | SW 运行正常，微服务关闭后没有数据上报，重新开启后恢复数据上报        | 正常 |
| 07   | 首先关停微服务应用程序，接着关停 SW，清空 ES 数据（这里直接删掉 ES 容器，新建一个），启动 SW，启动微服务应用程序 | 之前的采集数据全部丢失，重新采集工作正常                    | 正常 |

### 注：如果微服务数量较多， ES 存储压力过载，建议修改 SW 的如下配置，定期清理过时的数据

如下这些参数适当调小，能够减少 ES 的存储负担

```yaml
storage:
  elasticsearch:
    # Set a timeout on metric data. After the timeout has expired, the metric data will automatically be deleted.
    traceDataTTL: 90 # Unit is minute
    minuteMetricDataTTL: 90 # Unit is minute
    hourMetricDataTTL: 36 # Unit is hour
    dayMetricDataTTL: 45 # Unit is day
    monthMetricDataTTL: 18 # Unit is month
```
