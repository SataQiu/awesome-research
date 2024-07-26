# dlv

**场景二**：线上服务未开启 pprof 功能

运行如下程序，模拟线上服务：

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	// 限制 CPU 使用数
	runtime.GOMAXPROCS(1)
	var buf []byte
	// 模拟 Memory 高占用
	go func() {
		buf = Eat()
		fmt.Println(len(buf))
	}()

	for {
		time.Sleep(time.Hour)
	}
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

使用 `top` 命令，按下 M 按照内存排序，找到内存占用进程 ID：

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

使用 dlv attach go 程序

```
dlv attach 40387
```
