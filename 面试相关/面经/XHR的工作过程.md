##### 1.概述

> ajax是asynchronous javascript and XML的简写，中文翻译是异步的javascript和XML，这一技术能够向服务器请求额外的数据而无须卸载页面，会带来更好的用户体验。虽然名字中包含XML，但ajax通信与数据格式无关

**ajax包括以下几步骤：**
1. 创建AJAX对象;
2. 发出HTTP请求；
3. 接收服务器传回的数据；
4. 更新网页数据

概括起来，就是一句话，ajax通过原生的XMLHttpRequest对象发出HTTP请求，得到服务器返回的数据后，再进行处理

##### 2.创建
> ajax技术的核心是XMLHttpRequest对象(简称XHR);XMLHttpRequest对象是通过JavaScript创建的

代码如下:
```JS
var xhr;
if(window.XMLHttpRequest){
    xhr = new XMLHttpRequest();
}else{
    xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
```
**Tips**:如果要==建立N个不同的请求，就要使用N个不同的XHR对象==。当然可以重用已存在的XHR对象，但这会终止之前通过该对象挂起的任何请求

##### 3.XMLHttpRequest对象方法与属性
**open()**<br>
在使用XHR对象时，要调用的第一个方法是open()，如下所示，该方法接受3个参数
```JS
xhr.open("get","example.php", false);
```

**open(method,url,[async],[username],[password])**
> 规定请求的类型、URL 以及是否异步处理请求。<br>
> method：请求的类型；GET 或 POST（特别说明的是：仍要特别注意两种请求类型下的参数汉字乱码的问题）<br>
>url：要访问的文件的URL

> open() 方法的 url 参数是服务器上文件的地址：xmlhttp.open("GET","ajax_test.asp",true);该文件可以是任何类型的文件，比如 .txt 和 .xml，或者服务器脚本文件，比如 .asp 和.php （在传回响应之前，能够在服务器上执行任务）。
> 
> async：true（异步）或 false（同步）。该参数是可选的，默认为 true。异步优点在等待服务器响应时执行其他脚本；当响应就绪后对响应进行处理<br>
> username：如果需要身份验证，则可以在此指定用户名。该参数没有默认值。<br>
> password：如果需要身份验证，则可以在此指定口令。该参数没有默认值。<br>
> 说明：通常使用其中的前三个参数。事实上，即使需要异步连接，通常指定第三个参数为 “true”，这样更容易理解。

**send([string])**

将要发送的内容发送到服务器。<br>
string：仅用于 POST 请求。
> 如果是GET方法，send()方法无参数，或参数为null；如果是POST方法，send()方法的参数为要发送的数据

```JS
xhr.open("get", "example.txt", false);
xhr.send(null);
```
##### 接收响应
一个完整的HTTP响应由状态码、响应头集合和响应主体组成。在收到响应后，这些都可以通过XHR对象的属性和方法使用，主要有以下4个属性
> responseText: 作为响应主体被返回的文本(文本形式)<br>
responseXML: 如果响应的内容类型是'text/xml'或'application/xml'，这个属性中将保存着响应数据的XML DOM文档(document形式)<br>
status: HTTP状态码(数字形式)<br>
statusText: HTTP状态说明(文本形式)

在接收到响应后，第一步是检查status属性，以确定响应已经成功返回。一般来说，可以将HTTP状态码为200作为成功的标志。此时，responseText属性的内容已经就绪，而且在内容类型正确的情况下，responseXML也可以访问了。此外，状态码为304表示请求的资源并没有被修改，可以直接使用浏览器中缓存的版本；当然，也意味着响应是有效的<br>
　　无论内容类型是什么，响应主体的内容都会保存到responseText属性中，而对于非XML数据而言，responseXML属性的值将为null
```JS
if((xhr.status >=200 && xhr.status < 300) || xhr.status == 304){
    alert(xhr.responseText);
}else{
    alert('request was unsuccessful:' + xhr.status);
}
```
##### XMLHttpRequest对象工作流程图
![image](https://img-blog.csdn.net/20180626223324617?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xJVUhPTkdMSUFOOTE1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)