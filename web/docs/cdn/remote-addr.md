# 访客IP地址
可以在网站"设置"--"访客IP地址"中设置访客的IP地址获取方式。

## 影响范围
访客IP地址设置会应用于记录访问日志、检查IP黑白名单、WAF地区封禁、IP并发限制等处。

## 选项说明
### 直接获取
用户直接访问边缘节点，即 "用户 --> 边缘节点" 模式，这时候可以直接从连接中读取到真实的IP地址。

### 从上级代理中获取
用户和边缘节点之间有别的代理服务转发，即 "用户 --> `[第三方代理服务]` --> 边缘节点"，这时候只能从上级代理中获取传递的IP地址。

用户首先访问的第三方代理服务，比如Nginx或者Squid之类的反向代理服务，再通过这些服务转发到${DocSystemName}的边缘节点，边缘节点上可以自动从 `X-Real-IP` 和 `X-Forwarded-For` 两个报头中获取用户的真实IP。

### 从请求报头中读取
可以填写单个或多个请求报头，用于从中读取真实的客户端IP：
* 单个请求报头示例：`X-Real-IP`、`X-Client-IP`、`X-My-IP`
* 多个请求报头示例（使用英文逗号分隔）：`X-Real-IP,X-Client-IP`

### 自定义变量
可以自定义变量，在 [这里](variables.md) 可以查看支持的请求变量。

## 错误处理
如果从选项或者变量值获得的IP地址格式是错误的（非IPv4和IPv6格式），则自动使用直接连接边缘节点的连接IP地址。

## 第三方代理示例
### 从Nginx传递客户端IP地址到边缘节点
如果Nginx处在${DocSystemName}的下游（就是"用户" --> Nginx --> ${DocSystemName}），可以在Nginx中使用 `X-Real-IP` 报头将获取的IP地址传递给${DocSystemName}:
~~~nginx
server {
	...
	
	location / {
       proxy_pass http://127.0.0.1:8005; # proxy_pass $${DocSystemName}节点地址
       proxy_set_header X-Real-IP $remote_addr; # 设置 X-Real-IP
       proxy_http_version 1.1; # 设置HTTP/1.1
       proxy_set_header Connection 'keep-alive'; # 设置保持连接
	}
}
~~~

## 源站读取IP
默认地，边缘节点会传递 `X-Real-IP` 和 `X-Forwarded-For` 两个报头到源站，其中`X-Real-IP`只可能为单个IP，而 `X-Forwarded-For`中可能包含有多个IP：
~~~
# X-Forwarded-For语法
X-Forwarded-For: <client>, <proxy1>, <proxy2>

# 示例
X-Forwarded-For: 192.168.2.100, 192.168.2.1, 127.0.0.1
~~~
从上面示例中可以看出，要获取`X-Forwarded-For`中的IP值需要分隔多个IP并读取第一个。


### PHP读取客户端IP示例
~~~php
$_SERVER["HTTP_X_FORWARDED_FOR"]
$_SERVER["HTTP_X_REAL_IP"]
~~~

### Go读取客户端IP示例
~~~go
req.Header.Get("X-Forwarded-For")
req.Header.Get("X-Real-Ip") // 注意这里的Ip格式
~~~
