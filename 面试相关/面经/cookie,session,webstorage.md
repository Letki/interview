#### 概述
> 会话（Session）跟踪是Web程序中常用的技术，用来跟踪用户的整个会话。常用的会话跟踪技术是Cookie与Session。Cookie通过在客户端记录信息确定用户身份，Session通过在服务器端记录信息确定用户身份。

#### Cookie机制
> 本质上cookies就是http的一个扩展。有两个http头部是专门负责设置以及发送cookie的,它们分别是Set-Cookie以及Cookie。当服务器返回给客户端一个http响应信息时，其中如果包含Set-Cookie这个头部时，意思就是指示客户端建立一个cookie，并且在后续的http请求中自动发送这个cookie到服务器端，直到这个cookie过期。

一个cookie的设置以及发送过程分为以下四步：
![image](https://images.cnblogs.com/cnblogs_com/andy-zhou/811282/o_Cookie_Session001.png)

#### 什么是cookie
Cookie 是一些数据, 存储于你电脑上的文本文件中。Cookie 用于存储 web 页面的用户信息。<br>
当 web 服务器向浏览器发送web页面时，在连接关闭后，服务端不会记录用户的信息。<br>
Cookie 的作用就是用于解决 "如何记录客户端的用户信息":<br>
- 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
- 在用户下一次访问该页面时，可以在 cookie中读取用户访问记录。

#### Cookie的不可跨域名性
> Cookie在客户端是由浏览器来管理的。浏览器能够保证Google只会操作Google的Cookie而不会操作Baidu的Cookie，从而保证用户的隐私安全。浏览器判断一个网站是否能操作另一个网站Cookie的依据是域名。Google与Baidu的域名不一样，因此Google不能操作Baidu的Cookie。
> 
> **需要注意的是，虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie。**

如果想所有google.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数。
```JS
Cookie cookie = new Cookie("time","20080808"); // 新建Cookie
cookie.setDomain(".google.com"); // 设置域名
cookie.setPath("/"); // 设置路径
cookie.setMaxAge(Integer.MAX_VALUE); // 设置有效期
response.addCookie(cookie); // 输出到客户端
```