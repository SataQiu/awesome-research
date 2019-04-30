# Spring Cloud Gateway

Spring Cloud Gateway 是基于 Spring MVC 实现的 API 网关，它提供一种简单又高效的方法将请求路由到 API。
此外，Spring Cloud Gateway 能够提供诸如：安全、监控/度量、弹性等附加功能。

网关特性：

- Built on Spring Framework 5, Project Reactor and Spring Boot 2.0
- Able to match routes on any request attribute.
- Predicates and filters are specific to routes.
- Hystrix Circuit Breaker integration.
- Spring Cloud DiscoveryClient integration
- Easy to write Predicates and Filters
- Request Rate Limiting
- Path Rewriting

## QuickStart

1.  [在线生成初始 SpringBoot 项目](https://start.spring.io/)
1.  生成的项目为 Maven 格式，使用 IDE 导入
1.  编辑 pom.xml

    ```xml
        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-dependencies</artifactId>
                    <version>Greenwich.SR1</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
            </dependencies>
        </dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-gateway</artifactId>
            </dependency>
        </dependencies>

        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
    ```

1.  编辑 Application.java

    ```java

    import reactor.core.publisher.Mono;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.cloud.gateway.route.RouteLocator;
    import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
    import org.springframework.context.annotation.Bean;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    @SpringBootApplication
    @RestController
    public class Application {

        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

        @Bean
        public RouteLocator myRoutes(RouteLocatorBuilder builder) {
            String httpUri = "http://httpbin.org:80";
            return builder.routes()
                    .route(p -> p.path("/get")
                    .filters(f -> f.addRequestHeader("Hello", "World"))
                    .uri(httpUri)).build();
        }

        @RequestMapping("/fallback")
        public Mono<String> fallback() {
            return Mono.just("fallback");
        }
    }
    ```

2.  运行 Application.java
3.  访问 `http://localhost:8081/get`

    ```sh
    {
    "args": {}, 
    "headers": {
        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3", 
        "Accept-Encoding": "gzip, deflate, br", 
        "Accept-Language": "zh-CN,zh;q=0.9", 
        "Cache-Control": "max-age=0", 
        "Forwarded": "proto=http;host=\"localhost:8081\";for=\"0:0:0:0:0:0:0:1%0:35628\"", 
        "Hello": "World", 
        "Host": "httpbin.org", 
        "Upgrade-Insecure-Requests": "1", 
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36", 
        "X-Forwarded-Host": "localhost:8081"
    }, 
    "origin": "0:0:0:0:0:0:0:1%0, 45.57.247.6, 45.57.247.6", 
    "url": "https://localhost:8081/get"
    }
    ```

附项目地址：https://github.com/SataQiu/springcloudgateway-demo
