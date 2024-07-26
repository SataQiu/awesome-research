# dlv

**场景二**：线上服务未开启 pprof 功能

运行如下程序，模拟线上服务：

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	// 限制 CPU 使用数
	runtime.GOMAXPROCS(1)
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

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

然后，通过 `top -H -p 22706` 查看进程内的线程，找到 CPU 占用高的线程：

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

这里看到，占用 CPU 较高的线程 ID 是 22710

使用 dlv attach go 程序

```
dlv attach 22706
```

输入 help 可以查看使用帮助

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

这里使用 `goroutines` 命令，查看协程信息：

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

可以看出，22710 线程正在执行 5 号协程，`goroutine 5`切换到对应协程

```
goroutine 5
```

接着，通过 `stack` 或 `bt` 命令打印栈信息（注意这会下断点，打断程序执行，执行前做好线上隔离）

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

通过 `ls` 查看正在执行的代码（ls 也可以查看指的代码，如 `ls main.Eat`）

```
ls
```

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

很明显了，for 循环在持续占用 CPU。

最后，执行 `quit` 退出调试，注意选择 `n` 以不杀死进程

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

