# 加密解密

如何设计一套加密服务？需要一个类似 KMS 的加解密服务以及与之匹配的 SDK。

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>加解密流程示意图</p></figcaption></figure>

优点：

* 每个应用使用独立的加密密钥，即使单个泄漏不影响其它服务
* 通过 SDK 统一客户端和服务端的加解密流程，可通过微调著名算法实现自定义，增加破解难度
* tls 双向认证加密传输，避免中间人攻击

改进和说明项：

* 如果要实现对应用程序透明，在启动时自动解密，需要考虑对加密数据单独进行打标。比如通过`decrypt{{ data }}` 的方式标记 data 需要解密，客户端 SDK 识别后自动执行解密操作，应用层在使用的时候已经是解密后的数据了，没有感知到解密过程。
* AppID 和 客户端证书信息如何自动注入呢？肯定不能直接写到配置文件里，可以考虑环境变量注入。
* 后端存储密钥可以设计成分片存储模式，运行时通过算法动态组装。



如果想采用云原生方式实现 Secret 加密，可以考虑 Kamus 方案，通过 init container 解密数据

<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption><p>kamus</p></figcaption></figure>

