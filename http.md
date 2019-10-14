---
title: http
date: 2019-09-10 14:25:59
tags: http
---

用于客户端和服务器端的通信

请求必定由客户端发出，而服务端进行响应回复，服务端在没有接收到请求之前不会发送响应

## 请求报文

http请求报文构成：请求方法，请求URI，协议版本，可选的请求首部字段，请求实体

![http请求报文](/img/http/http请求报文.png)

响应报文：协议版本，状态码，解释状态码的原因短语，可选的响应首部字段，实体主体

![http响应报文](/img/http/http请求报文.png)

## 无状态协议

使用cookie技术管理撞塌

## http方法（http/1.1）

### get：获取资源

请求已经被URI识别的资源，指定的资源经过服务器端的解析之后返回响应内容，如果请求的资源是文本，那就原样返回，如果请求的资源需要解析，就返回解析之后的内容

### post：传输实体主体

### put：传输文件

就像FTP协议的文件上传，要求在请求的报文主体中包含文件内容，然后保存到请求URI指定的地址

http/1.1 put方法自身不带验证机制，存在安全性问题

### head：获取报文首部

与get方法一样只是不反悔报文主体部分，用于确认URI的有效性和资源更新的日期时间等

### delete：删除文件

按请求URI删除指定的资源

http/1.1 delete方法自身不带验证机制，存在安全性问题

### options：询问支持的方法

1.1新增

查询针对请求URI指定的资源支持的方法

### trace：追踪路径

1.1新增

该方法是让web服务器端将之前的请求通信环回给客户端

容易引发跨站追踪

### connect：要求用隧道协议链接代理

1.1新增

该方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信，主要使用SSL（安全套接层）和TLS（传输层安全）协议把通信内容加密后经过网络隧道直接传输

### LINK UNLINK方法被1.1废弃