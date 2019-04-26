# 源码编译

**本文基于官方推荐的 Git 克隆方式进行 SkyWalking 源码编译**

### 编译步骤

1.  安装 git、JDK8 以及 maven3
1. `git clone https://github.com/apache/skywalking.git`
1. `cd skywalking/`
1. `git checkout v6.0.0-GA`
1. `git submodule init`
1. `git submodule update`
1. `./mvnw clean package -DskipTests`
1. `/dist` 内会生成编译结果