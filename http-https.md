http 不足之处：

+ 通信使用明文（不加密），内容可能会被窃听

+ 不验证通信方的身份，因此有可能遭遇伪装

+ 无法证明报文的完整性，所以有可能已被篡改

## 通信使用明文（不加密），内容可能会被窃听

广泛使用的抓包工具wireshark

### 防止窃听保护

1. 通信加密，使用SSL（安全套接层）和TLS（安全层传输协议），建立安全的通信线路

2. 内容加密

## 不验证通信方的身份，因此有可能遭遇伪装

1. 客户端发送请求，不能确定接收到的响应是否就是期望接收响应的那台服务器返回的响应，因为服务器可能被伪装

2. 服务器无法确定返回的响应是不是被正确的客户端接收，有可能是被伪装的客户端

3. 无法验证互相通信的双方是否正确

4. 无法判定请求来自何方

5. 无法防止DOS攻击（拒绝服务攻击）

查明对手的证书，SSL提供证书手段，证书由信任的第三方机构办法，证明客户端和服务器是真实存在的，伪造证书也相对来说有点困难

进行http通信的时候验证双方的证书，可以判定通信双方的真实面目

## 无法证明报文的完整性，所以有可能已被篡改

--------------------------------------------------------------------------------------------------------

http + 加密 + 认证 + 完整的保护 = https

http本来是直接和TCP通信的，而加了https就要先和SSL通信

### 加密方式

1. 共享密钥加密：用共享密钥加密时，必须将密钥发给对方，如果密钥能够安全的送达，那么被加密的内容也就会安全的送达，加密的意义不大

2. 公开密钥加密：发送密文的一方使用对方的公开密钥把内容进行加密，对方接收到加密的信息后，使用私有密钥进行解密

公开密钥加密更安全，同时要比共享密钥加密处理起来更麻烦，处理速度也会慢一点，要充分利用两者的优缺点进行使用

+ 使用公开密钥加密方式，安全的交换在稍后共享密钥加密中使用的共享密钥

+ 确保交换的密钥时安全的前提下，使用共享密钥加密方式进行加密

### 验证公开密钥的证书

用来确定建立公开密钥加密的通信时收到的密钥就是原本期望的服务器传来的密钥，证书一般由通信双方都信任的机构颁发

数字认证机构的业务流程：

1. 服务器运营人员向数字认证机构提出公开密钥申请

2. 数字认证机构在判明提出者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将公开密钥放入公钥证书，然后绑定在一起

3. 服务器会讲这份由数字证书认证机构颁发的公钥证书发送给客户端

4. 接到数字证书的客户端，用这份证书来进行公开密钥加密通信，会对这张证书进行验证

5. 认证通过，客户端可以确定认证服务器的公开密钥的是真实有效的数字证书认证机构，服务器的公开密钥是值得信任的

https中还可以使用客户端证书

## https通信步骤

首先客户端询问服务器端能否建立SSL安全连接，协商决定加密组件

1. 客户端通过发送 Client Hello 报文开始 SSL 通信。报文中包含客户端支持的 SSL 的指定版本、加密组件(Cipher Suite)列表(所使用的加密算法及密钥长度等)

2. 服务器可进行 SSL 通信时，会以 Server Hello 报文作为应答。和客户端一样，在报文中包含 SSL 版本以及加密组件。服务器的加密组件内容是从接收 到的客户端加密组件内筛选出来的。

3. 之后服务器发送 Certificate 报文。报文中包含公开密钥证书。

4. 最后服务器发送 Server Hello Done 报文通知客户端，最初阶段的 SSL 握手（第一次握手）协商部分结束

如果服务器可以进行SSL连接，就开始建立SSL安全连接，客户端接收到服务器给到的公钥证书，首先会确定证书的有效性

1. SSL 第一次握手结束之后，（第二次握手）客户端以 Client Key Exchange 报文作为回应。报文中包含通信加密中使用的一种被称为 Pre-master secret 的随机密码串（其实就是共享密钥）。该 报文已用步骤 3 中的公开密钥进行加密。

2. 接着客户端继续发送 Change Cipher Spec（更改密码规范） 报文。该报文会提示服务器，在此报文之后的通信会采用 Pre-master secret 密钥加密。（共享密钥）

3. 客户端发送 Finished 报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。

4. 服务端接收到Pre-master secret之后会用自己的私钥进行解密，服务器同样发送 Change Cipher Spec 报文。

5. 服务器同样发送 Finished 报文。

开始建立HTTP请求

1. 服务器和客户端的 Finished 报文交换完毕之后，SSL 连接就算建立完成。当然，通信会受到 SSL 的保护。从此处开始进行应用层协议的通信，即发 送 HTTP 请求。

2. 应用层协议通信，即发送 HTTP 响应。

3. 最后由客户端断开连接。断开连接时，发送 close_notify 报文。

https要进行加密解密的过程，所以通信速度会慢，而且加密和加密同时会给客户端和服务端都带来CPU的消耗
