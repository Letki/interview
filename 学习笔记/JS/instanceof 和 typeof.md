# typeof 操作符
#### 简介
typeof操作符返回一个字符串，表示未经计算的操作数的类型

#### 返回值类型

类型 | 结果
---|---
Undefined |	"undefined"
Null | "object"（见下文）
Boolean | "boolean"
Number | "number"
String | "string"
Symbol （ES6 新增）| "symbol"
宿主对象（由JS环境提供） | Implementation-dependent
函数对象（[[Call]] 在ECMA-262条款中实现了）	| "function"
任何其他对象 |	"object"

#### typeof 事例
```
// Numbers
typeof 37 === 'number';
typeof 3.14 === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; // 尽管NaN是"Not-A-Number"的缩写
typeof Number(1) === 'number'; // 但不要使用这种形式!

// Strings
typeof "" === 'string';
typeof "bla" === 'string';
typeof (typeof 1) === 'string'; // typeof总是返回一个字符串
typeof String("abc") === 'string'; // 但不要使用这种形式!

// Booleans
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(true) === 'boolean'; // 但不要使用这种形式!

// Symbols
typeof Symbol() === 'symbol';
typeof Symbol('foo') === 'symbol';
typeof Symbol.iterator === 'symbol';

// Undefined
typeof undefined === 'undefined';
typeof declaredButUndefinedVariable === 'undefined';
typeof undeclaredVariable === 'undefined'; 

// Objects
typeof {a:1} === 'object';

// 使用Array.isArray 或者 Object.prototype.toString.call
// 区分数组,普通对象
typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';

// 下面的容易令人迷惑，不要使用！
typeof new Boolean(true) === 'object';
typeof new Number(1) === 'object';
typeof new String("abc") === 'object';

// 函数
typeof function(){} === 'function';
typeof class C{} === 'function'
typeof Math.sin === 'function';
typeof new Function() === 'function';
```
>  All constructor functions while instantiated with 'new' keyword will always be typeof 'object'<br>
所有用new 生成的对象,用typeof方法都会返回**object**
有个例外就是function对象,它将会返回function

```
var str = new String('String');
var num = new Number(100);

typeof str; // It will return 'object'
typeof num; // It will return 'object'

// But there is a exception in case of Function constructor of Javascript

var func = new Function();

typeof func; // It will return 'function'
```
正则表达式不同浏览器会返回不同的值
```
//对正则表达式字面量的类型判断在某些浏览器中不符合标准：
typeof /s/ === 'function'; // Chrome 1-12 , 不符合 ECMAScript 5.1
typeof /s/ === 'object'; // Firefox 5+ , 符合 ECMAScript 5.1
```

###### 【typeof为什么要区分object和function？】<br>
- 答案一：《JavaScript高级程序设计》：从技术角度讲，函数在ECMAScript中是对象，不是一种数据类型。然而，函数也确实有一些特殊的属性，因此通过typeof操作符来区分函数和其他对象是有必要的。<br>
- 答案二：在实际的使用过程中有必要区分Object和Function，所以在typeof这里实现了

###### 【typeof的不足之处】
-  不能区分对象、数组、正则，对它们操作都返回"object"；（正则特殊一点后面说）
-  Safar5,Chrome7之前的版本对正则对象返回 'function'
-  在IE6,7和8中，大多数的宿主对象是对象，而不是函数；如：typeof alert; //object
-  而在非ID浏览器或则IE9以上（包含IE9），typeof alert; //function