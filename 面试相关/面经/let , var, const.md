### var
通过var定义的变量，作用域是整个封闭函数，是全域的。<br>
##### var有变量提升的特性
浏览器在运行代码之前会进行预解析，**首先解析函数声明，定义变量**，解析完之后再对函数、变量进行运行、赋值等。 
- 不论var声明的变量处于当前作用域的第几行，都会提升到作用域的头部。 
- var 声明的变量会被提升到作用域的顶部并初始化为undefined，而let声明的变量在作用域的顶部未被初始化

```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;
//相当于
var foo;  //声明且初始化为undefined
console.log(foo);
foo=2;
```
**var声明的变量，在声明前调用不会直接报错，只会出现undefined。**

### let
通过let定义的变量，作用域是在块级或是子块中。
##### 典型例子(for循环)
```
//使用let声明
for(let i = 0; i < 9; i++) {
    // i的作用域只有这里面部分
}
 console.log(i) // 会报错;
// 使用var声明
for(var i = 0; i < 9; i++) {
    // i的作用域只有这里面部分
}
console.log(i) // 用var声明是可以访问的,因为变量提升了;
```
```
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
//相当于在第一行先声明bar但没有初始化，直到赋值时才初始化
```

- 只要块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部的影响。总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的，尽管代码块外也存在相同全局变量。
```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
alert(tmp);  //输出值为123，全局tmp与局部tmp不影响
```
- let 允许同一个作用域下有相同的变量声明
```
// 报错
function () {
  let a = 10;
  var a = 1;
}
// 报错
function () {
  let a = 10;
  let a = 1;
}
```

### const
const定义的变量不可以修改，而且必须初始化。
**是静态变量**


> **ES6明确规定，如果区块中存在let和const，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。**