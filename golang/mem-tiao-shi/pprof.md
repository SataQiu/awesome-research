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
	// 模拟 Memory 高占用
	go func() {
		buf := Eat()
		fmt.Println(len(buf))
		select {}
	}()

	select {}
}

func Eat() []byte {
	fmt.Println("Eat Memory")
	var buf []byte
	for i := 0; i < 1000; i++ {
		buf = append(buf, make([]byte, 1024*1024)...)
	}
	return buf
}

```

```
go run main.go
```

通过如下命令采集 pprof 信息：

```
go tool pprof http://localhost:6060/debug/pprof/heap
```

上述命令执行后，自动转入交互模式，通过执行 `top` 查看占用 Memory 最高的部分：

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

通过 `list main.Eat` 查看具体代码：

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

可以看出，内存占用发生位置为 buf 创建位置。
