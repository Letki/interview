#### 简介
CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。它允许浏览器向跨源(协议 + 域名 + 端口)服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

**CORS需要浏览器和服务器同时支持**。它的通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还**会多出一次附加的请求**(与服务端检验是否允许此域名跨域)，但用户不会有感觉。
因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

#### 流程
浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。
浏览器发出CORS简单请求，只需要在头信息之中增加一个Origin字段。
浏览器发出CORS非简单请求，**会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight)**.浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。

只要同时满足以下两大条件，就属于简单请求。
1. 请求方法是以下三种方法之一：
- HEAD
- GET
- POST

2. HTTP的头信息不超出以下几种字段：
- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain