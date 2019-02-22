### 浮动的理解
首先要知道，div是块级元素，在页面中独占一行，自上而下排列，也就是传说中的流
无论多么复杂的布局，其基本出发点均是：“如何在一行显示多个div元素”。
显然标准流已经无法满足需求，这就要用到浮动。<br>
**浮动可以理解为让某个div元素脱离标准流，漂浮在标准流之上，和标准流不是一个层次。**
> 假如某个div元素A是浮动的，如果A元素上一个元素也是浮动的，那么A元素会跟随在上一个元素的后边(如果一行放不下这两个元素，那么A元素会被挤到下一行)；如果A元素上一个元素是标准流中的元素，那么A的相对垂直位置不会改变，也就是说A的顶部总是和上一个元素的底部对齐。

div的顺序是HTML代码中div的顺序决定的。
**靠近页面边缘的一端是前，远离页面边缘的一端是后。**<br>
![image](https://images0.cnblogs.com/blog/471788/201303/27005253-004f304d2c6e424d81e7658e26d3f8a3.png)

假如我们把div2、div3、div4都设置成左浮动，效果如下：
![image](https://images0.cnblogs.com/blog/471788/201303/27005331-95ad1122cc5641a3a3701e6c7fb775e0.png)
> 根据上边的结论，理解一遍：先从div4开始分析，它发现上边的元素div3是浮动的，所以div4会跟随在div3之后；div3发现上边的元 素div2也是浮动的，所以div3会跟随在div2之后；而div2发现上边的元素div1是标准流中的元素，因此div2的相对垂直位置不变，顶部仍 然和div1元素的底部对齐。由于是左浮动，左边靠近页面边缘，所以左边是前，因此div2在最左边。

假如把div2、div3、div4都设置成右浮动，效果如下：
![image](https://images0.cnblogs.com/blog/471788/201303/27005405-331c3a369d5c4bfb8692c40ac3e59bf9.png)
> 道理和左浮动基本一样，只不过需要注意一下前后对应关系。由于是右浮动，因此右边靠近页面边缘，所以右边是前，因此div2在最右边。

<br>
---

### 清除浮动
**清除浮动可以理解为打破横向排列。
清除浮动的关键字是clear，官方定义如下：<br>
语法：<br>
> clear : none | left | right | both <br>
> 取值：none  :  
> - 默认值。允许两边都可以有浮动对象
> - left   :  不允许左边有浮动对象
> - right  :  不允许右边有浮动对象
> - both  :  不允许有浮动对象

**对于CSS的清除浮动(clear)，一定要牢记：这个规则只能影响使用清除的元素本身，不能影响其他元素。**

#### 清理浮动一般有两种思路
1. 利用 clear属性，清除浮动
2. 使父容器形成BFC

##### 1.1 常用方法

###### 添加额外的标签。
```HTML
<div >
<h2>1）清除浮动的第一种方法：添加额外标签</h2>
<div class="head">.</div>
<div class="center"></div>
<div style="clear:both;"></div>
</div>
<div class="footer">.footer</div>
```
或者
```Html
<br clear="all"/>
```
缺点:会增添许多无意义标签。不好后期维护。

###### 使用after伪元素(推荐方法)
```Html
<!doctype html>
<html>
<head>
<meta charset="utf-8"/ content="添加伪类的方法消除浮动">
<style type="text/css">
.bow1{border:3px solid black;}
.box2{background:red;float:left;width:80px;height:100px;}
.box3{background:green;float:left;width:80px;height:100px;}
.clear:after{display:block;content:"";clear:both;}
.clear{zoom:1;}
</style>
</head>
<body>
<div class="bow1 clear">
    <div class="box2"></div>
    <div class="box3"></div>
</div>
</body>
</html>
```
优点:结构和语义化正确，推荐使用
###### 父元素添加高度。
弊端：工作中很多都是自适应的。父元素添加高度往往不可行。

###### 父元素设置overflow:hidden
弊端：当内容过多时导致内容被隐藏，无法显示需要溢出的元素。

###### 父元素也设overflow:auto;
弊端：多个嵌套后，firefox某些情况会造成内容全选；IE中 mouseover 造成宽度改变时会出现最外层模块有滚动条等，firefox早期版本会无故产生focus等,

###### 父元素也设置浮动
弊端：相信我，会因为父元素也发生了浮动，从而产生新的问题。

###### 父元素设置display:table
弊端：盒模型被改变，会因此造成一系列问题。得不偿失

###### 利用position:使得元素周围形成BFC


[不明白的话看这篇文章](https://www.cnblogs.com/acorn/p/5249089.html)
<br>
[CSS清浮动处理](https://www.cnblogs.com/acorn/p/5249188.html)