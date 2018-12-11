##### 例一 采用module.exports 暴露接口
```
function hello() {
    console.log('Hello, world!');
}

function world(name) {
    console.log('Hello, ' + name);
}

module.exports = {
    hello: hello,
    world: world
};
```
##### 例二 采用exports 暴露接口
```
function hello() {
    console.log('Hello, world!');
}

function world(name) {
    console.log('Hello, ' + name);
}

exports.hello = hello;
exports.world= world;
```
##### 区别
不可以直接对进行exports赋值：
```
exports = {
    hello: hello,
    world: world
};//这样代码不报错, 但是没有用, 接口并未暴露出来. 原因请看下面
```
##### 原因
module.exports初始为指向为一个空对象,exports也指向这个对象,是这个对象的另一个应用。就像下面这个例子：
```
var a = {
  color: "white"
};
var b = a;

console.log(a);//{ color: 'white' }
console.log(b);//{ color: 'white' }

b.color = "red";

console.log(a);//{ color: 'red' }
console.log(b);//{ color: 'red' }


b = {
  name: "test"
};

console.log(a);//{ color: 'red' }
console.log(b);//{ name: 'test' }
```
在代码的前面部分，b和a是指向相同内存里的对象，所以更改b，a也同时更改了。再者，我们通过赋值为b = {name: “test”}以后，发现b指向了一块新的内存，因此两者输出的都不一样了。

如果直接exports = {} exports对象就指向新的内存地址。所以接口实际未被暴露。

> 总结起来，exports 和 module.exports 二者的关系： 
> module.exports 初始值为一个空对象 {},而exports为指向module.exports 的引用，同时在require() 的时候，返回的是 module.exports 而不是 exports，因此，直接赋值exports常常会出现错误，而赋值为module.exports常常是解决这一问题的折中办法。

#### 因此推荐使用module.exports
