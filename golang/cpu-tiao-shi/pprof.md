# pprof

**场景一**：线上服务开启了 pprof 功能

运行如下程序，模拟线上服务：

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"runtime"

	_ "net/http/pprof"
)

func main() {
	// 限制 CPU 使用数
	runtime.GOMAXPROCS(1)
	// 启动 pprof server
	go func() {
		if err := http.ListenAndServe(":6060", nil); err != nil {
			log.Fatal(err)
		}
	}()
	// 模拟 CPU 高占用
	go Eat()

	select {}
}

func Eat() {
	fmt.Println("Eat CPU")
	for {
	}
}
```

```bash
go run main.go
```

通过如下命令采集 pprof 信息：

```bash
go tool pprof http://localhost:6060/debug/pprof/profile
```

上述命令需要等待一段时间，然后自动转入交互模式：

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>debug cpu by pprof</p></figcaption></figure>

如上图所述，通过执行 top 查看占用 CPU 最高的部分，通过 list 查看具体代码

如果不能直接定位到高 CPU 占用代码，可以通过 traces 顺藤摸瓜

例如，我们改写代码，如下：

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"runtime"

	_ "net/http/pprof"
)

func main() {
	// 限制 CPU 使用数
	runtime.GOMAXPROCS(1)
	// 启动 pprof server
	go func() {
		if err := http.ListenAndServe(":6060", nil); err != nil {
			log.Fatal(err)
		}
	}()
	// 模拟 CPU 高占用
	go func() {
		for {
			fmt.Println("for loop")
		}
	}()

	select {}
}

```

那么调试结果如下图所示，发现 top 占用是系统调用：

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>debug cpu top by traces</p></figcaption></figure>

此时不好直接定位出问题代码，可以通过 traces 查看

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>debug cpu top by traces 2</p></figcaption></figure>

可以明显看出，是 main.main.func2 最终调用了 syscall.syscall，具体查看代码

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>debug cpu top by traces 3</p></figcaption></figure>

最终定位出 CPU 占用大的代码是 `fmt.Println("for loop")`



