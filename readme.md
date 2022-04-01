# _dingding 钉钉 API 集成

## 安装

```
go get github.com/gozelle/_dingding
```

## 群机器人

**特性：**
* 使用签名校验方式；
* 自动合并多条消息发送，避免 API 调用限流。

```go
package main

import (
	"fmt"
	"github.com/gozelle/_dingding"
	"time"
)

func main() {
	
	robot := _dingding.NewRobot(&_dingding.Config{
		Title:   "监控报警消息",
		Webhook: "...",
		Secret:  "...",
	})
	
	go func() {
		for {
			robot.Listen() // 线程异步接收消息
		}
	}()
	
	// 第一个消息收到时会立马发送
	robot.Push(_dingding.Markdown{
		Title: "第一个消息",
		Text:  fmt.Sprintf("第一行: %d  \n第二行: %d", 1, 2),
	})
	
	// 第二个消息收到时会再 6 秒后合并发送
	robot.Push(_dingding.Markdown{
		Title: "第二个消息",
		Text:  fmt.Sprintf("第一行: %d  \n第二行: %d", 1, 2),
	})
	
	time.Sleep(7 * time.Second)
}
```


