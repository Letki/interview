## http
#### TCP报文格式
![image](https://upload-images.jianshu.io/upload_images/1500839-7c70aa7edb98cfb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/383/format/webp)

**上图中有几个字段需要重点介绍下：**
1. 序号：Seq序号，占32位，用来标识从TCP源端向目的端发送的字节流，发起方发送数据时对此进行标记。
2. 确认序号：Ack序号，占32位，只有ACK标志位为1时，确认序号字段才有效，Ack=Seq+1。
3. 标志位：共6个，即URG、ACK、PSH、RST、SYN、FIN等，具体含义如下：
> - （A）URG：紧急指针（urgent pointer）有效。
> - （B）ACK：确认序号有效。
> - （C）PSH：接收方应该尽快将这个报文交给应用层。
> - （D）RST：重置连接。
> - （E）SYN：发起一个新连接。
> - （F）FIN：释放一个连接。

#### http三次握手
###### 是什么?
HTTP（HyperText Transfer Protocol)超文本传输协议是互联网上应用最为广泛的一种网络协议。由于信息是明文传输，所以被认为是不安全的。而关于HTTP的三次握手，其实就是使用三次TCP握手确认建立一个HTTP连接。
###### 为什么?
> 每当建立一个TCP/IP连接的时候都要经历3次握手，这是为了保证建立一个可靠的连接。
###### 怎么做?

如下图所示，SYN（synchronous）是TCP/IP建立连接时使用的握手信号、Sequence number（序列号）、Acknowledge number（确认号码），三个箭头指向就代表三次握手，完成三次握手，客户端与服务器开始传送数据。

![image](https://upload-images.jianshu.io/upload_images/1500839-e288b048fa80e96c.png?imageMogr2/auto-orient/)
1. **第一次握手**：Client将标志位SYN置为1，随机产生一个值seq=J，并将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认。<br>
2. **第二次握手**：Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给Client以确认连接请求，Server进入SYN_RCVD状态。<br>
3. **第三次握手**：Client收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给Server，Server检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client与Server之间可以开始传输数据了。

#### http四次挥手
###### 怎么做?
![image](https://upload-images.jianshu.io/upload_images/1500839-d185016c4ea4e36f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507/format/webp)
> 由于TCP连接时全双工的，因此，每个方向都必须要单独进行关闭，这一原则是当一方完成数据发送任务后，发送一个FIN来终止这一方向的连接，收到一个FIN只是意味着这一方向上没有数据流动了，即不会再收到数据了，但是在这个TCP连接上仍然能够发送数据，直到这一方向也发送了FIN。首先进行关闭的一方将执行主动关闭，而另一方则执行被动关闭，上图描述的即是如此。

1. **第一次挥手**：Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。
2. **第二次挥手**：Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。
3. **第三次挥手**：Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。
4. **第四次挥手**：Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手。

上面是一方主动关闭，另一方被动关闭的情况，实际中还会出现同时发起主动关闭的情况，具体流程如下图：

![image](https://upload-images.jianshu.io/upload_images/1500839-6c29c7564f19c33a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507/format/webp)

<br>

#### 相关面试问题
- **三次握手是什么或者流程？四次握手呢？答案前面分析就是。**
- **为什么建立连接是三次握手，而关闭连接却是四次挥手呢？**
<br> **答**:这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送。

<br>

---
<br>

## https
HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。具体是如何进行加密，解密，验证的，且看下图

![image](https://upload-images.jianshu.io/upload_images/2829175-9385a8c5e94ad1da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/648)

**1. client Hello,客户端（通常是浏览器）先向服务器发出加密通信的请求。传输的内容为：**

> （1） 支持的协议版本，比如TLS 1.0版。<br>
 （2） 一个客户端生成的随机数random1，稍后用于生成”对话密钥”。<br>
 （3） 支持的加密方法，比如RSA公钥加密。<br>
 （4） 支持的压缩方法。
 
**2. 服务器收到请求,然后响应 (server Hello)**
> （1） 确认使用的加密通信协议版本，比如TLS 1.0版本。如果浏览器与服务器支持的版本不一致，服务器关闭加密通信。<br>
（2） 一个服务器生成的随机数random2，稍后用于生成”对话密钥”。<br>
（3） 确认使用的加密方法，比如RSA公钥加密。<br>
（4） 服务器证书。

**3. 客户端收到证书之后会首先会进行验证，验证通过之后生成随机数r，并将随机数用证书的公钥加密，传回服务器。**<br>
**4. 服务器用证书的私钥进行解密，并在后面通讯中使用r作为 对称加密的密钥，对网页内容加密。**

<br>

---

<br>

##### 参考文章:
[SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)