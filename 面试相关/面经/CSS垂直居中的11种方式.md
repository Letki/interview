##### 1. 使用绝对定位和负外边距对块级元素进行垂直居中
html代码:
```html
<div id="box">
    <div id="child">我是测试DIV</div>
</div>
```
CSS代码
```CSS
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    position: relative;
}
#child {
    width: 150px;
    height: 100px;
    background: orange;
    position: absolute;
    top: 50%;
    margin: -50px 0 0 0;
    line-height: 100px;
}
```
> 300*0.5=150px;margin-top为0的话,会从距离150px的地方渲染,设置margin-top就是为了把它拉回来,(300-100)/2=100px,即距离100px的地方渲染就可以居中了.所以margin-top要设置-50px拉回来
>
> 这个方法兼容性不错，但是有一个小缺点：必须提前知道被居中块级元素的尺寸，否则无法准确实现垂直居中。

##### 2. 使用绝对定位和transform
CSS代码
```CSS
#child {
    background: #93BC49;
    position: absolute;
    top: 50%;
    transform: translate(0, -50%);
}
```
> 这种方法有一个非常明显的好处就是不必提前知道被居中元素的尺寸了，**因为transform中translate偏移的百分比就是相对于元素自身的尺寸而言的**。[(去学习CSS的transform)](http://www.runoob.com/cssref/css3-pr-transform.html)

##### 3. 另外一种使用绝对定位和负外边距进行垂直居中的方式
```CSS
#child {
　　width: 50%;
    height: 30%;
    background: pink;
    position: absolute;
    top: 50%;
    margin: -15% 0 0 0;
}
```
> 这种方式的原理实质上和第一种相同。只是把-50px用百分比表示补充的一点是：margin的取值也可以是百分比，这时这个值规定了该元素基于父元素尺寸的百分比，可以根据实际的使用场景来决定是用具体的数值还是用百分比。

##### 4. 绝对定位结合margin: auto
```CSS
#child {
    width: 200px;
    height: 100px;
    background: #A1CCFE;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    line-height: 100px;
}
```
> 这种实现方式的两个核心是：把要垂直居中的元素相对于父元素绝对定位，top和bottom设为相等的值，我这里设成了0，也可以设为99999px或者-99999px无论什么，只要两者相等就行，这一步做完之后再将要居中元素的margin设为auto，这样便可以实现垂直居中了。
>
>**要实现居中top和bottom,left和right必须成对出现.加上left和right为0就可以实现水平居中**
>
>被居中元素的宽高也可以不设置，但不设置的话就必须是图片这种自身就包含尺寸的元素，否则无法实现。

##### 5. 使用padding实现子元素的垂直居中
```CSS
#box {
    width: 300px;
    background: #ddd;
    padding: 100px 0;
}
#child {
    width: 200px;
    height: 100px;
    background: #F7A750;
    line-height: 50px;
}
```
> 这种实现方式非常简单，就是给父元素设置相等的上下内边距，则子元素自然是垂直居中的，当然这时候父元素是不能设置高度的，要让它自动被填充起来，除非设置了一个正好等于上内边距+子元素高度+下内边距的值，否则无法精确的垂直居中。
　　这种方式看似没有什么技术含量，但其实在某些场景下也是非常好用的。

如果要加上高度的话,就要把标准盒模型换成IE盒模型[(两者如何切换和区别)](https://blog.csdn.net/zyuzixiao/article/details/18733463)

代码改写:
```CSS
#box {
    width: 300px;
  height:300px;
    background: #ddd;
    padding: 100px 0;
  box-sizing:border-box;    /*重点 content-box为标准盒模型*/
}
#child {
    width: 200px;
    height: 100px;
    background: #F7A750;
    line-height: 50px;
}
```

##### 6. 设置第三方基准
Html代码:
```html
<div id="box">
    <div id="base"></div>
    <div id="child">今天写了第一篇博客，希望可以坚持写下去！</div>
</div>
```
CSS代码:
```CSS
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
}
#base {
    height: 50%;
    background: #AF9BD3;
}
#child {
    height: 100px;
    background: rgba(131, 224, 245, 0.6);
    line-height: 50px;
    margin-top: -50px;
}
```
运行结果如下：

![image](https://images2015.cnblogs.com/blog/1082990/201612/1082990-20161219215048135-1923582137.png)

> 这种方式也非常简单，首先设置一个高度等于父元素高度一半的第三方基准元素，那么此时该基准元素的底边线自然就是父元素纵向上的中分线，做完这些之后再给要垂直居中的元素设置一个margin-top，值的大小是它自身高度的一半取负，则实现垂直居中。

##### 7. 使用flex布局(好用,喜欢!)
html代码使用最开始的

CSS代码:
```CSS
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    align-items: center;
}
#child {
    width: 300px;
    height: 100px;
    background: #8194AA;
    line-height: 100px;
}
```
> flex布局很好用->认真学!~[(阮一峰flex布局教程)](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool))

##### 8. 第二种使用弹性布局的方式
```CSS
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
#child {
    width: 300px;
    height: 100px;
    background: #08BC67;
    line-height: 100px;
}
```
##### 9. 还有一种在前面已经见到过很多次的方式就是使用 line-height 对单行文本进行垂直居中
html代码：
```html
<div id="box">
    我是一段测试文本
</div>
```
CSS代码
```CSS
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px;/*行高设为高度一样即可*/
}
```
> 这里有一个小坑需要注意：line-height(行高)的值不能设为100%，我们来看看官方文档中给出的关于line-height取值为百分比时候的描述：基于当前字体尺寸的百分比行间距。所以大家就明白了，这里的百分比并不是相对于父元素尺寸而言，而是相对于字体尺寸来讲的。

##### 10. 使用 line-height和vertical-align对图片进行垂直居中
Html代码:
```Html
<div id="box">
    <img src="duncan.jpeg">
</div>
```
CSS代码:
```CSS
#box{
    width: 300px;
    height: 300px;
    background: #ddd;
    line-height: 300px;
}
#box img {
    vertical-align: middle;
}
```
**vertical-align相关:**<br>
[CSS深入理解vertical-align和line-height的基友关系 ](https://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)<br>
[深入理解css中vertical-align属性- 博客园](http://www.cnblogs.com/starof/p/4512284.html?utm_source=tuicool&utm_medium=referral)

##### 11. 使用 display 和 vertical-align 对容器里的文字进行垂直居中
```CSS
#box {
    width: 300px;
    height: 300px;
    background: #ddd;
    display: table;
}
#child {
    display: table-cell;
    vertical-align: middle;
}
```