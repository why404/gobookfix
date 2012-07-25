# HTTP 编程

本节内容：

* 简介
* HTTP客户端
	- 基本方法
		- http.Get
		- http.Head
		- http.Post
		- http.PostForm
		- (*http.Client).Do
	- 高级封装
		- 自定义 http.Client
		- 自定义 http.Transport
		- 灵活的 http.RoundTripper 接口
	- 设计优雅的 HTTP Client
* HTTP服务端
    - 处理 http 请求
    - 处理 https 请求


**(*http.Client).Do**

在多数情况下，http.Get、http.PostForm 就可以满足需求。但是如果我们发起的 http 请求需要更多的定制性，我们希望设定一些自定义的 Http Header 字段，比如：

* 设定自定义的 User-Agent，而不是默认的 "Go http package"
* 传递 Cookies
* …

此时可以使用 net/http 包 http.Client 对象的 Do 方法来实现：

    req, err := http.NewRequest("GET", "http://example.com", nil)
    // ...
    req.Header.Add("User-Agent", "Gobook Custom User-Agent")
    // ...
    client := &http.Client{ //... }
    resp, err := client.Do(req)
    // ...

### 高级封装

**自定义 http.Client**

前面我们使用的 http.Get、http.Post、http.PostForm 和 http.Head 方法其实都是在 http.DefaultClient 基础上进行调用的，比如 http.Get 等价于 http.DefaultClient.Get，依此类推。