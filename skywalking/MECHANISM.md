# 工作机理

SkyWalking 通过 JavaAgent 机制，对需要拦截的类的方法，使用 `byte-buddy` 动态修改 Java 类的二进制，从而进行方法切面拦截，记录调用链路。
