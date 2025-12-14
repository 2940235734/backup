# HTTP报头
## 设置
可以在网站"设置" -- "HTTP报头"中添加自定义报头。

其中：
* `响应报头` - 发送给客户端的报头
* `请求报头` - 发送给源站的报头

添加时，请严格遵守大小写规范，比如 `Content-Type` 的以下写法都是**错误**的：
~~~
content-type
content_type
contentType
~~~

严重注意：在HTTP/2中，所有报头在浏览器端查看时可能显示的是小写，比如`content-type`，但是，在CDN系统添加的时候一定要严格地写成`Xxx-Yyy`类似的格式。

### 变量
可以在报头值中使用变量，具体可以使用的变量列表在[这里](variables.md)查看。

### 删除报头
如果想删除一个报头，也可以在这里添加"需要删除的报头"，同样注意大小写规范。

## 示例
### 传递IP地址
有时候需要使用一个自定义的报头（比如叫`X-Example-Forwarded-For`）向源站传递IP地址，可以添加一个报头：
~~~
X-Example-Forwarded-For: ${remoteAddr}
~~~

### 实现浏览器端跨域访问
在同个网页展示多个域名的资源时，比如`https://example.org`调用`https://example.com`，浏览器可能会出现类似于以下的错误提示：
~~~
Uncaught DOMException: Blocked a frame with origin "https://www.example.com" from accessing a cross-origin frame.
~~~
或者
~~~
Access to XMLHttpRequest at 'https://example.com/...' (redirected from 'https://example.org/...') from origin 'https://example.org' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
~~~

出现这些的错误提示后，当前网页可能无法正常加载。此时需要在被调用的域名（比如上面示例中的`https://example.com`）网站中增加[CORS](cors.md)相关报头。