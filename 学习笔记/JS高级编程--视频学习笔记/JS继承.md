### JS继承的方式
> 原型作用之一:数据共享,节省内存空间
<br>原型作用之二:为了实现继承

>**多态**:一个对象有不同的行为,或者是同一个行为针对不同的对象,产生不同的结果,要想有多态,就要先有继承,js中可以模拟多态,但是不会去使用,也不会模拟
#### 通过原型实现继承
```JS
    function Person(name,age,sex) {
      this.name=name;
      this.sex=sex;
      this.age=age;
    }
    Person.prototype.eat=function () {
      console.log("人可以吃东西");
    };
    Person.prototype.sleep=function () {
      console.log("人在睡觉");
    };
    Person.prototype.play=function () {
      console.log("生活就是不一样的玩法而已");
    };


    function Student(score) {
      this.score=score;
    }
    //改变学生的原型的指向即可==========>学生和人已经发生关系
    Student.prototype=new Person("小明",10,"男");
    Student.prototype.study=function () {
      console.log("学习很累很累的哦.");
    };

    //相同的代码太多,造成了代码的冗余(重复的代码)

    var stu=new Student(100);
    console.log(stu.name);
    console.log(stu.age);
    console.log(stu.sex);
    stu.eat();
    stu.play();
    stu.sleep();
    console.log("下面的是学生对象中自己有的");
    console.log(stu.score);
    stu.study();
```
为了数据共享,改变原型指向,做到了继承---通过改变原型指向实现的继承.<br>
缺陷:因为改变原型指向的同时实现继承,直接初始化了属性，继承过来的属性的值都是一样的了,所以,只能重新调用对象的属性进行重新赋值.
###### 解决方案:继承的时候,不用改变原型的指向,直接调用父级的构造函数的方式来为属性赋值就可以了------借用构造函数:把要继承的父级的构造函数拿过来,使用一下就可以了

#### 借用构造函数实现继承
```JS
    function Person(name, age, sex, weight) {
      this.name = name;
      this.age = age;
      this.sex = sex;
      this.weight = weight;
    }
    Person.prototype.sayHi = function () {
      console.log("您好");
    };
    function Student(name,age,sex,weight,score) {
      //借用构造函数
      Person.call(this,name,age,sex,weight);
      this.score = score;
    }
    var stu1 = new Student("小明",10,"男","10kg","100");
    console.log(stu1);
    console.log(stu1.name, stu1.age, stu1.sex, stu1.weight, stu1.score);

    var stu2 = new Student("小红",20,"女","20kg","120");
    console.log(stu2.name, stu2.age, stu2.sex, stu2.weight, stu2.score);

    var stu3 = new Student("小丽",30,"妖","30kg","130");
    console.log(stu3.name, stu3.age, stu3.sex, stu3.weight, stu3.score);
```
借用构造函数:构造函数名字.call(当前对象,属性,属性,属性....);<br>
解决了属性继承,并且值不重复的问题
<br>缺陷:父级类别中的方法不能继承

#### 组合继承
> 通过原型继承和构造函数组合实现

```JS
    function Person(name,age,sex){
      this.name=name;
      this.age=age;
      this.sex=sex;
    };
    Person.prototype.sayHi=function(){
      console.log("sayHi");
    }

    function Student(name,age,sex,score) {
      Person.call(this,name,age,sex);
      this.score=score;
    }
    Student.prototype=new Person();
    Student.prototype.constructor=Student;//此方法要恢复构造器指针
    var stu1=new Student("Letki",17,"男",100);
    console.log(stu1);
```

#### 拷贝继承
> 把一个对象中的属性或者方法直接复制到另一个对象中

```JS
function Person() {
    }
    Person.prototype.age=10;
    Person.prototype.sex="男";
    Person.prototype.height=100;
    Person.prototype.play=function () {
      console.log("玩的好开心");
    };
    console.dir(Person);
    var obj2={};
    //Person的构造中有原型prototype,prototype就是一个对象,那么里面,age,sex,height,play都是该对象中的属性或者方法

    for(var key in Person.prototype){
      obj2[key]=Person.prototype[key];
    }
    console.dir(obj2);
    obj2.play();
```