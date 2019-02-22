### 作用
> 可以改变this的指向

#### apply和call
>调用后,该方法也就运行了一次
###### 示例代码
apply和call方法中如果没有传入参数,或者是传入的是null,那么调用该方法的函数对象中的this就是默认的window.
```JS
   function f1(x,y) {
     console.log("结果是:"+(x+y)+this);
     return "10000";
   }
   f1(10,20);//函数的调用


    console.log("========");
    //此时的f1实际上是当成对象来使用的,对象可以调用方法
   // apply和call方法也是函数的调用的方式
    f1.apply();//不传参
    f1.call();//不传参
    console.log("==========");
    f1.apply(null);
    f1.call(null);
```
改变原型指向事例

```JS
function Person(age,sex) {
      this.age=age;
      this.sex=sex;
    }
    //通过原型添加方法
    Person.prototype.sayHi=function (x,y) {
      console.log("您好啊:"+this.sex);
      return 1000;
    };
    var per=new Person(10,"男");
    per.sayHi();

    console.log("==============");
    function Student(name,sex) {
      this.name=name;
      this.sex=sex;
    }
    var stu=new Student("小明","人妖");
    var r1=per.sayHi.apply(stu,[10,20]);
    var r2=per.sayHi.call(stu,10,20);

    console.log(r1);
    console.log(r2);
```


#### 使用语法格式
###### apply
调用的函数所需的参数用数组的格式传入
```JS
var r1=per.sayHi.apply(stu,[10,20]);
```

###### call
调用的函数所需的参数用分别用逗号隔开传入
```JS
var r2=per.sayHi.call(stu,10,20);
```


#### bind
>使用后,调用的方法并未运行,而是复制了一份,bind返回值是一个新的方法
>- apply和call是调用的时候改变this指向
>- bind方法,是赋值一份的时候,改变了this的指向

```JS
//传null默认为windows对象
var ff=f1.bind(null);
    ff(10,20);
//也可以在bind的时候传参
var ff=f1.bind(null,10,20);
ff();
```
###### 使用示例

```JS
    function Person(age) {
      this.age=age;
    }
    Person.prototype.play=function () {
      console.log(this+"====>"+this.age);
    };

    function Student(age) {
      this.age=age;
    }
    var per=new Person(10);
    var stu=new Student(20);
    //复制了一份
    var ff=per.play.bind(stu);
    ff();
```
#### 使用语法
函数名字.bind(对象,参数1,参数2,...);---->返回值是复制之后的这个函数