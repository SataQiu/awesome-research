# SkyWalking 客户问题实验记录

### 1. ES 重启导致 SW 无法正常工作？

##### 1.1 实验环境

操作系统 Ubuntu:18.04 amd64

SW 版本：5.0.0-GA

ES 版本：5.6.4

##### 1.2 实验关键配置

SkyWalking 连接 ES 配置：
```yaml
storage:
  elasticsearch:
    clusterName: elasticsearch
    clusterTransportSniffer: true
    clusterNodes: 10.20.14.224:9300
    indexShardsNumber: 2
    indexReplicasNumber: 0
    highPerformanceMode: true
    bulkActions: 1
    bulkSize: 1
    flushInterval: 5
    concurrentRequests: 2
```

##### 1.3 实验记录

| 实验编号 | 实验步骤 | 结果描述 | 结论 |
| ------ | ------ | ------ | ----- |
| 01 | 首先关闭 ES，间隔任意长时间后启动 ES | SW 在 ES 关停后停止服务，ES 正常后恢复服务，ES 关停期间的数据丢失 | 正常 |
| 02 | 首先关闭 ES，然后重启微服务应用程序，间隔任意长时间后启动ES | SW 在 ES 关停后停止服务，ES 正常后恢复服务，ES 关停期间的数据丢失 | 正常 |
| 03 | 首先关闭 SW，间隔任意长时间后启动 SW | SW 关停期间数据无法采集，重启后恢复数据采集 | 正常 |
| 04 | 首先关闭 SW，然后重启微服务应用程序，间隔任意长时间后启动 SW | SW 关停期间数据无法采集，重启后恢复数据采集 | 正常 |
| 05 | 首先关闭微服务应用程序，间隔任意长时间后启动微服务应用程序 | 微服务关闭后没有数据上报，重新开启后恢复数据上报 | 正常 |
| 06 | 首先关闭微服务应用程序，然后重启 SW，间隔任意长时间后启动微服务应用程序 | SW 运行正常，微服务关闭后没有数据上报，重新开启后恢复数据上报 | 正常 |
