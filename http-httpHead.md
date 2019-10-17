---
title: http报文首部
date: 2019-09-17 11:24:57
tags: http
---

http首部字段根据实际用途，分为四种：

+ 通用首部字段，请求报文和响应报文都会用到的首部字段

+ 请求首部字段，

+ 响应首部字段

+ 实体首部字段，请求报文和响应报文实体部分实用的首部字段，补充了资源内容更新时间等等与实体有关的信息

## 通用首部字段

### cache-control

操作缓存的工作机制

请求指令

| 指令                  | 参数            | 说明
| -                     | -              | -
| no-cache              | 无             | 强制向源服务器再次验证
| no-store              | 无             | 不缓存请求或者响应的任何内容
| max-age = [秒]        | 必须            | 响应的最大Age值
| max-stale = [秒]      | 可以忽略         | 接收已经过期的响应
| min-fresh = [秒]      | 必须            | 期望在指定时间内的响应仍有效
| no-transform          | 无              | 代理不可更改媒体类型
| only-if-cached        | 无              | 从缓存获取资源
| cache-extension       | -               | 新指令标记

缓存响应指令

| 指令                    | 参数            | 说明
| -                       | -              | -
| public                  | 无              | 可以向任意方提供响应的缓存
| private                 | 可省略          | 仅向特定用户返回响应
| no-cache                | 可省略          |  缓存前必须先确认其有效性
| no-store                | 无              | 不缓存请求或者响应的任何内容
| no-transform            | 无              | 代理不可更改媒体类型
| must-revalidate         | 无              | 可缓存但是必须向源服务器再次确认
| proxy-revalidate        | 无              | 要求中间缓存服务器对缓存的响应有效性进行再次确认
| max-age = [秒]          | 必须              | 响应的最大Age值 
| s-maxage = [秒]         | 必须              | 公共缓存服务器响应的最大age值
| cache-extension         | -                | 新指令标记

表示是否能缓存的：

#### cache-control: public

表明其他用户也可以使用缓存

#### cache-control: private

缓存服务器只会对该特定的用户提供资源缓存的服务，对于其他用户发送过来的请求，缓存服务器不会返回缓存

#### cache-control: no-cache

客户端发送请求，包含no-cache指令，表示客户端将不会接收缓存过的响应，于是缓存服务器，必须把请求转发给源服务器

如果服务器返回的响应中设置cache-control: no-cache，那么缓存服务器不能对资源进行缓存，源服务器以后也不在对缓存服务器请求中提出的资源进行有效性确认，并且禁止对响应资源进行缓存

由服务器返回的响应中，如果报文字段cache-control: no-cache = loaction，具体指定了参数，那么客户端接收了这个被指定具体参数值的首部字段对应的响应报文的时候，就不能使用缓存，无参数的首部字段可以使用缓存，只能在响应报文中指定参数

控制可执行缓存的对象：

#### cache-control: no-store

缓存不能在本地存储请求或响应的任何部分，也就是不能进行任何缓存操作

指定缓存期限和认证：

#### cache-control: a-maxage = 604800

#### cache-control: max-age = 604800

客户端发送请求，包含cache-control: max-age = 604800，缓存资源的缓存数值比给定时间的数值小，客户端就接收缓存的资源

cache-control: max-age = 0，缓存服务器同城要把请求转发给源服务器

服务器返回的响应中包含cache-control: max-age = 604800，缓存服务器将不再对资源的有效性进行再次确认，max-age的值表示资源被保存为缓存的最长时间

#### cache-control: min-fresh = 60

要求返回再过60秒缓存时间还没到的资源

#### cache-control: max-stale = 3600

max-stale没有指定参数，无论资源过期多久，都会被客户端接收，如果设定了具体的值，资源过期了，但是还处于max-stale设定的时间以内，客户端仍然可以接收

#### cache-control: only-if-cache

客户端要求缓存服务器如果有请求的资源的缓存的情况下才会返回，如果缓存服务器无响应，返回504状态码

#### cache-control: must-revalidate

代理服务器会向源服务器验证即将返回的响应缓存是否仍然有效，如果代理服务器无法连通源服务器再次获取有效资源，则返回504状态码

cache-control: max-stale = 3600，失效

#### cache-control: proxy-revalidate

要求所有的缓存服务器，在接收到带有该指令的请求返回响应之前，必须再次验证缓存的有效性

#### cache-control: no-transform

无论实在请求中还是响应中，缓存都不能改变实体主体的媒体类型，可以防止缓存或者代理服务器压缩图片等类似操作

### Connection

Connection首部字段的作用：

+ 控制不在转发给代理的首部字段

+ 管理持久链接

### Date

表示创建http报文的日期和时间

### Pragma

pragma: no-cache, 用于客户端发送请求，表示客户端不要缓存来的资源

### Trailer

事先说明在报文的主体后面记录了那些首部字段，该首部字段可以用在http/1.1版本分块传输编码时

### Transfer-Encoding

规定传输报文主体时的编码方式

HTTP/1.1的传输编码方式，仅对分块传输编码有效

### Upgrade

检测HTTP以及其他协议是否可以使用更高的版本进行通信，其参数值可以用来指定一个完全不同的通信协议，于connection配合使用

``` js
Upgrade: TSL/1.0
connection: Upgrade
```

仅限于客户端和邻接服务器之间起作用，

对于有Upgrade首部字段的请求，服务器端可以使用101 Switching Protocols状态码作为响应

### Via

跟踪客户端与服务器之间的请求和响应报文的传输路径

报文经过代理和网关时，会在首部字段Via中附加该服务器的信息，然后在进行转发，不仅可以用来追踪报文的转发，还可以避免请求回环的发生，

### Warning

用于告知用户与缓存相关的问题的警告，格式： 

```js
Warning: [警告码] [警告的主机: 端口号] "[警告的内容]" ([日期时间])
```

## 请求首部字段

### Accept

通知服务器，用户代理能够处理的媒体类型以及媒体类型的相对优先级，格式：type/subType，以逗号间隔，一次指定多种格式

可以使用q=..这种格式为媒体类型指定权重，权重值取值范围为0-1，1为最大权重，精确到小数点后3位，服务器会优先返回权重值最高的媒体类型

```json
Accept: image/webp,image/apng,image/*,*/*;q=0.8

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
```

### Accept-Charset

通知服务器用户代理支持的字符集，以及字符集相对的优先顺序，也可以一次性指定多种字符集，用逗号分隔，也可以使用q=n，来指定权重

### Accept-Encoding

告知服务器用户代理支持的内容编码以及内容编码的优先级顺序，可以一次性指定多种编码，用逗号分隔，也可以使用q=n来指定权重，也可以使用*作为通配符

```json
Accept-Encoding: gzip, deflate
```

### Accept-language

告知服务器用户代理能够处理的自然语言集，以及自然语言集的相对优先级

```json
Accept-Language: zh-CN,zh;q=0.9
```

### Authorization

告知服务器用户代理的认证信息

想要通过服务器认证的用户代理在周到401状态码响应后，会把首部字段Authorization加入请求中

### Expect

告知服务器期望出现的某种特定行为，因服务器无法理解客户端的期望作出回应而发生错误时，返回417状态码

### From

告知服务器使用用户代理的用户的电子邮箱

### Host

告知服务器，请求资源所处的互联网主机名和端口号，http/1.1规范哪唯一一个必须包含在内的首部字段

```json
Host: www.baidu.com
```

### If-Match

形如 If-xxx这样子的请求，叫做条件请求，服务器接收到附带的条件请求后，判定指定条件为真，才会执行请求

If-Match告知服务器匹配资源所用的实体标记（ETag）值，服务器会比对 If-Match 和 资源的ETag值，两者一致，才会执行请求，否则返回状态码412 Precondition Failed

还可以使用If-Match：*，服务器会忽略ETag值，只要资源存在，就会返回

### If-modified-Since

告知服务器If-modified-Since的字段值早于资源更新的时间，则希望处理请求

否则返回304 Not Modified状态码

### If-None-Match

该字段与If-Match相反，服务器会比对 If-Match 和 资源的ETag值，两者不一致，才会执行请求

### If-Range

如果If-Range值和请求资源的ETag值一样，则作为范围请求处理，否则返回全部资源

### If-Unmodified-Since

与If-modified-Since作用相反，告知服务器，如果要请求的资源，在当前指定的If-Unmodified-Since指定的日期时间之后没有发生更新的情况下，才会处理请求

如果在指定的时间之后发生更新，则返回状态码412 Precondition Failed

### Max-Forwards

通过TRACE方法或者OPTIONS发送包含首部字段Max-Forwards的请求时，Max-Forwards的值以10进制整数形式指定可以经过的服务器足大数目

服务器在往下一个服务器进行转发之前Max-Forwards的值减1，当Max-Forwards为0 的时候不进行转发

### Proxy-Authorization

接收到从代理服务器发来的认证质询的时候，客户端会发送包含首部字段Proxy-Authorization的请求，告知服务器所需要的信息

### Range

对于只需获取部分资源的请求，首部字段Range告知服务器指定资源的范围

服务器接收到带有Range的请求后，处理请求之后返回206 Partial Content响应，如果无法处理该范围的请求，则返回全部的资源，以及200 OK状态码

### Referer

告知服务器请求的原始资源的URI

```json
Referer: https://www.baidu.com/
```

### TE

告知服务器客户端能够处理的传输编码方式以及相对优先级

### User-Agent

创建请求的浏览器和用户代理名称等信息传递给服务器

## 响应首部字段

响应报文中使用的字段，补充响应的附加信息，服务器信息以及对客户端的附加要求等信息

### Accept-Ranges

告知客户端服务器能否处理范围请求，以指定获取服务器端某个部分的资源

可处理范围请求时 Accept-Ranges： Bytes， 繁殖为none

### Age

告知客户端，源服务器在多久前创立了响应，单位为秒

如果创建响应的服务器是缓存服务器，Age值指的是缓存后的响应再次发起认证到认证完成的时间

### ETag

告知客户端实体标记，这是一种可以将资源以字符串形式做唯一性标识的方式，服务器会为每份资源分配ETag值

当资源更新的时候，虽然资源的URI没有改变，但是ETag值也需要更新

```json
ETag: W/"276b25-3WRldRgTLSW2+HSNh9/CP6Szd18"
```

强ETag：不论实体发生多么细微的变化都会改变值

弱ETag：只用于提示资源是否相同，只有资源发生了根本改变，产生差异的时候才会改变值，会在值的开头以W开头

### Location

将响应接收方引导至与请求URI不同位置的资源

基本上该字段会配合3XX： redirection，提供重定向的URI，基本上所有的浏览器接收到首部字段包含Location的响应后，都会强制性的尝试对已提供的重定向的资源访问

### Proxy-Authenticate

把代理服务器所要求的认证信息发送给客户端

### Retry-After

告知客户端在多久之后再次发送情趣，主要配合状态码502 Service Unavailable，或者3XX Redirect使用

字段值可以是具体的时间，或者是创建响应后的秒数

### Server

告知客户端，当前服务器上安装的http服务器应用程序的信息

包括：服务器上的软件应用的名称，还可能有版本号和安装时启用的可选项

```json
server: nginx/1.10.2
Server: Tengine
```

### Vary

对缓存控制，源服务器向代理服务器传达本地缓存使用方法的命令

```json
Vary: Accept-Encoding
```

Vary指定一个首部字段，请求中Vary指定的首部字段值相同，则返回缓存的资源，否则就要向源服务器重新请求资源

### WWW-authenticate

用于http访问认证，告知客户端适用于请求资源的认证方案和带参数提示的质询

响应状态码401 Unauthorized肯定带有这个字段

## 实体首部字段
