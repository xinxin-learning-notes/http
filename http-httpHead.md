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
