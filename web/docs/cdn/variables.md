# 内置变量
## 变量形式
在使用变量的地方，使用`${varName}`来表示变量，比如：
~~~
/${1}/hello${requestPath}?name=${arg.name}
~~~

## 内置变量列表
### 请求相关变量
* `${cloudVersion}` - 边缘节点的版本
* `${remoteAddr}` - 客户端地址（IP），会依次根据X-Forwarded-For、X-Real-IP、RemoteAddr获取，适合前端有别的反向代理服务时使用；如果前端没有别的反向代理，则存在被伪造的风险
* `${rawRemoteAddr}` - 客户端地址（IP），返回直接连接服务的客户端原始IP地址，如果前端有别的反向代理，可能会返回前端反向代理的地址，而不是真实的客户端地址
* `${remotePort}` - 客户端端口
* `${remoteUser}` - 客户端用户名
* `${requestId}` - 请求ID，类似于`1638414954148000001`
* `${requestURI}` - 请求URI，比如`/hello?name=lily`
* `${requestPath}` - 请求路径（不包括参数），比如`/hello`
* `${requestURL}` - 完整的请求URL，比如`https://example.com/hello?name=lily`
* `${requestPathExtension}` - 请求路径中的文件扩展名，包括点符号，比如`.html`、`.png`
* `${requestPathLowerExtension}` 请求路径中的文件扩展名，其中大写字母会被自动转换为小写，包括点符号，比如`.html`、`.png`
* `${requestLength}` - 请求内容长度
* `${requestMethod}` - 请求方法，比如`GET`、`POST`
* `${requestFilename}` - 请求文件路径
* `${scheme}` - 请求协议，`http`或`https`
* `${proto}` - 包含版本的HTTP请求协议，类似于`HTTP/1.0`
* `${timeISO8601}` - ISO 8601格式的时间，比如`2018-07-16T23:52:24.839+08:00`
* `${timeLocal}` - 本地时间，比如`17/Jul/2018:09:52:24 +0800`
* `${msec}` - 带有毫秒的时间，比如`1531756823.054`
* `${timestamp}` - unix时间戳，单位为秒
* `${host}` - 主机名，通常是请求的域名
* `${cname}` - 当前网站的CNAME，比如38b48e4f.example.com
* `${host.first}` - 主机名第一段，比如`www.example.com`的值为`www`
* `${host.last}` - 主机名最后一段，比如`www.example.com`的值为`com`
* `${host.N}` - 主机名的第几段，从0开始，N最大为4，即`${host.0}`、`${host.1}`...`${host.4}`
* `${host.-N}` - 主机名的倒数第几段，从-1开始，比如对于`www.example.com`的`${host.-1}`值为`com`，N最大为5
* `${serverName}` - 接收请求的服务器名
* `${serverAddr}` - 服务器地址（IP），需要管理员设置启用后才会生效
* `${serverPort}` - 接收请求的服务器端口
* `${referer}` - 请求来源URL
* `${referer.host}` - 请求来源URL域名
* `${userAgent}` - 客户端信息
* `${contentType}` - 请求头部的Content-Type
* `${request}` - 构造类似于"GET / HTTP/1.1"之类的请求字符串  
* `${cookies}` - 所有cookie组合字符串
* `${cookie.NAME}` - 单个cookie值，比如`${cookie.sid}`
* `${isArgs}` - 问号（?）标记，如果URL有参数，则值为`?`；否则，则值为空
* `${args}` - 所有URL参数组合字符串
* `${arg.NAME}` - 单个URL参数值，比如`${arg.name}`
* `${headers}` - 所有Header信息组合字符串
* `${header.NAME}` - 单个请求Header值，比如`${header.User-Agent}`
* `${documentRoot}` - 当前请求的文档根目录

#### 地区相关
* `${geo.country.name}` - 国家/地区名称
* `${geo.country.id}` - 国家/地区ID
* `${geo.province.name}` - 省份名称（目前只包含中国省份）
* `${geo.province.id}` - 省份ID（目前只包含中国城市）
* `${geo.city.name}` - 城市名称（目前只包含中国城市）
* `${geo.city.id}` - 城市ID（目前只包含中国城市）
* `${geo.town.name}` - 县级单位名称（目前只包含中国县级单位）
* `${geo.town.id}` - 县级单位ID（目前只包含中国县级单位）

#### ISP相关
* `${isp.name}` - ISP服务商名称
* `${isp.id}` - ISP服务商ID

#### 浏览器相关
* `${browser.os.name}` - 客户端所在操作系统名称
* `${browser.os.version}` - 客户端所在操作系统版本
* `${browser.name}` - 客户端浏览器名称
* `${browser.version}` - 客户端浏览器版本
* `${browser.isMobile}` - 手机标识，如果客户端是手机，则值为1

#### 产品相关
* `${product.name}` - 产品名，可以在"系统设置"--"管理界面设置"中设置
* `${product.version}` - 产品版本，可以在"系统设置"--"管理界面设置"中设置

### 响应相关变量
* `${requestTime}` - 请求花费时间
* `${bytesSent}` - 发送的内容长度，包括Header（字节）
* `${bodyBytesSent}` - 发送的内容长度，不包括Header（字节）
* `${status}` - 状态码，比如`200`
* `${statusMessage}` - 状态消息，比如`200 OK`
* `${response.contentType}` - 响应的`Content-Type`值  
* `${response.header.NAME}` - 响应中的Header值

### 源站相关变量

> 注意：在缓存Key中不能使用源站相关变量，因为读取缓存是在读取源站之前执行的。

* `${origin.id}` - 源站服务器ID
* `${origin.code}` - 源站服务器代号
* `${origin.address}` - 源站服务器地址，包含主机地址和端口
* `${origin.host}` - 源站服务器地址，只包含主机地址
* `${origin.scheme}` - 源站服务器协议

### 缓存相关变量
* `${cache.status}` - 缓存状态，值可能为：
  * `BYPASS` - 没有开启缓存策略或者其他原因未通过缓存策略处理的时候，状态为`BYPASS`
  * `HIT` - 已命中缓存
  * `MISS` - 未命中缓存
  * `PURGE` - 正在清除缓存
  * `UPDATING` - 正在更新缓存
  * `EXPIRED` - 已过期
  * `STALE` - 正在使用过期的缓存
* `${cache.age}` - 缓存对象的有效期
* `${cache.policy.id}` - 缓存策略ID	
* `${cache.policy.name}` - 缓存策略名称
* `${cache.policy.type}` - 缓存策略类型（memory、file之类） 
* `${cache.key}` - 当前请求的缓存Key

## 修饰符
变量中可以加入修饰符，用于对变量值再次进行处理：
~~~
${varName|修饰符1|修饰符2|更多修饰符...}
~~~
目前支持以下几个修饰符：
* `urlEncode` - 对变量值进行URL编码
* `urlDecode` - 对变量值进行URL解码
* `base64Encode` - 对变量值进行Base64编码
* `base64Decode` - 对变量值进行Base64解码
* `md5` - 对变量值进行md5编码
* `sha1` - 对变量值进行sha1编码
* `sha256` - 对变量值进行sha256编码
* `toLowerCase` - 转换为小写
* `toUpperCase` - 转换为大写

示例：
~~~
${requestPath|urlEncode}               // "/hello/world" => "%2Fhello%2Fworld"
${arg.name|base64Encode|md5}           // "Lily" => "0374cb368f88b84dfb57bb883cca02ec"
~~~

## 匹配变量
匹配变量值的是有正则表达式的地方，使用匹配结果，通常为一个从0开始的数字，比如在重写规则中：
~~~
/(\w+)/(\w+)
~~~
那么
* `${0}` - 表示整体匹配的内容
* `${1}` - 表示第一个括号匹配的内容
* `${2}` - 表示第二个括号匹配的内容

可以使用`(?i)`来设置不区分大小写：
~~~
(?i)/index.php
~~~

更多可用的正则表达式可以参考 [RE2 Syntax](https://github.com/google/re2/wiki/Syntax)。

### 命名变量
也可以给变量设置一个名称：
~~~
/(?P<myName>\w+)
~~~
然后就可以在待替换字符串中使用 `${myName}` ：
~~~
/hello/${myName}
~~~