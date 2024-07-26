# dlv

**场景一**：线上服务未开启 pprof 功能

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

通过 `top` 命令找到 CPU 占用高的进程 ID：

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

然后，通过 `top -H -p 16901` 查看进程内的线程，找到 CPU 占用高的线程：

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

这里看到，占用 CPU 较高的线程 ID 是 16903

使用 dlv attach go 程序

```
dlv attach 16901
```

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

输入 help 可以查看使用帮助，这里使用 goroutines 命令，查看协程信息：

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

可以看出，16903 正在执行 6 号协程，`goroutine 6`切换到对应协程

```
goroutine 6
```

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

接着，通过 `bt` 打印栈信息

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

确认是 main.go:29 的 main.Eat 占用 CPU 资源

通过 list 查看相关代码

```
list main.Eat
```

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

main.go 的第 29 行，就是 for 循环
