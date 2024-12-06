## Gin 笔记



### Gin 优势

简单原则

并发高

分配内存少



### 快速开始

安装 Gin



下载 Gin 框架



### 请求路由

#### 多种请求类型

代码示例：

> main.php

```go
package main

import "github.com/gin-gonic/gin"

func main()  {
	r := gin.Default()
  // get 请求
	r.GET("get", func(c *gin.Context) {
		c.String(200, "get")
	})
  // post 请求
	r.POST("post", func(c *gin.Context) {
		c.String(200, "post")
	})
  // delete 请求
	r.Handle("DELETE", "/delete", func(c *gin.Context) {
		c.String(200, "delete")
	})
  // any 请求
	r.Any("any", func(c *gin.Context) {
		c.String(200, "any")
	})
	r.Run()
}

```



#### 绑定静态文件夹

##### 代码示例

> /router_static/main.php

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main()  {
	r := gin.Default()
	// 静态文件绑定
	r.Static("/assets", "./assets")
	r.StaticFS("/static", http.Dir("static"))
	// 单个静态文件
	r.StaticFile("/favicon.ico", "./favicon.ico")
	r.Run()
}
```

> router_static/assets/a.html

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
a content
</body>
</html>
```

> router_static/static/b.html

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
b content
</body>
</html>
```

> 执行 shell

```shell
$ go build -o router_static && ./router_static
```



##### 测试

```shell
$ curl "http://127.0.0.1:8080/assets/a.html"
# 输出： a content

$ curl "http://127.0.0.1:8080/static/b.html"
# 输出： b content
```



#### 参数作为URL

##### 代码示例：

> router_uri/main.go

```go
package main

import "github.com/gin-gonic/gin"

func main()  {
	r:=gin.Default()
	r.GET("/:name/:id", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"name": c.Param("name"),
			"id": c.Param("id"),
		})
	})
	r.Run()
}
```



##### 测试

```shell
$ curl -X GET "http://127.0.0.1:8080/zhangsan/12"
# {"id":"12","name":"zhangsan"}
```



#### 泛绑定

##### 代码示例：

> router_generic/main.go

```go
package main

import "github.com/gin-gonic/gin"

func main()  {
	r:=gin.Default()
	r.GET("/user/*action", func(c *gin.Context) {
		c.String(200, "hello world")
	})
	r.Run()
}
```

##### 测试：

```shell
$ curl -X GET "http://127.0.0.1:8080/user/1111"
hello world
```



#### 获取 Get 参数

##### 代码示例：

> param_get/main.go

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main()  {
	r:=gin.Default()
	r.GET("/test", func(c *gin.Context) {
		firstName:=c.QueryArray("first_name")
		lastName:=c.DefaultQuery("last_name", "last_default_name")
		c.String(http.StatusOK, "%s, %s \n", firstName, lastName)
	})
	r.Run(":8080")
}
```

##### 测试：

```go
$ curl -X GET "http://127.0.0.1:8080/test?first_name=wang&last_name=kai"
[wang], kai
```



#### 获取 Body 内容

##### 代码示例：

> param_body/main.go

```go
package main

import (
	"github.com/gin-gonic/gin"
	"io/ioutil"
	"net/http"
)

func main()  {
	r:=gin.Default()
	r.POST("/test", func(c *gin.Context) {
		bodyBytes, err := ioutil.ReadAll(c.Request.Body)
		if err != nil {
			c.String(http.StatusBadRequest, err.Error())
			c.Abort()
		}
		//c.Request.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes))
		firstName := c.PostForm("first_name")
		lastName := c.DefaultPostForm("last_name", "default_last_name")

		c.String(http.StatusOK, "%s, %s, %s", firstName, lastName, string(bodyBytes))
		//c.String(http.StatusOK, string(bodyBytes))
	})
	r.Run(":8080")
}
```

##### 测试：

```shell
$ curl -X POST 'http://127.0.0.1:8080/test' -d '{"name":"wang"}'
{"name":"wang"}
```



#### 获取 bind 参数

代码示例

> param_struct/main.go

```go
package main

import (
	"github.com/gin-gonic/gin"
	"time"
)

type Person struct {
	Name     string    `form:"name"`
	Address  string    `form:"address"`
	Birthday time.Time `form:"birthday" time_format:"2006-01-02"`
}

func main() {
	r := gin.Default()
	r.GET("testing", testing)
	r.POST("testing", testing)
	r.Run()
}

func testing(c *gin.Context) {
	var person Person
	// 这里是根据请求 content-type 来做不同的 binding 操作
	if err := c.ShouldBind(&person); err == nil {
		c.String(200, "%v \n", person)
	} else {
		c.String(200, "person bind error：%v \n", err)
	}
}
```

##### 测试：

```shell
$ curl -X POST 'http://127.0.0.1:8080/test' -d '{"name":"wang"}'
{"name":"wang"}

$ curl -X POST 'http://127.0.0.1:8080/testing' -d 'name=wang&address=wuhan&birthday=2000-01-06'
{wang wuhan 2000-01-06 00:00:00 +0800 CST} 

$ curl -H "Content-Type:application/json" -X POST "http://127.0.0.1:8080//testing" -d '{"name":"wang"}'
{wang  0001-01-01 00:00:00 +0000 UTC} 
```



#### 结构体验证



##### 代码示例：

> valid_binding/main.go

```go
package main

import "github.com/gin-gonic/gin"

type Person struct {
	Age int `form:"age" binding:"required,gt=10"`
	Name string `form:"name" binding:"required"`
	Address string `form:"address" binding:"required"`
}


func main()  {
	r := gin.Default()
	r.GET("testing", func(c *gin.Context) {
		var person Person
		if err := c.ShouldBind(&person); err != nil {
			c.String(500, "%v", err)
			return
		}
		c.String(200, "%v", person)
	})

	r.Run()
}
```



##### 测试

```go
$ curl -X GET "http://127.0.0.1:8080/testing?age=12&name=wang&address=wuhan"
{12 wang wuhan}
```



#### 自定义 IP 中间件

> middleware_whitelist/main.go

```go
package main

import "github.com/gin-gonic/gin"

func IPAuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		ipList := []string{
			"127.0.0.2",
		}

		flag := false
		clientIp := c.ClientIP()
		for _,host := range ipList {
			if clientIp == host {
				flag = true
				break
			}
			if !flag {
				c.String(401, "%s, not in iplist", clientIp)
				c.Abort()
			}
		}
	}
}

func main() {
	r := gin.Default()
	r.Use(IPAuthMiddleware())
	r.GET("/test", func(c *gin.Context) {
		c.String(200, "hello test")
	})
	r.Run()
}

```



#### 测试：

```
$ curl -X GET "http://127.0.0.1:8080/test"
127.0.0.1, not in iplistb
```



