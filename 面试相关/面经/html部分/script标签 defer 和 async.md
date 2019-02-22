![image](https://user-images.githubusercontent.com/9568094/31619989-a874ae42-b25b-11e7-9a80-e0f644f27849.png)

#### 普通script （类似串行）
文档解析的过程中，如果遇到script脚本，就会停止页面的解析进行下载（但是Chrome会做一个优化，如果遇到script脚本，会快速的查看后边有没有需要下载其他资源的，如果有的话，会先下载那些资源，然后再进行下载script所对应的资源，这样能够节省一部分下载的时间 @Update: 2018-08-17）。
资源的下载是在解析过程中进行的，虽说script1脚本会很快的加载完毕，但是他前边的script2并没有加载&执行，所以他只能处于一个挂起的状态，等待script2执行完毕后再执行。
当这两个脚本都执行完毕后，才会继续解析页面。

![image](https://user-images.githubusercontent.com/9568094/31621391-39849b1a-b25f-11e7-9301-641b1bc07155.png)

#### defer（并行加载，串行运行，再触发DomContentLoaded）
文档解析时，遇到设置了defer的脚本，就会在后台进行下载，但是并不会阻止文档的渲染，当页面解析&渲染完毕后。
会等到所有的defer脚本加载完毕并按照顺序执行，执行完毕后会触发DOMContentLoaded事件。

![image](https://user-images.githubusercontent.com/9568094/31621324-046d4a44-b25f-11e7-9d15-fe4d6a5726ae.png)

#### async
async脚本会在**加载完毕后执行**。
async脚本的加载不计入DOMContentLoaded事件统计，也就是说下图两种情况都是有可能发生的

![image](https://user-images.githubusercontent.com/9568094/31621170-b4cc0ef8-b25e-11e7-9980-99feeb9f5042.png)
![image](https://user-images.githubusercontent.com/9568094/31622216-6c37db9c-b261-11e7-8bd3-79e5d4ddd4d0.png)

###### 什么是DOMContentLoaded
DOMContentLoaded顾名思义，就是dom内容加载完毕。那什么是dom内容加载完毕呢？我们从打开一个网页说起。当输入一个URL，页面的展示首先是空白的，然后过一会，页面会展示出内容，但是页面的有些资源比如说图片资源还无法看到，此时页面是可以正常的交互，过一段时间后，图片才完成显示在页面。从页面空白到展示出页面内容，会触发DOMContentLoaded事件。而这段时间就是HTML文档被加载和解析完成。JS会阻塞html的解析

参考文章：
[浅谈script标签中的async和defer](https://www.cnblogs.com/jiasm/p/7683930.html)