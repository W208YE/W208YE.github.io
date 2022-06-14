# 说明：JavaScript-Note

此笔记为[尚硅谷李立超js](https://www.bilibili.com/video/BV1YW411T7GX?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click)视频学习笔记。
笔记参考于[学习JavaScript这一篇就够了](https://caochenlei.blog.csdn.net/article/details/109257751)。
笔记为不完全转载，自己进行了一定程度的修改和自己的理解，希望看完整版的请去上面链接。

# 第一章 JavaScript基础语法

## 1.3 数据类型

### 1.3.1 类型分类

五种基本数据类型 String, Number, Boolean, Undefined, Null

一种引用数据类型 Object（null用typeof检验是Object类型）

### 1.3.2 typeof运算符

### 1.3.3 String



<img src="https://img-blog.csdnimg.cn/img_convert/8cd27bded82d24387ae42b8444f0d25e.png" alt="image-20201013085608008" style="zoom:50%;" />

### 1.3.4 Number

最大值：（Number.MAX_VALUE）+1.7976931348623157e+308

大于0的最小值：（Number.MIN_VALUE）5e-324

最小值：-1.7976931348623157e+308

**特殊的数值(typeof检查都是Number类型)：**

- Infinity：正无穷
- -Infinity：负无穷
- NaN：非法数字（Not A Number）

### 1.3.5 Boolean

### 1.3.6 Undefined

Undefined 类型只有一个值，即特殊的 undefined。

在声明变量但未对其加以初始化时，变量的默认值就是 undefined。

> 注意：使用typeof检查没有声明的变量时，将返回“undefined”。

### 1.3.7 Null

Null类型也只有一个值，即null。null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象。

undefined值实际上是由null值衍生出来的，所以如果比较undefined和null是否相等，会返回true。

> 注意：从语义上看null表示的是一个空的对象，即是一个不存在对象的占位符,  所以使用typeof检查null会返回一个Object。

## 1.4 强制类型转换

一般是指，将其它的数据类型转换为String、Number、Boolean

### 1.4.1 转换为String

三种方法：toString()、String()、拼串

***1.toString()方法***

不会影响到原变量

**null**、**undefined**这两个值没有toString()方法

***2.String()函数***

使用String()函数做强制类型转换时, **对于Number和Boolean实际上就是调用的toString()方法**，但是对于null和undefined，就不会调用toString()方法，它会将 null 直接转换为 “null”，将 undefined 直接转换为 “undefined”

***3.将任意数据类型 + ""***

```javascript
let a = 123;
a = a + "";
console.log(a);
console.log(typeof a);

let b;
let a = 'asd' + b;
console.log(a); // "asdundefined"
```

### 1.4.2 转换为Number

Number()、parseInt() 和parseFloat()。

Number()可以用来转换任意类型的数据。

而后两者只能用于转换字符串。（“只能“不准确，但你要是用本就是Number的变量进行转换，这种行为是无意义的。除Number和String之外的类型转换后都是NaN）

根本原因是这两个函数先将非String的变量转换为String，然后再进行提取操作

parseInt()只会将字符串转换为整数，而parseFloat()可以将字符串转换为浮点数。

***1.Number()函数***

字符串 --> 数字
		如果是纯数字的字符串，则直接将其转换为数字
		如果字符串中有非数字的内容，则转换为NaN
		如果字符串是一个空串或者是一个全是空格的字符串，则转换为0
布尔 --> 数字
		true 转成 1
		false 转成 0
null --> 数字
		null 转成 0
undefined --> 数字
		undefined 转成 NaN

- 二进制：0b 开头表示二进制，但是，并不是所有的浏览器都支持
- 八进制：0 开头表示八进制
- 十六进制：0x 开头表示十六进制

***2.parseInt()函数***

将字符串中的有效整数提取出来（可以用来取整）

如果字符串是一个空串或者是一个全是空格的字符串，则转换为NaN

```javascript
let a = "123pxpxpd"; 		// 结果为123
a = "123px123"; 			// 结果为123
let b = "bwe123ddf34"; 		// 结果为NaN
b = "    "; 				// 结果为NaN
b = ""; 					// 结果为NaN
```

`第二个参数，转化为不同的进制`

***3.parseFloat()函数***

和parseInt类似

### 1.4.3 转换为Boolean

- 使用Boolean()函数
  - 数字 —> 布尔
    - 除了0和NaN，其余的都是true
  - 字符串 —> 布尔
    - 除了空串，其余的都是true
  - null和undefined都会转换为false
  - 对象会转换为true

## 1.5 运算符

typeof就是运算符，可以来获得一个值的类型，它会将该值的类型以**字符串**的形式**返回**

### 1.5.1 算数运算符

* true为1，false为0，
* 任何数据类型和NaN做运算都得NaN

* **当对非Number类型的值进行运算时都会先转换为Number再运算**

* **任何值和String做加法运算，都会先转换为String再拼串(String之间做加法运算为直接拼串)**

```js
let c = 123 + "";	// 隐私类型转换，由浏览器完成，实际上还是调用String()函数, 结果c类型为string
let result = 100 - "1"; 			// 结果为99, (若为100 + "1", 则result类型为string)
result = 2 * "8"; 					// 结果为16
result = 2 * undefined; 			// NaN
result = 2 * NaN; 					// NaN
result = 2 * null; 					// 0
console.log(typeof result); 		// 以上result运算后类型均为number
```

任何值做`- * /`运算时都会自动转换为Number

```javascript
let a = "123" - 0;
// 隐式类型转换，可以通过-0, *1, /1, 来将其转换为Number
// 原理和Number()函数一样
```

### 1.5.2 一元运算符

`+ -`也是隐式运算

 **对于非Number类型的值, 它会先转换为Number, 然后再运算, 原理和Number()一样**

```js
let result = 1 + "2" + 3; 	// 结果为String的"123"
result = 1 + +"2" + 3; 		// 结果为Number的 6
```

### 1.5.3 逻辑运算符

**&&：**JS中的“与”属于短路的与，如果第一个为false，则直接返回第一个值，不会检查第二个值

如果第一个为true，则直接返回第二个值

- 非布尔值时：如果两个都为true，则返回第二个值，如果两个值中有false，则返回靠前的false的值

  ```js
  let result = 5 && 6; // 6
  result = 6 && 5; // 5
  result = 0 && 2; // 0
  result = 2 && 0; // 0
  result = NaN && 0; // NaN
  result = 0 && NaN; // 0
  ```

**||：**JS中的“或”属于短路的或，如果第一个为true，则直接返回第一个值，不会检查第二个值

如果第一个值为false，则直接返回第二个值

- 非布尔值时：如果两个都为false ，则返回第二个值，如果两个值中有true，则返回靠前的true的值

  ```javascript
  let result = 2 || 1; // 2;
  result = 2 || NaN; // 2;
  result = 2 || 0; // 2
  result = NaN || 1; // 1
  result = NaN || 0; // 0
  result = "" || "hello"; // "hello"
  result = 1 || "您好"; // 1
  ```

  

**!：**如果对一个值进行两次取反，它不会变化

- 非布尔值时：先会将其转换为布尔值，然后再取反，所以我们可以利用该特点，来将一个其它的数据类型转换为布尔值，**可以为一个任意数据类型取两次反，来将其转换为布尔值，原理和Boolean()函数一样**

### 1.5.4 赋值运算符

### 1.5.5 关系运算符

* 任何值和NaN做比较结果都是false


* 对于非数值进行比较时，先转换为数字然后再比较


* 如果两侧的值都是字符串，不会将其转换为数字进行比较，而会分别比较字符串中字符的ascii码，即按照字典序比较


```js
// 如果想比较两个String类型的数字, 必须先转型
console.log("12213124214142" < "5"); 		// true
console.log("12213124214142" < +"5"); 		// false
```

```js
// 在字符串中使用转义字符输入Unicode编码
console.log("\u2620");
```

### 1.5.6 比较运算符

```javascript
/*当使用==来比较两个值时，如果值的类型不同，则会自动进行类型转换，将其转换为相同的类型，然后在比较*/
console.log("1" == 1); 					// true
console.log(true == "1"); 				// true
console.log(null == 0); 				// false
// undefined衍生自null
console.log(undefined == null); 		// true

// NaN不和任何值相等，包括它本身
console.log(NaN == NaN); 				// false

// 检查变量是否为NaN
let a = NaN;
console.log(a == NaN); 					// false
console.log(isNaN(a)); 					// true

/*
=== 全等 不会进行类型转换
*/
console.log("123" == 123); 				// true
console.log("123" === 123); 			// false
console.log(null == undefined); 		// true
console.log(null === undefined); 		// false
```

```javascript
/*不相等用来判断两个值是否不相等，如果不相等返回true，否则返回false，不相等也会对变量进行自动的类型转换*/
console.log("1" != 1); 					// false
/*
!== 不全等 不会进行类型转换
*/
console.log(1 != "1");				 	// false
console.log(1 !== "1"); 				// true
```

### 1.5.7 条件运算符（三元运算符）

```
// 获取a b c中的最大值
let max = a > b ? (a > c ? a : c) : (b > c ? b : c);
```

## 1.6 运算符优先级

<img src="https://img-blog.csdnimg.cn/img_convert/695dff6b3d3117760c6fba7c0b09895b.png" alt="image-20201013115557984" style="zoom: 80%;" />

## 1.8 条件语句

switch...case...在执行时会依次将case后的表达式的值和switch后的条件表达式的值进行**全等比较**

## 1.9循环语句

```javascript
// 可以用prompt()函数来弹出警告框以和用户进行交互，输入数据
let score = null;
do {
	score = prompt();
} while (score < 0 || score > 100 || isNaN(score));
console.log(score);
```

```javascript
// 可以为循环语句创建一个标签，来标识当前的循环
outer: for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
        if (j == 5) break outer;
        console.log(j);
    }
}
```

```javascript
// 可以用console.time("计时器名字")来开启计时器
// 需要一个字符串作为参数，这个String会作为计时器的标识
console.time("runTime");
for (let i = 1; i <= 9; ++i) {
	for (let j = 1; j <= i; ++j) 
		document.write(i + " " + "*" + j + "=" + i * j + "\t");
	document.write("<br>");
}
console.timeEnd("runTime");
```

## 1.10 对象基础

```javascript
// 如果要使用特殊的属性名，不能采用.的方式
// 需要使用另一种方式，读取时也需要采用这种方式
// 创建属性时, 需要符合js变量命名规则
let obj = new Object();

obj.age = 23;
console.log(obj.age);		// 23
console.log(obj['age']); 	// 23

obj["name"] = "mary";
console.log(obj.name); 		// "mary"
console.log(obj['name']); 	// "mary"

obj["123"] = 1234;
console.log(obj["123"]);

// 检查obj中是否有name属性
console.log('age' in obj); 	// true
console.log("name" in obj); // true
console.log("123" in obj); 	// true
```

### 1.10.6 数据类型梳理

1. 栈内存用来保存变量和基本类型，堆内存是用来保存对象。

2. 基本数据类型的值是无法修改的，是不可变的。基本数据类型的比较是值的比较，也就是只要两个变量的值相等，我们就认为这两个变量的值相等。

3. 当一个变量是对象时，实际上变量中保存的并不是对象本身，而是对象的引用。当从一个变量向另一个变量复制引用类型的值时，会将对象的引用复制到变量中，并不是创建一个新的对象。这时，两个变量指向的是同一个对象，因此，改变其中一个变量会影响另一个。

```javascript
let obj1 = new Object();
obj1.name = "111";
let obj2 = obj1;
obj1.name = "222"; 
console.log(obj2.name); // "222"

obj2 = null;
console.log(obj1.name); // "222"
console.log(obj2); // null

let obj3 = new Object();
let obj4 = new Object();
obj3.name = "333";
obj4.name = "333";
// 不仅值相等，内存地址也要相等
console.log(obj3 == obj4); // false
```

```js
/* 
	使用对象字面量可以在创建对象时直接指定对象中的属性，也更方便
	和new Object()一样，但更简洁
	对象字面量的属性名可以加引号也可以不加，建议不加，
	如果要使用一些特殊的名字，则必须加引号
*/
// 多个键值对之间用,隔开，最后一个属性尽量不要写逗号, 
let obj = {
	name: "猪八戒", 
	age: 28,
	"132": 123,
    obj2: {name: "沙和尚"}
}; 
```

## 1.11 函数

要注意的是JavaScript中的函数也是一个对象，使用typeof检查一个函数对象时，会返回function。

### 1.11.2 函数创建

- 使用 **函数对象** 来创建一个函数（几乎不用）

```js
// 语法格式
let 函数名 = new Function("执行语句");
// 示例
let fun = new Function("console.log('这是我的第一个函数');");
```

- 使用 **函数声明** 来创建一个函数（比较常用）

```javascript
// 语法格式
function 函数名([形参1,形参2,...,形参N]) {
    语句...
}
// 示例
function fun(){
    console.log("这是我的第二个函数");
}

```

- 使用 **函数表达式** 来创建一个函数（比较常用）

```js
// 语法格式
let 函数名  = function([形参1,形参2,...,形参N]) {
    语句....
}
// 示例
let fun = function() {
    console.log("这是我的第三个函数");
}
```

### 1.11.4 函数参数

* JS中的所有的参数传递都是按值传递的，也就是说把函数外部的值赋值给函数内部的参数，就和把值从一个变量赋值给另一个变量是一样的，在调用函数时，可以在()中指定实参（实际参数），实参将会赋值给函数中对应的形参

- 调用函数时，解析器不会检查实参的类型，所以要注意，是否有可能会接收到**非法**的参数，如果有可能，则需要对参数进行类型的检查，函数的实参可以是任意的数据类型

- 调用函数时，解析器也不会检查实参的**数量**，多余实参不会被赋值，如果实参的数量少于形参的数量，则没有对应实参的形参将是undefined

### 1.11.5 函数返回值

**注意：**在函数中return后的语句都不会执行，如果return语句后不跟任何值就相当于返回一个undefined，如果函数中不写return，则也会返回undefined，return后可以跟任意类型的值。

### 1.11.8 立即执行函数

```js
(function (a, b) {
    console.log(a + b);
})(10, 20); // 30
```

### 1.11.9 对象中的函数

```javascript
/*

	如果一个函数作为对象的属性保存，那么我们称这个函数是这个对象的方法，
		调用这个函数即为调用对象的方法(method).
	注意：方法和函数只是名称上的区别，没有其它别的区别
*/
let person = {
    setInterval: true,
    name: "zhangsan",
    age: 18,
    sayHello: function() {
        console.log(this.name + "-->hello")
    }
}
person.sayHello();

// 遍历对象(这里的i为对象属性的索引值index, 而非真实值)
for (let i in person) {
    console.log(i + ":" + person[i]);
}
```

### 1.11.10 this对象

解析器在调用函数每次都会向函数内部传递进一个隐含的参数，这个隐含的参数就是this，this指向的是一个对象，这个对象我们称为函数执行的上下文对象，根据函数的调用方式的不同，this会指向不同的对象

- 以函数的形式调用时，this永远都是window
- 以方法的形式调用时，this就是调用方法的那个对象

```js
//创建一个全局变量name
let name = "全局变量name";

//创建一个函数
function fun() {
    console.log(this.name);
}

//创建一个对象
let obj = {
    name: "孙悟空",
    sayName: fun
};

fun(); // "全局变量name"
obj.sayName(); // "孙悟空"
```

## 1.12 作用域

### 1.12.1 全局作用域

在全局作用域中：

​	创建的变量都会作为window对象的属性保存

​	创建的函数都会作为window对象的方法保存

### 1.12.2 声明提前

- 变量的声明提前：使用var关键字声明的变量，会在所有的代码执行之前被声明（但是不会赋值），但是如果声明变量时不使用var关键字，则变量不会被声明提前。

  但是使用let关键字声明的变量如果不提前定义，则会报错：**在初始化之前无法访问“a”**

  ```js
  var a = 1;
  console.log("a:" + a); // "a:1"
  console.log("b:" + b); // "b:undefined"
  // Uncaught ReferenceError: Cannot access 'c' before initialization
  console.log("c:" + c); 
  // 无论c和b谁在上面都会执行
  let c = 3;
  var b = 2;
  ```

  ```js
  // 函数作用域中也类似
  function fun() {
  	let a = 1;
  	console.log("a:" + a + " b:" + b + " c:" + c);
  	var b = 2;
  	var c = 3;
  }
  fun();
  /*
  	"a:1 b:undefined c:undefined"
  	需要注意的是：如果把var c = 3; 改为 let c = 3;
  	则fun1()不会执行, 并报错:demo.html:54 Uncaught ReferenceError: Cannot access 'c' before initialization
  */
  ```

- 函数的声明提前：使用函数声明形式创建的函数 function 函数名(){} ，它会在所有的代码执行之前就被创建，所以我们可以在函数声明前来调用函数。**（此时就和var、let关键字没有关系）**使用函数表达式创建的函数，不会被声明提前，所以不能在声明前调用

```js
fun1(); // "fun1"
fun2(); // "Uncaught TypeError: fun2 is not a function"
fun3(); // "Uncaught ReferenceError: Cannot access 'fun3' before initialization"

// 且fun2()和fun3()不能同时写，谁在上面只会报谁的错误，如此例中fun3()不执行，原因是报错后下面的代码停止编译

function fun1() {
	console.log("fun1");
}
var fun2 = function() {
	console.log("fun2");
}
let fun3 = function() {
	console.log("fun3");
}
```

```js
// 在函数作用域中也类似
fun4(); // "fun5"
function fun4() {
	fun5();
	function fun5() {
		console.log("fun5");
	}
}
```



### 1.12.3 函数作用域

- 调用函数时创建函数作用域，函数执行完毕以后，函数作用域销毁
- 每调用一次函数就会创建一个新的函数作用域，它们之间是互相独立的
- **在函数作用域中可以访问到全局作用域的变量**，在全局作用域中无法访问到函数作用域的变量
- **在函数(嵌套)中要访问全局变量可以使用window对象**

- 作用域链：当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用，如果没有则向上一级作用域中寻找，直到找到全局作用域，如果全局作用域中依然没有找到，则会报错ReferenceError

```
let a = 1;
function fun1() {
	console.log("a:"a + " " + "b:" + b);
}
let b = 2;        
fun1(); // "a:1 b:2"
```

```
let a;
function fun() {
	a = 100; // 此时会修改全局变量
}
console.log(a); // "100"
```

## 1.13 对象进阶

### 1.13.1 用工厂方法创建对象

```
// 使用工厂模式创建对象
function createPerson(name, age) {
    // 创建新的对象
    var obj = new Object();
    // 设置对象属性
    obj.name = name;
    obj.age = age;
    // 设置对象方法
    obj.sayName = function () {
        console.log(this.name);
    };
    //返回新的对象
    return obj;
}

for (var i = 1; i <= 1000; i++) {
    var person = createPerson("person" + i, 18);
    console.log(typeof person);
}
```

### 1.13.2 构造函数

那构造函数创建对象的过程:

1. 调用构造函数，它会立刻创建一个新的对象

2. 将新建的对象设置为函数中this，在构造函数中可以使用this来引用新建的对象

3. 逐行执行函数中的代码（用户编写，其他三点均隐藏不可见）

4. 将新建的对象作为返回值返回

你会发现构造函数有点类似工厂方法，但是它创建对象和返回对象都给我们隐藏了，使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个类。我们将通过一个构造函数创建的对象，称为是该类的实例。

现在，this又出现了一种新的情况:

- 当以函数的形式调用时，this是window
- 当以方法的形式调用时，谁调用方法this就是谁
- [**new**]当以构造函数的形式调用时，this就是新创建的那个对象

```
// 使用构造函数来创建对象
function Person(name, age) {
    // 设置对象的属性
    this.name = name;
    this.age = age;
    // 设置对象的方法
    this.sayName = function () {
        console.log(this.name);
    };
}

var person1 = new Person("孙悟空", 18);
var person2 = new Person("猪八戒", 19);
var person3 = new Person("沙和尚", 20);

console.log(person1);
console.log(person2);
console.log(person3);

// 可以使用 instanceof 运算符检查一个对象是否是一个类的实例，它返回true或false，任何对象和Object进行检查都返回true

console.log(person1 instanceof Person); // "true"
```

### 1.13.3 原型

```
// 使用构造函数来创建对象
function Person(name, age) {
    // 设置对象的属性
    this.name = name;
    this.age = age;
    // 设置对象的方法
    this.sayName = sayName
}

// 抽取方法为全局函数
function sayName() {
    console.log(this.name);
}

var person1 = new Person("孙悟空", 18);
var person2 = new Person("猪八戒", 19);
var person3 = new Person("沙和尚", 20);

console.log(person1.sayName == person2.sayName); // "true"
/*
	当sayName方法在对象内部创建时，比较结果则为"false", 因为构造函数时没执行一次，
	就创建一个新的sayName方法
*/
```

但是，在全局作用域中定义函数却不是一个好的办法，如果要是涉及到多人协作开发一个项目，别人也有可能叫sayName这个方法，这样在工程合并的时候就会导致一系列的问题，污染全局作用域。此时就要用到原型（prototype）对象。

那原型（prototype）到底是什么呢？

我们所创建的每一个函数，解析器都会向函数中添加一个属性prototype，这个属性对应着一个对象，这个对象就是我们所谓的原型对象，即显式原型，原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问到这个原型对象，我们可以将对象中共有的内容，统一设置到原型对象中。

如果函数作为**普通函数**调用**prototype**没有任何作用，当函数以**构造函数**的形式调用时，它所创建的对象中都会有一个隐含的属性，指向该构造函数的原型对象，我们可以通过**\_\_proto\_\_**（隐式原型）来访问该属性。当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找。

以后我们创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中，这样不用分别为每一个对象添加，也不会影响到全局作用域，就可以使每个对象都具有这些属性和方法了。

```
// 使用构造函数来创建对象
function Person(name, age) {
    // 设置对象的属性
    this.name = name;
    this.age = age;
    // 设置对象的方法
    this.sayName = function () {
        console.log(this.name);
    };
}
Person.prototype.bianliang = 21321;
let p = new Person();
console.log(p.__proto__.bianliang);
```

### 1.13.4 原型链

使用in检查对象中是否含有某个属性时，如果对象中没有但是原型中有，也会返回true

可以使用对象的hasOwnProPerty()来检查对象自身中是否含有该属性

```
function Person() {

}
let p = new Person();
// 使用该方法只有当对象自身中含有属性时，才会返回true
console.log(p.hasOwnProperty("name")); // "false"
// 查看对象中是否有hasOwnProperty()方法
console.log(p.hasOwnProperty("hasOwnProperty")); // "false"
// 查看此对象的原型中是否有hasOwnProperty()方法，说明在上一级的原型
console.log(p.__proto__.hasOwnProperty("")); // "false"
// Object对象是所有对象的祖宗，Object的原型对象指向为null，也就是没有原型对象
```

<img src="https://img-blog.csdnimg.cn/img_convert/34f5a7f9ab460f148c64a66e8632634b.png" alt="image-20201016092628653" style="zoom:50%;" />

### 1.13.5 toString方法

```
// 使用构造函数来创建对象
function Person(name, age) {
    // 设置对象的属性
    this.name = name;
    this.age = age;
}

//创建对象的一个实例对象
var p = new Person("张三", 20);
console.log(p); // Person {name: '张三', age: 20}
console.log(p.toString()); // [object Object]

```

<img src="C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220405211100464.png" alt="image-20220405211100464" style="zoom: 80%;" />

```
function Person(name, age, gender) {
    this.name = name;
    this.age = age;
    this.gender = gender;
}
Person.prototype.toString = function() {
    return "Person[name = " + this.name + ", age = " + this.age + ", gender = " + this.gender + ']';
}

let p1 = new Person("p1", 11, "1");
let p2 = new Person("p2", 22, "2");

console.log(p1); // Person {name: 'p1', age: 11, gender: '1'}
console.log(p1.toString()); // Person[name = p1, age = 11, gender = 1]
console.log(p2.toString()); // Person[name = p2, age = 22, gender = 2]
```

### 1.13.6 垃圾回收（GC）

在JS中拥有自动的垃圾回收机制，会自动将这些垃圾对象从内存中销毁，我们不需要也不能进行垃圾回收的操作，我们需要做的只是要将不再使用的对象设置null即可。

### 1.13.7 继承

JavaScript有六种非常经典的对象继承方式，但是我们只学习前三种：

- **原型链继承**
- **借用构造函数继承**
- **组合继承（重要）**
- 原型式继承
- 寄生式继承
- 寄生组合式继承

#### 1.13.7.1 原型链继承

**核心思想：** 子类型的原型为父类型的一个实例对象

```
// 定义父类型构造函数
function SupperType() {
    this.supProp = 'Supper property';
}

// 给父类型的原型添加方法
SupperType.prototype.showSupperProp = function () {
    console.log(this.supProp);
};

// 定义子类型的构造函数
function SubType() {
    this.subProp = 'Sub property';
}

// 创建父类型的对象赋值给子类型的原型
SubType.prototype = new SupperType();

// 将子类型原型的构造属性设置为子类型
SubType.prototype.constructor = SubType;

// 给子类型原型添加方法
SubType.prototype.showSubProp = function () {
    console.log(this.subProp)
};

// 创建子类型的对象: 可以调用父类型的方法
var subType = new SubType();
subType.showSupperProp();
subType.showSubProp();
```

**缺点：**

1. 原型链继承多个实例的引用类型属性指向相同，一个实例修改了原型属性，另一个实例的原型属性也会被修改
2. 不能传递参数
3. 继承单一

#### 1.13.7.2 借用构造函数继承

**核心思想：** 使用.call()和.apply()将父类构造函数引入子类函数，使用父类的构造函数来增强子类实例，等同于复制父类的实例给子类

借用构造函数继承的重点就在于SuperType**.call(this, name)**，调用了SuperType构造函数，这样，SubType的每个实例都会将SuperType中的属性复制一份。

```
// 定义父类型构造函数
function SuperType(name) {
    this.name = name;
    this.showSupperName = function () {
        console.log(this.name);
    };
}

// 定义子类型的构造函数
function SubType(name, age) {
    // 在子类型中调用call方法继承自SuperType
    SuperType.call(this, name);
    this.age = age;
}

// 给子类型的原型添加方法
SubType.prototype.showSubName = function () {
    console.log(this.name);
};

// 创建子类型的对象然后调用
var subType = new SubType("孙悟空", 20);
subType.showSupperName();
subType.showSubName();
console.log(subType.name);
console.log(subType.age);

```

**缺点：**

1. 只能继承父类的实例属性和方法，不能继承原型属性和方法
2. 无法实现构造函数的复用，每个子类都有父类实例函数的副本，影响性能，代码会臃肿

#### 1.13.7.3 组合继承

**核心思想：** 原型链+借用构造函数的组合继承

**基本做法：**

1. 利用原型链实现对父类型对象的方法继承
2. 利用super()借用父类型构建函数初始化相同属性

```
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.setName = function (name) {
    this.name = name;
};

function Student(name, age, price) {
    Person.call(this, name, age); // 为了得到父类型的实例属性和方法
    this.price = price; // 添加子类型私有的属性
}

Student.prototype = new Person(); // 为了得到父类型的原型属性和方法
Student.prototype.constructor = Student; // 修正constructor属性指向
Student.prototype.setPrice = function (price) { // 添加子类型私有的方法 
    this.price = price;
};

var s = new Student("孙悟空", 24, 15000);
console.log(s.name, s.age, s.price);
s.setName("猪八戒");
s.setPrice(16000);
console.log(s.name, s.age, s.price);
```

**缺点描述：**

1. 父类中的实例属性和方法既存在于子类的实例中，又存在于子类的原型中，不过仅是内存占用，因此，在使用子类创建实例对象时，其原型中会存在**两份相同的属性和方法** 。

> 注意：**这个方法是JavaScript中最常用的继承模式**。

# 第二章 JavaScript常用对象

## 2.1 数组对象

### 2.1.4 数组属性

```
// constructor属性演示：返回创建数组对象的原型函数
var arr = [1,2,3,4];
console.log(arr.constructor); // Array() { [native code] }

// length属性演示：设置或返回数组元素的个数
let a = [1, 2, 3, 4];
console.log(a);
console.log(a.length); // 4
a.length = 3;
console.log(a); // [1, 2, 3]
console.log(a.length); // 3
a.length = 5; 
console.log(a); // [1, 2, 3, , ,];
console.log(a.length); // 5
a[a.length] = 6;
console.log(a); // [1, 2, 3, , , 6];
console.log(a.length); // 6   
```

### 2.1.5 数组方法

```javascript
// push()
var arr = [1, 2, 3]
var result = arr.push(4, 5, 6, "last");
console.log(arr); // [1, 2, 3, 4, 5, 6, 7]
console.log(result); // 7, 返回数组的长度

// pop()
result = arr.pop();
console.log(arr); // [1, 2, 3, 4, 5, 6]
console.log(result);  // "last"，返回最后一个被删除的元素

// unshift()
result = arr.unshift("first");
console.log(arr); // ["first", 1, 2, 3, 4, 5, 6];
console.log(result); // 7， 返回数组长度

// shift()
result = arr.shift();
console.log(arr); // [1, 2, 3, 4, 5, 6];
console.log(result); // "first"，返回第一个被删除的元素

// slice(start, end)
result = arr.slice(2, 4);
console.log(arr); // [1, 2, 3, 4, 5, 6]; 不改变原数组
console.log(result); // [3, 4] 将结果封装到新数组中返回
result = arr.slice(2); // 返回到index:2到结尾的所有元素
console.log(result); // [3, 4, 5, 6];
result = arr.slice(-1); // 返回从倒数第一个元素到end的位置，也就是返回最后一个元素
console.log(result); // [6]
result = arr.slice(-1, -2); // 如果start <= end，则返回空数组
console.log(result); // []

// concat() （不会对原数组产生影响）
var arr = ["孙悟空", "猪八戒", "沙和尚"];
var arr2 = ["白骨精", "玉兔精", "蜘蛛精"];
var arr3 = ["二郎神", "太上老君", "玉皇大帝"];
var result = arr.concat(arr2, arr3, "牛魔王", "铁扇公主");

// ['孙悟空', '猪八戒', '沙和尚', '白骨精', '玉兔精', '蜘蛛精', '二郎神', '太上老君', '玉皇大帝', '牛魔王', '铁扇公主']
console.log(result);

/* join():该方法可以将数组转换为一个字符串，该方法不会对原数组产生影响，而是将转换后的字符串作为结果返回，在join()中可以指定一个字符串作为参数，这个字符串将会成为数组中元素的连接符，如果不指定连接符，则默认使用，作为连接符  */
var arr = ["孙悟空", "猪八戒", "沙和尚"];
var result = arr.join("@-@");
console.log(result); // "孙悟空@-@猪八戒@-@沙和尚"

// reverse()
var arr = [1, 2, 3];
arr.reverse(); // 反转数组
console.log(arr); // [3, 2, 1]
```



**forEach()方法：该方法可以用来遍历数组**

```
// forEach()
// 注：这个方法只支持IE8以上的浏览器，IE8及以下的浏览器均不支持该方法，所以如果需要兼容IE8，则不要使用forEach()，还是使用for循环来遍历数组

let cnt = 0;
arr.forEach(function(it) {
	console.log("arr[" + cnt++ + "]:" + it);
}); // arr[0]:1 arr[1]:2 ...... arr[5]:6
```

浏览器会在回调函数中传递三个参数：

- 第一个参数：就是当前正在遍历的元素

- 第二个参数：就是当前正在遍历的元素的索引

- 第三个参数：就是正在遍历的数组

  ```
  var arr = ["孙悟空", "猪八戒", "沙和尚"];
  arr.forEach(function (value, index, obj) {
      console.log(value + " #### " + index + " #### " + obj);
  });
  /*
  	孙悟空 #### 0 #### 孙悟空,猪八戒,沙和尚
  	猪八戒 #### 1 #### 孙悟空,猪八戒,沙和尚
  	沙和尚 #### 2 #### 孙悟空,猪八戒,沙和尚
  */
  ```

**splice()方法**：该方法可以用于删除数组中的指定元素，该方法会影响到原数组，会将指定元素从原数组中删除，并将被删除的元素作为返回值返回

参数：

- 第一个参数：**表示开始位置的索引**
- 第二个参数：**表示要删除的元素数量**
- 第三个参数及以后参数：可以传递一些新的元素，**这些元素将会自动插入到开始位置索引前边**

```
var arr = ["孙悟空", "猪八戒", "沙和尚", "唐僧", "白骨精"];
var result = arr.splice(3, 2);
console.log(arr); // ['孙悟空', '猪八戒', '沙和尚']
console.log(result); // ['唐僧', '白骨精']
result = arr.splice(1, 0, "牛魔王", "铁扇公主", "红孩儿");
console.log(arr); // ['孙悟空', '牛魔王', '铁扇公主', '红孩儿', '猪八戒', '沙和尚']
console.log(result); // []
```

**数组去重：**

```
let a = [1, 2, 1, 3, 1, 4, 1, 2, 2, 5, 5, 5, 6];
for (let i = 0; i < a.length - 1; ++i) {
	for (let j = i + 1; j < a.length; ++j) {
		if (a[i] == a[j]) {
			a.splice(j, 1);
            // 确保长度减小后数组下标前移的元素会被遍历到
            j--;
        }
    }
}
```

**sort()方法：该方法可以用来对数组中的元素进行排序，也会影响原数组，默认会按照Unicode编码进行排序，所以即使对于纯数字的数组，使用sort()排序时，也会按照Unicode编码来排序，所以对数字进排序时，可能会得到错误的结果**

```
var arr = [1, 3, 2, 11, 5, 6];
arr.sort();
console.log(arr); // [1, 11, 2, 3, 5, 6]

arr.sort(function (a, b) {
    return a - b;
});
console.log(arr); // [1, 2, 3, 5, 6, 11]

arr.sort(function (a, b) {
    return b - a;
});
console.log(arr); // [11, 6, 5, 3, 2, 1]

```

## 2.2 函数对象

### 2.2.1 call()和apply()

在调用call()和apply()可以将一个对象指定为第一个参数，**此时这个对象将会成为函数执行时的this**，call()方法可以将实参在对象之后依次传递，apply()方法需要将实参封装到一个数组中统一传递

注意：默认fun()函数调用，this指向的是window对象，你可以使用call()调用函数，在调用的时候传入一个对象，这个对象就是this所指向的对象，也就是说，可以自己指定this的指向，然后从第二个参数开始，实参将会依次传递，而apply()的第二个参数开始，需要制定一个实参数组进行参数传递

```
function fun(a, b) {
    console.log("a = " + a);
    console.log("b = " + b);
    console.log("fun = " + this);
}

var obj = {
    name: "obj",
    sayName: function () {
        console.log(this.name);
    }
};

fun(2, 3); // a = 2 b = 3 fun = [object Window]

fun.call(obj, 2, 3); // a = 2 b = 3 fun = [object object]

fun.apply(obj, [2, 3]); // a = 2 b = 3 fun = [object object]
```

### 2.2.2 this指向

- 以函数形式调用时，this永远都是window
- 以方法的形式调用时，this是调用方法的对象
- 以构造函数的形式调用时，this是新创建的那个对象
- 使用call和apply调用时，this是传入的那个指定对象

### 2.2.3 arguments参数

在调用函数时，浏览器每次都会传递进两个隐含的参数：

1. 函数的上下文对象： **this**
2. 封装实参的对象： **arguments**

arguments是一个类数组对象，它也可以通过索引来操作数据，也可以获取长度，在调用函数时，我们所传递的实参都会在arguments中保存，比如：arguments.length 可以用来获取实参的长度，我们即使不定义形参，也可以通过arguments来使用实参，只不过比较麻烦，例如：arguments[0], arguments[1]

属性callee，这个属性对应一个函数对象，就是当前正在指向的函数的对象。

```
function fun(a, b) {
    // 通过下标获取第一个参数
    console.log(arguments[0]);
    // 通过下标获取第二个参数
    console.log(arguments[1]);
    // 获取实参的个数
    console.log(arguments.length);
    // 获取的函数对象
    console.log(arguments.callee);
    console.log(arguments.callee == fun);
}

fun("Hello", "World");
```

## 2.3 Date对象

```
let d = new Date();
console.log(d); // 当前的时间

// 指定时间需要在构造函数中传递一个表示时间的字符串作为函数
// 格式 月份/日/年 时:分:秒
let d2 = new Date("12/03/2021 2:23:11");
console.log(d2); // Fri Dec 03 2021 02:23:11 GMT+0800

// getDate() 获取当前对象是几日
let date = d2.getDate();
console.log(data); // 3

// getDay() 获取当前对象是周几
date = d2.getDay(); // 返回0-6，0表示周日
console.log(date); // 5

// getMonth() 获取当前对象的月份，返回0-11，0是一月，2是二月
date = d2.getMonth();
console.log(date) // 11

// getFullYear(); // 获取当前对象的年份
date = d2.getFullYear(); // getYear()方法已经废弃
console.log(date); // 2021

// getTime() 获取当前对象的时间戳(便于保存到计算机)
// 时间戳：指的是从格林威治时间的1970年1月1日，0时0分0秒到当前日期所花费的毫秒数
date = d2.getTime();
console.log(date); // 1638469391000

let d3 = new Date("1/1/1970 0:0:0");
time = d3.getTime();
console.log(time); // -28800000于北京时间相差8小时

// 获取当前时间戳
date = Date.now();
console.log(date); // 1649294578596
// 可以利用时间戳来计算代码运行时间
let start = Date.now();
for (let i = 0; i < 100; ++i) console.log(" ");
let end = Date.now();
console.log(end-start); // 单位是毫秒
```

## 2.4 Math对象

Math和其它的对象不同，它不是一个构造函数，**它属于一个工具类不用创建对象**，它里边封装了数学运算相关的属性和方法

```

console.log("PI = " + Math.PI); // 3.141592653589793
console.log("E  = " + Math.E); // 2.718281828459045
console.log("===============");

console.log(Math.abs(1));  			// 1
//可以对一个数进行向上取整，小数位只有有值就自动进1
console.log(Math.ceil(1.1)); 		// 2
//可以对一个数进行向下取整，小数部分会被舍掉
console.log(Math.floor(1.99)); 		// 1
//可以对一个数进行四舍五入取整
console.log(Math.round(1.4)); 		// 1
console.log(Math.abs(-1)); 			// 1
console.log(Math.ceil(-1.1)); 		// -1
console.log(Math.floor(-1.99)); 	// -2
console.log(Math.round(-1.4));   	// -1
console.log("===============");

//Math.random()：可以用来生成一个(0, 1)之间的随机数
//生成一个0-x之间的随机数：Math.round(Math.random()*x)
//生成一个x-y之间的随机数：Math.round(Math.random()*(y-x)+x)
//生成一个[0, 10]之间的随机数
console.log(Math.round(Math.random() * 10));
//生成一个1-10之间的随机数
console.log(Math.round(Math.random() * (10 - 1) + 1));
console.log("===============");

/*数学运算*/
//Math.pow(x,y)：返回x的y次幂
console.log(Math.pow(12, 3)); // 1728
//Math.sqrt(x) ：返回x的平方根
console.log(Math.sqrt(4)); // 2
//Math.max() 返回多个数的最大值
console.log(Math.max(1, 2, 3, 6, 19)); // 19
//Math.min() 返回多个数的最小值
console.log(Math.min(1, 2, 3, 6, 19)); // 1
```

## 2.5 String对象

在JS中为我们提供了三个包装类，通过这三个包装类可以将基本数据类型的数据转换为对象

- String()：可以将基本数据类型字符串转换为String对象

- Number()：可以将基本数据类型的数字转换为Number对象

- Boolean()：可以将基本数据类型的布尔值转换为Boolean对象

  但是注意：我们在实际应用中不会使用基本数据类型的对象，如果使用基本数据类型的对象，在做一些比较时可能会带来一些不可预期的结果，在这一章节中，我们重点介绍String()对象的属性和方法。

```javascript
// charAt() : 和用索引index访问字符一样
let str = "Hello World";
console.log(str.charAt(1)); // 'e'

// charCodeAt() : 获取指定位置字符的字符编码（Unicode编码）
console.log(str.charCodeAt(1)); // 101

// String.fromCharCode() : 可以根据字符编码去获取字符
console.log(String.fromCharCode(101)); // 'e'

// concat() : 连接两个或多个字符串
let temp = str;
console.log(temp.concat("dawd")); // "Hello Worlddawd"

// indexOf() : 检索一个字符串中是否含有指定内容，返回第一次出现的位置，没有找到则返回-1
console.log(str.indexOf('W')); // 6
// 从index为3的位置开始寻找
console.log(str.indexOf('l', 3)); // 3 
// lastIndexOf() : 从后往前找
console.log(str.lastIndexOf('l')); // 9

// slice() : 和数组中的slice()类似，也可以传递一个负数作为参数，负数的话将会从后边计算
console.log(str.slice(1, 3)); // "el"

// substring() : 和slice()类似，不同的是这个方法不能接受负值作为参数，如果传递了一个负值，则默认为0，而且它还自动调整参数的位置，如果第二个参数小于第一个，则自动交换
console.log(str.substring(1, 3)); // "el"

// substr() : 截取字符串，第二个参数表示要截取的长度
console.log(str.substr(3, 2)); // "lo";

// split() : 将字符串拆分成数组，需要一个字符串作为参数，将会根据该字符串去拆分
// 如果第二个参数是""空串，则会将每个字符都拆分
let str2 = "adw, dwada, wd, awdaw, d, aw";
console.log(str2.split(", ")); // ['adw', 'dwada', 'wd', 'awdaw', 'd', 'aw']

// toUpperCase() : 将字符串转换为大写
console.log(str.toUpperCase()); // "HELLO WORLD"

// toLowerCase() : 将字符串转化为小写
console.log(str.toLowerCase()); // "hello world"

```

## 2.6 RegExp(正则表达式)对象

### 2.6.2 创建正则对象

```
// 使用对象创建。
/* 匹配模式：	 i：忽略大小写
				g：全局匹配模式
				ig：忽略大小写且全局匹配模式 */
// 这种方式中的正则表达式可以传参数，更加灵活
var 变量名 = new RegExp("正则表达式","匹配模式");

// 使用字面量创建
// 注意：可以为一个正则表达式设置多个匹配模式，且顺序无所谓
/* 匹配模式：	 i：忽略大小写
				g：全局匹配模式
				m：执行多行匹配式 */
var 变量名 = /正则表达式/匹配模式;

```

### 2.6.3 正则进阶

**使用 | 表示或者的意思检查一个字符串中是否有a或b**

```
var reg = /a|b|c/;
var str = "Abc";
var result = reg.test(str);
console.log(result); // true
```

```
// 这个正则表达式可以来检查一个字符串中是否含有字母
var reg = /[A-z]/;
var str = "Abc";
var result = reg.test(str);
console.log(result); // true

// 检查一个字符串中是否含有 abc 或 adc 或 aec
var reg = /a[bde]c/;
var str = "abc123";
var result = reg.test(str);
console.log(result); // true
```

[a-z]：任意小写字母		[A-Z]：任意大写字母

[A-z]：任意字母				[0-9]：任意数字

[^a-z]：除了任意小写字母   		[ ^A-Z]：除了任意大写字母

[^A-z]：除了任意字母 				[ ^0-9]：除了任意数字

### 2.6.4 正则方法

**split()中可以传递一个正则表达式作为参数，这样方法将会根据正则表达式去拆分字符串，这个方法即使不指定全局匹配，也会全都拆分**

```
var str = "1a2b3c4d5e6f7";
var result = str.split(/[A-z]/);
console.log(result); // ['1', '2', '3', '4', '5', '6', '7']
```

**search()方法可以搜索字符串中是否含有指定内容，如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到返回-1，它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串，serach()只会查找第一个，即使设置全局匹配也没用**

```
var str = "hello abc hello aec afc";
var result = str.search(/a[bef]c/);
console.log(result); // 6
```

**match()方法可以根据正则表达式，从一个字符串中将符合条件的内容提取出来，默认情况下我们的match()只会找到第一个符合要求的内容，找到以后就停止检索，我们可以设置正则表达式为全局匹配模式，这样就会匹配到所有的内容，可以为一个正则表达式设置多个匹配模式，且顺序无所谓，match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果**

```
var str = "1a2a3a4a5e6f7A8B9C";
var result = str.match(/[a-z]/ig);
console.log(result); // ['a', 'a', 'a', 'a', 'e', 'f', 'A', 'B', 'C']
```

**replace()方法可以将字符串中指定内容替换为新的内容，默认只会替换第一个，但是可以设置全局匹配替换全部**

```
var str = "1a2a3a4a5e6f7A8B9C";
var result = str.replace(/[a-z]/gi, "@_@");
console.log(result); // 1@_@2@_@3@_@4@_@5@_@6@_@7@_@8@_@9@_@
```

### 2.6.5 正则量词

通过量词可以设置一个内容出现的次数，量词只对它前边的一个内容起作用，如果有多个内容可以使用 `()` 括起来，常见量词如下

- `{n}` ：正好出现n次
- `{m,}` ：出现m次及以上
- `{m,n}` ：出现m-n次
- `+` ：至少一个，相当于{1,}
- `*` ：0个或多个，相当于{0,}
- `?` ：0个或1个，相当于{0,1}

### 2.6.6 正则高阶

如果我们要检查或者说判断是否以某个字符或者字符序列开头或者结尾就会使用`^`和`$`。

- `^` ：表示开头，注意它在`[^字符序列]`表达的意思不一样
- `$` ：表示结尾

```
// 检查一个字符串中是否以a开头
var str = "abcabca";
var reg = /^a/;
console.log(reg.test(str)); // true

// 检查一个字符串中是否以a结尾
var str = "abcabca";
var reg = /a$/;
console.log(reg.test(str)); // true

// 如果在正则表达式中同时使用^ $，则要求字符串必须完全符合正则表达式
var reg = /^a$/;
console.log(res.test("aaaa")); // false
console.log(res.test("a")); // true;
```

如果我们想要检查一个字符串中是否含有`.`和`\`就会使用转义字符

- `\.` ：表示`.`
- `\\` ：表示`\`

> 注意：使用构造函数时，由于它的参数是一个字符串，而`\`是字符串中转义字符，如果要使用`\`则需要使用`\\`来代替

- `\w` ：任意字母、数字、\_   相当于[A-z0-9_]
- `\W` ：除了字母、数字、\_    相当于[ ^A-z0-9_]
- `\d` ：任意的数字，相当于`[0-9]`
- `\D` ：除了任意的数字，相当于`[^0-9]`
- `\s` ：空格
- `\S` ：除了空格
- `\b` ：单词边界
- `\B` ：除了单词边界

```
// 去除字符串前后的空格
var str = "  hello child  "
var reg = /^\s*|\s*$/g;
str = str.replace(reg, "");
console.log(str); // hello child

// 检查字符串中是否含有单词child
var str = "hello child"
var reg = /\bchild\b/;
console.log(reg.test(str)); // true
```

### 2.6.7 正则案例

```javascript
// 检查手机号
let reg = /^1[3-9]\d{9}$/;
let phone = "15343948573";
console.log(reg.test(phone)); // true

// 检查邮箱号
var emailStr = "abc.def@163.com";
var emailReg = /^\w{3,}(\.\w+)*@[A-z0-9]+(\.[A-z]{2,5}){1,2}$/;
console.log(emailReg.test(emailStr)); // true
```

# 第三章 JavaScript DOM

## 3.2 DOM文档节点

节点Node，是构成我们网页的最基本的组成部分，网页中的每一个部分都可以称为是一个节点。

比如：html标签、属性、文本、注释、整个文档等都是一个节点。

常用节点分为四类：

- 文档节点：整个HTML文档
- 元素节点：HTML文档中的HTML标签
- 属性节点：元素的属性
- 文本节点：HTML标签中的文本内容

<img src="https://img-blog.csdnimg.cn/img_convert/6c58b926da64501f486305b5afe3d9f8.png" alt="image-20201019115515595" style="zoom:50%;" />

### 3.2.2 节点属性

<img src="https://img-blog.csdnimg.cn/img_convert/2271dc3f0f61e890fe4cb76817fc6942.png" alt="image-20201019115655478" style="zoom: 67%;" />

### 3.2.3 文档节点

文档节点（Document）代表的是整个HTML文 档，网页中的所有节点都是它的子节点。

document对象作为window对象的属性存在的，我们不用获取可以直接使用。

通过该对象我们可以在整个文档访问内查找节点对象，并可以通过该对象创建各种节点对象。

### 3.2.4 元素节点

HTML中的各种标签都是元素节点（Element），这也是我们最常用的一个节点。

浏览器会将页面中所有的标签都转换为一个元素节点， 我们可以通过document的方法来获取元素节点。

例如：`document.getElementById()`，根据id属性值获取一个元素节点对象。

### 3.2.5 属性节点

属性节点（Attribute）表示的是标签中的一个一个的属 性，这里要注意的是属性节点并非是元素节点的子节点，而是元素节点的一部分。可以通过元素节点来获取指定的属性节点。

例如：`元素节点.getAttributeNode("属性名")`，根据元素节点的属性名获取一个属性节点对象。

**注意：我们一般不使用属性节点。**

### 3.2.6 文本节点

文本节点（Text）表示的是HTML标签以外的文本内容，任意非HTML的文本都是文本节点，它包括可以字面解释的纯文本内容。文本节点一般是作为元素节点的子节点存在的。获取文本节点时，一般先要获取元素节点，再通过元素节点获取文本节点。

例如：`元素节点.firstChild;`，获取元素节点的第一个子节点，一般为文本节点。



## 3.3 DOM文档操作

### 3.3.1 查找HTML元素

| 方法                                  | 描述                      |
| :------------------------------------ | ------------------------- |
| document.getElementById(id)           | 通过元素 id 来查找元素    |
| document.getElementsByTagName(name)   | 通过标签名来查找元素      |
| document.getElementsByClassName(name) | 通过类名来查找元素        |
| document.querySelector(CSS选择器)     | 通过CSS选择器选择一个元素 |
| document.querySelectorAll(CSS选择器)  | 通过CSS选择器选择多个元素 |



```javascript
<body>
<button id="btn">我是按钮</button>
<button class="btn2">我是按钮2</button>
<ul class="list">
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
    <li>列表项4</li>
</ul>
<script>
    // 通过id获取按钮节点对象
    let btn = document.getElementById("btn");
    console.log(btn);
    // 通过标签名获取按钮节点对象数组
    btn = document.getElementsByTagName("button");
    console.log(btn);
    // 通过类名获取按钮节点对象数组
    btn = document.getElementsByClassName("btn");
    console.log(btn);
    // 通过CSS选择器选择按钮
    btn = document.querySelector(".btn2");
    console.log(btn);
    
    // 创建一个无序列表，通过CSS选择器选择该列表的所有li
    let list = document.querySelectorAll(".list li");
    console.log(list);
</script>
</body>
```

![image-20220408175417519](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220408175417519.png)

### 3.3.2 获取 HTML 的值

| 方法                             | 描述                     |
| -------------------------------- | ------------------------ |
| 元素节点.innerText               | 获取HTML元素的innerText  |
| 元素节点.innerHTMl               | 获取HTML元素的innerHTML  |
| 元素节点.属性                    | 获取HTML元素的属性值     |
| 元素节点.getAttribute(attribute) | 获取HTML元素的属性值     |
| 元素节点.style.样式              | 获取HTML元素的行内样式值 |

```javascript
<button id="btn">我是按钮</button>
<div id="box">
    <h1>我是Box中的h1标题</h1>
</div>
<a id="a" href="https://www.baidu.com">打开百度，你就知道！</a>
<div id="box2" style="width:100px;height:100px;background:red;"></div>
<script>
    // 获取按钮的文本内容
    let btn = document.getElementById("btn");
    console.log(btn.innerText);
    // 获取div中的html代码
    let box1 = document.getElementById("box");
    console.log(box1.innerHTML);
    // 读取超链接中href属性
    let a = document.getElementById("a");
    console.log(a.href);
    a = document.getElementById("a");
    console.log(a.getAttribute("href"));
    // 获取div的宽度
    let box2 = document.getElementById("box2");
    console.log(box2.style.width);
</script>
```

![image-20220409103926177](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409103926177.png)

**拓展：**

通过style属性设置和读取的都是内联样式，无法读取样式表中的样式或者正在应用的样式，如果想要读取当前正在应用的样式属性我们可以使用`元素.currentStyle.样式名`来获取元素的当前显示的样式，如果当前元素没有设置该样式，则获取它的默认值，**但是currentStyle只有IE浏览器支持，其它的浏览器都不支持**，在其它浏览器中可以使用`getComputedStyle()`这个方法来获取元素当前的样式，这个方法是window的方法，可以直接使用，但是需要两个参数：

- 第一个参数：要获取样式的元素
- 第二个参数：可以传递一个伪元素，一般都传null

该方法会返回一个对象，对象中封装了当前元素对应的样式，可以通过`对象.样式名`来读取样式，如果获取的样式没有设置，则会获取到真实的值，而不是默认值，比如：没有设置width，它不会获取到auto，而是一个长度，但是该方法不支持IE8及以下的浏览器。通过currentStyle和getComputedStyle()读取到的样式都是只读的，不能修改，如果要修改必须通过style属性。

- **编写一段兼容性代码，用来适配各个浏览器的读取元素样式的方法**。

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        /*样式表的样式*/
        #box {
            width: 200px;
            height: 200px;
            background-color: green;
        }
    </style>
</head>
<body>
<div style="width: 100px;height: 100px;" id="box"></div>
<script>
    /*通用的获取元素样式的方法*/
    function getStyle(obj, name) {
        if (window.getComputedStyle) {
            //正常浏览器的方式，具有getComputedStyle()方法
            return getComputedStyle(obj, null)[name];
        } else {
            //IE8的方式，没有getComputedStyle()方法
            return obj.currentStyle[name];
        }
    }
    var box = document.getElementById("box");
    console.log(getStyle(box, "width"));
    console.log(getStyle(box, "height"));
    console.log(getStyle(box, "background-color"));
</script>
</body>
</html>
```

![image-20220409105640072](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409105640072.png)

- **编写一段兼容性代码，用来获取任意标签的文本内容**

```
<body>
<a href="https://www.baidu.com" id="a">打开百度，你就知道！</a>
<script>
    var a = document.getElementById("a");
    console.log(getInnerText(a));
    /*获取任意标签的内容*/
    function getInnerText(element) {
        // 判断浏览器是否支持textContent,如果支持，则使用textContent获取内容，否则使用innerText获取内容。
        if(typeof element.textContent == "undefined") {
            return element.innerText;
        } else {
            return element.textContent;
        }
    }
</script>
</body>
```

### 3.3.3 改变 HTML 的值

| 方法                                        | 描述                       |
| ------------------------------------------- | -------------------------- |
| 元素节点.innerText = *new text content*     | 改变元素的 inner Text      |
| 元素节点.innerHTML = *new html content*     | 改变元素的 inner HTML      |
| 元素节点.属性 = *new value*                 | 改变 HTML 元素的属性值     |
| 元素节点.setAttribute(*attribute*, *value*) | 改变 HTML 元素的属性值     |
| 元素节点.style.样式 = *new style*           | 改变 HTML 元素的行内样式值 |

```
<body>
    <button id="btn">我是按钮</button>
    <div id="box"></div>
    <a id="a" href="">打开百度，你就知道！</a>
    <div id="box2" style="width:100px;height:100px;background-color:red"></div>
<script>
    // 改变按钮的文本内容
    let btn = document.getElementById("btn");
    btn.innerText = "我是JavaScript的按钮";
    console.log(btn);
    // 在div中插入一个h1标题
    let box = document.getElementById("box");
    box.innerHTML = "<h1>我是Box中的大标题</h1>";
    console.log(box);
    // 1.修改a标签中href属性
    let a = document.getElementById("a");
    a.href = "https://www.baidu.com";
    console.log(a);
    // 2.修改a标签中href属性
    a.setAttribute("href", "https://www.baidu.com");
    console.log(a);
    // 将默认颜色改为绿色
    let box2 = document.getElementById("box2");
    box2.style.backgroundColor = "green";
    console.log(box2);
</script>
</body>
```

![image-20220409111545231](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409111545231.png)

**注意**：如果CSS的样式名中含有-，这种名称在JS中是不合法的比如background-color，**需要将这种样式名修改为驼峰命名法，去掉-，然后将-后的字母大写。**我们通过style属性设置的样式都是行内样式，同样的获取也是行内样式，而行内样式有较高的优先级，所以通过JS修改的样式往往会立即显示，**但是如果在样式中写了!important，则此时样式会有最高的优先级，即使通过JS也不能覆盖该样式，**此时将会导致JS修改样式失效，所以尽量不要为样式添加!important。

**拓展：**

修改节点的内容除了常用的innerHTML和innerText之外，还有insertAdjacentHTML和insertAdjacentText方法，可以在指定的地方插入内容。insertAdjacentText方法与insertAdjacentHTML方法类似，只不过是插入纯文本，参数相同。

语法说明：

```
object.insertAdjacentHTML(where,html);
object.insertAdjacentText(where,text)
```

参数说明：

- html：一段html代码
- text：一段文本值

- where：

![img](https://img-blog.csdnimg.cn/img_convert/dae7b01cf5aa68e8b71b939b4aea1a9b.png)

注意事项：

1. 这两个方法必须等文档加载好后才能执行，否则会出错。
2. insertAdjacentText只能插入普通文本，insertAdjacentHTML插入html代码。
3. 使用insertAdjacentHTML方法插入script脚本文件时，必须在script元素上定义defer属性。
4. 使用insertAdjacentHTML方法插入html代码后，页面上的元素集合将发生变化。
5. insertAdjacentHTML方法不适用于单个的空的元素标签(如img，input等)。

```
<body>
    <div id="insert"><p>小苹果</p></div>
<script>
    let div = document.getElementById("insert");
    div.insertAdjacentHTML("beforeBegin", "小苹果");
</script>
</body>
```

![image-20220409113547463](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409113547463.png)

- **编写一段兼容性代码，用来设置任意标签的文本内容**

```javascript
<body>
    <a id="a" href="https://www.baidu.com">打开百度，你就知道！</a>
<script>
    let a = document.getElementById("a");
    console.log(getInnerText(a));
    setInnerText(a, "你要打开百度吗？");
    
    // 获取任意标签的内容
    function getInnerText(element) {
        // 判断浏览器是否支持textContent，如果支持，则使用textContent获取内容，否则innerText获取内容
        if (typeof element.textContent == "undefined") {
            return element.innerText = text;
        } else {
            return element.textContent;
        }
    }

    // 设置任意标签的内容
    function setInnerText(element, text) {
        // 判断浏览器是否支持textContent，如果支持，则使用textContent获取内容，否则innerText获取内容
        if (typeof element.textContent == "undefined") {
            return element.innerText = text;
        } else {
            return element.textContent = text;
        }
    }
</script>
</body>
```

![image-20220409114758152](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409114758152.png)

![image-20220409114748018](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409114748018.png)

### 3.3.4 修改 HTML 元素

| 方法                                  | 描述                             |
| ------------------------------------- | -------------------------------- |
| document.createElement(*element*)     | 创建 HTML 元素节点               |
| document.createAttribute(*attribute*) | 创建 HTML 属性节点               |
| document.createTextNode(*text*)       | 创建 HTML 文本节点               |
| 元素节点.removeChild(*element*)       | 删除 HTML 元素                   |
| 元素节点.appendChild(*element*)       | 添加 HTML 元素                   |
| 元素节点.replaceChild(*element*)      | 替换 HTML 元素                   |
| 元素节点.insertBefore(*element*)      | 在指定的子节点前面插入新的子节点 |

```
<script>
    // 创建一个ul列表，然后在列表中追加4个li标签
    let ul = document.createElement("ul");
    // 方法1
    let li1 = document.createElement("li");
    let text1 = document.createTextNode("列表项1");
    li1.appendChild(text1);
    ul.appendChild(li1);
    // 方法2
    let li2 = document.createElement("li");
    li2.innerHTML = "列表项2";
    ul.appendChild(li2);
    // 方法3
    let li3 = "<li>列表项3</li>";
    ul.innerHTML += li3;

    document.getElementsByTagName("body")[0].appendChild(ul);
</script>
```

![image-20220409123102052](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409123102052.png)

```
// 创建一个ul列表，里边有四个li子元素，删除第一个li，替换最后一个li
<body>
    <ul id="ul">
        <li id="first">列表项1</li>
        <li>列表项2</li>
        <li>列表项3</li>
        <li id="last">列表项4</li>
    </ul>
<script>
    let ul = document.getElementById("ul");
    let first = document.getElementById("first");
    let last = document.getElementById("last");
    // 删除第一个
    ul.removeChild(first);
    // 替换最后一个
    let replaceLi = document.createElement("li");
    replaceLi.innerHTML = "列表项4的替换";
    ul.replaceChild(replaceLi, last);
</script>
</body>
```

![image-20220409123643333](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409123643333.png)

**拓展：**

动态判断、添加、删除、切换样式，支持IE5-IE11，谷歌浏览器、火狐浏览器等

### 3.3.5 查找 HTML 父子

| 方法                            | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| 元素节点.parentNode             | 返回元素的父节点                                             |
| 元素节点.parentElement          | 返回元素的父元素                                             |
| 元素节点.childNodes             | 返回元素的一个子节点的数组（包含空白文本Text节点）           |
| 元素节点.children               | 返回元素的一个子元素的集合（不包含空白文本Text节点）         |
| 元素节点.firstChild             | 返回元素的第一个子节点（包含空白文本Text节点）               |
| 元素节点.firstElementChild      | 返回元素的第一个子元素（不包含空白文本Text节点）             |
| 元素节点.lastChild              | 返回元素的最后一个子节点（包含空白文本Text节点）             |
| 元素节点.lastElementChild       | 返回元素的最后一个子元素（不包含空白文本Text节点）           |
| 元素节点.previousSibling        | 返回某个元素紧接之前节点（包含空白文本Text节点）             |
| 元素节点.previousElementSibling | 返回指定元素的前一个兄弟元素（相同节点树层中的前一个元素节点） |
| 元素节点.nextSibling            | 返回某个元素紧接之后节点（包含空白文本Text节点）             |
| 元素节点.nextElementSibling     | 返回指定元素的后一个兄弟元素（相同节点树层中的下一个元素节点） |

```javascript
<body>
<div id="container">
    <p>前面的P标签</p>
    <b>加粗文本</b>
    <a id="a" href="https://www.baidu.com">百度一下</a>
    <i>斜体文本</i>
    <p>最后的P标签</p>
</div>

<script>

    // 第一个元素
    let firstNode = getfirstElementChild(document.getElementById("container"));
    console.log(firstNode.innerHTML);

    // 最后一个元素
    let lastNode = getLastElementChild(document.getElementById("container"));
    console.log(lastNode.innerHTML);

    // 指定元素的前一个子元素
    let node1 = getPreviousElementSibling(document.getElementById("a"));
    console.log(node1.innerHTML);

    // 指定元素的后一个子元素
    let node2 = getNextElementSibling(document.getElementById("a"));
    console.log(node2.innerHTML);

    // 获取任意一个父级元素的第一个子元素
    function getfirstElementChild(element) {
        if (element.firstElementChild) {
            return element.firstElementChild;
        } else {
            let node = element.firstChild;
            while (node && node.nodeType != 1) {
                node = node.nextSibling;
            }
            return node;
        }
    }

    // 获取任意一个父级元素的最后一个子元素
    function getLastElementChild(element) {
        if (element.lastElementChild) {
            return element.lastElementChild;
        } else {
            let node = element.lastElementChild;
            while (node && node.nodeType != 1) {
                node = node.previousSibling;
            }
            return node;
        }
    }

    // 获取任意一个子元素的前一个兄弟元素
    function getPreviousElementSibling(element) {
        if (element.previousElementSibling) {
            return element.previousElementSibling;
        } else {
            let node = element.previousSibling;
            while (node && node.nodeType != 1) {
                node = node.previousSibling;
            }
            return node;
        }
    }
    
    // 获取任意一个子元素的后一个元素
    function getNextElementSibling(element) {
        if (element.nextElementSibling) {
            return element.nextElementSibling;
        } else {
            let node = element.nextSibling;
            while (node && node.nodeType != 1) {
                node = node.nextSibling;
            }
            return node;
        }
    }
</script>
</body>
```

## 3.4 DOM文档事件

### 3.4.2 窗口事件

由窗口触发该事件 (同样适用于 <body> 标签)

| 属性      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| onblur    | 当窗口失去焦点时运行脚本                                     |
| onfocus   | 当窗口获得焦点时运行脚本                                     |
| onload    | 当文档加载之后运行脚本                                       |
| onresize  | 当调整窗口大小时运行脚本                                     |
| onstorage | 当Web Storage区域更新时（存储空间中的数据发生变化时）运行脚本 |

```javascript
<script>
	window.onload = function() {
        console.log("窗口加载完毕");
    };
	window.onfocus = function() {
        console.log("窗口获取焦点");
    };
    window.onblur = function() {
        console.log("窗口失去焦点");
    };
    window.onresize = function() {
        console.log("窗口大小正在改变");
    };
</script>
```

### 3.4.3 表单事件

表单事件在HTML表单中触发 (适用于所有 HTML 元素，但该HTML元素需在form表单内)

| 属性      | 描述                         |
| --------- | ---------------------------- |
| onblur    | 当元素失去焦点时运行脚本     |
| onfocus   | 当元素获得焦点时运行脚本     |
| onchange  | 当元素改变时运行脚本         |
| oninput   | 当元素获得用户输入时运行脚本 |
| oninvalid | 当元素无效时运行脚本         |
| onselect  | 当选取元素时运行脚本         |
| onsubmit  | 当提交表单时运行脚本         |

```javascript
<body>
<form id="myform">
    <!-- required 自带的非空判断功能，加上了之后单击submit的警告信息才会生效 -->
    <input id="text" type="text" required>
    <input type="submit" value="submit">
</form>
<script>
    let textInput = document.getElementById("text");
    
    // 当文本框获取焦点，文本框背景为绿色
    textInput.onfocus = function() {
        this.style.background = "yellowgreen";
    };

    // 当文本框失去焦点，文本框背景为红色
    textInput.onblur = function() {
        this.style.background = "red";
    };
    
    // 当文本框内容改变时，鼠标离开文本框，自动将文本框的内容输出到控制台
    textInput.onchange = function() {
        console.log(this.value);
    };

    // 当文本框内容改变时，立即将改变的内容输出到控制台
    textInput.oninput = function() {
        console.log(this.value);
    };

    // 如果单击submit而不填写文本字段，将发生警告信息
    textInput.oninvalid = function() {
        console.log("请您完善表单内容！");
    };

    // 当选中文本框的内容时，输出"您已经选择了文本框内容"
    textInput.onselect = function() {
        console.log("您已经选择了文本框内容！");
    };

    // 当提交表单时，在控制台输出"表单提交"
    let myform = document.getElementById("myform");
    myform.onsubmit = function() {
        console.log("表单提交");
        return false; // 用来阻止表单提交，不写会跳转请求
    };
</script>
</body>
```

### 3.4.4 键盘事件

| 属性       | 描述                       |
| ---------- | -------------------------- |
| onkeydown  | 当按下按键时运行脚本       |
| onkeyup    | 当松开按键时运行脚本       |
| onkeypress | 当按下饼松开按键时运行脚本 |

**判断当前按键是不是'a':**

```javascript
<script>
	window.onkeydown = function(event) {
		// 解决兼容性问题
		event = event || window.event;
		if (event.keyCode == 65) {
			console.log("true");
		} else {
			console.log("false");
		}
	};
</script>
```

**使div可以根据不同的方向键向不同的方向移动：**

```
<body>
    <div id="box" style="width:100px;height:100px;background:red;position:absolute;"></div>
<script>
	let box = document.getElementById("box");

    document.onkeydown = function(event) {
        event = event || window.event;

        // 定义移动速度
        let speed = 10;

        // 选择移动方向
        switch(event.keyCode) {
            case 37:
                box.style.left = box.offsetLeft - speed + "px";
                break;
            case 39:
                box.style.left = box.offsetLeft + speed + "px";
                break;
            case 38:
                box.style.top = box.offsetTop - speed + "px";
                break;
            case 40:
                box.style.top = box.offsetTop + speed + "px";
                break;
        }
    };
</script>
</body>
```

**拓展：**

当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数。

Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标的状态。

在IE8中，响应函数被触发时，浏览器不会传递事件对象，在IE8及以下的浏览器中，是将事件对象作为window对象的属性保存的。

解决事件对象的兼容性问题：`event = event || window.event;`

**键鼠属性：**

| 属性     | 描述                                       |
| -------- | ------------------------------------------ |
| ctrlKey  | 返回当事件被触发时，"CTRL"键是否被按下     |
| altKey   | 返回当事件被触发时，“ALT” 是否被按下       |
| shiftKey | 返回当事件被触发时，“SHIFT” 键是否被按下   |
| clientX  | 返回当事件被触发时，鼠标指针的水平坐标     |
| clientY  | 返回当事件被触发时，鼠标指针的垂直坐标     |
| screenX  | 返回当某个事件被触发时，鼠标指针的水平坐标 |
| screenY  | 返回当某个事件被触发时，鼠标指针的垂直坐标 |

### 3.4.5 鼠标事件

通过鼠标触发事件，类似用户的行为：

| 属性         | 描述                                               |
| ------------ | -------------------------------------------------- |
| onclick      | 当单击鼠标时运行脚本                               |
| ondblclick   | 当双击鼠标时运行脚本                               |
| onmousedown  | 当按下鼠标按钮时运行脚本                           |
| onmouseup    | 当松开鼠标按钮时运行脚本                           |
| onmousemove  | 当鼠标指针移动时运行脚本                           |
| onmouseover  | 当鼠标指针移至元素之上时运行脚本（不可以阻止冒泡） |
| onmouseout   | 当鼠标指针移出元素时运行脚本（不可以阻止冒泡）     |
| onmouseenter | 当鼠标指针移至元素之上时运行脚本（可以阻止冒泡）   |
| onmouseleave | 当鼠标指针移出元素时运行脚本（可以阻止冒泡）       |
| onmousewheel | 当转动鼠标滚轮时运行脚本                           |
| onscroll     | 当滚动元素的滚动条时运行脚本                       |

- **创建一个正方形div，默认颜色为黑色，当鼠标移入div，背景颜色变为红色，当鼠标移出div，背景颜色变为绿色**

```
<body>
<div id="box" style="width:100px;height:100px;background:black"></div>
<script>
	let box = document.getElementById("box");
	// 当鼠标移入div，背景颜色变为红色
	box.onmouseenter = function() {
    	this.style.background = "red";
	};
	box.onmouseleave = function() {
    	this.style.background = "green";
	};
</script>
</body>
```

- **编写一个通用的拖拽元素函数，创建两个div，进行拖拽演示，要求兼容IE8、火狐、谷歌等主流浏览器**

```javascript
<body>
<div id="box1" style="width: 100px;height: 100px;background: red;position: absolute;"></div>
<div id="box2" style="width: 100px;height: 100px;background: green;position: absolute;"></div>

<script>
    var box1 = document.getElementById("box1");
    var box2 = document.getElementById("box2");

    drag(box1);
    drag(box2);

    /*
     * 提取一个专门用来设置拖拽的函数
     * 参数：开启拖拽的元素
     */
    function drag(obj) {
        //当鼠标在被拖拽元素上按下时，开始拖拽
        obj.onmousedown = function (event) {
            // 解决事件的兼容性问题
            event = event || window.event;

            // 设置obj捕获所有鼠标按下的事件
            /**
             * setCapture()：
             * 只有IE支持，但是在火狐中调用时不会报错，
             * 而如果使用chrome调用，它也会报错
             */
            obj.setCapture && obj.setCapture();

            // obj的偏移量 鼠标.clentX - 元素.offsetLeft
            // obj的偏移量 鼠标.clentY - 元素.offsetTop
            var ol = event.clientX - obj.offsetLeft;
            var ot = event.clientY - obj.offsetTop;

            // 为document绑定一个鼠标移动事件
            document.onmousemove = function (event) {
                // 解决事件的兼容性问题
                event = event || window.event;
                // 当鼠标移动时被拖拽元素跟随鼠标移动
                // 获取鼠标的坐标
                var left = event.clientX - ol;
                var top = event.clientY - ot;
                // 修改obj的位置
                obj.style.left = left + "px";
                obj.style.top = top + "px";
            };

            // 为document绑定一个鼠标松开事件
            document.onmouseup = function () {
                // 取消document的onmousemove事件
                document.onmousemove = null;
                // 取消document的onmouseup事件
                document.onmouseup = null;
                // 当鼠标松开时，取消对事件的捕获
                obj.releaseCapture && obj.releaseCapture();
            };

            /*
             * 当我们拖拽一个网页中的内容时，浏览器会默认去搜索引擎中搜索内容，
             * 此时会导致拖拽功能的异常，这个是浏览器提供的默认行为，
             * 如果不希望发生这个行为，则可以通过return false来取消默认行为，
             * 但是这招对IE8不起作用
             */
            return false;
        };
    }
</script>
</body>
```



### 3.4.6 媒体事件

通过视频（videos），图像（images）或音频（audio） 触发该事件

![image-20220409172502947](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220409172502947.png)

### 3.4.7 其他事件

| 属性     | 描述                                |
| -------- | ----------------------------------- |
| onshow   | 当<menu>元素在上下文显示时触发      |
| ontoggle | 当用户打开或关闭<details>元素时触发 |

### 3.4.8 事件冒泡

事件的冒泡（Bubble）：所谓的冒泡指的就是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发，在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡可以通过事件对象来取消冒泡。

- **创建两个div，叠放在一起，分别绑定单击事件，点击最里面的div，会触发两个div的单击事件**

```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        #div1 {
            width: 200px;
            height: 200px;
            background: pink;
        }
        #div2 {
            width: 100px;
            height: 100px;
            background: coral;
        }
    </style>
</head>
<body>
<div id="div1">
    我是div1
    <div id="div2">
        我是div2
    </div>
</div>
<a id="a" href="https://www.baidu.com">打开百度，你就知道！</a>
<script>
    let div1 = document.getElementById("div1");
    let div2 = document.getElementById("div2");

    // 为div1绑定单击事件
    div1.onclick = function() {
        console.log("div1的单击事件触发了！");
    	// stopBubble();
    };

    // 为div2绑定单击事件
    div2.onclick = function() {
        console.log("div2的单击事件触发了！");
		// stopBubble();
    };

	// 取消事件冒泡，点击最里面的div不会触发两个div的点击事件
    function stopBubble(event) {
        // 如果提供了事件对象，则这是一个非IE浏览器
        if (event && event.stopPropagation) {
            // 因此它支持W3C的stopPropagation()方法
            event.stopPropagation();
        } else {
            // 否则，我们需要使用IE的方式来取消事件冒泡
            window.event.cancelBubble = true;
        }
    }
	
/*------------当点击a标签的时候，阻止a标签的默认跳转事件，采用事件阻止---------*/
	let a= document.getElementById("a");

    // 为a绑定单击事件
    a.onclick = function() {
        stopDefault();
    };

    // 阻止浏览器的默认行为
    function stopDefault(event) {
        if (event && event.preventDefault) {
            // 阻止浏览器动作
            event.preventDefault();
        } else {
            // IE中阻止浏览器默认动作的方式
            window.event.returnValue = false;
        }
        return false;
    }
</script>
</body>
</html>
```

### 3.4.9 事件委派

我们希望只绑定一次事件，即可应用到多个的元素上，即使元素是后添加的，我们可以尝试将其绑定给元素的共同的祖先元素，也就是事件的委派。事件的委派，是指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件。事件委派是利用了事件冒泡，通过委派可以减少事件绑定的次数，提高程序的性能。

- **为ul列表中的所有a标签都绑定单击事件**

```javascript
<body>
<ul id="ul">
    <li><a href="javascript:;" class="link">超链接1</a></li>
    <li><a href="javascript:;" class="link">超链接2</a></li>
    <li><a href="javascript:;" class="link">超链接3</a></li>
</ul>
<script>
    let ul = document.getElementById("ul");

    // 为ul绑定一个单击相应函数
    ul.onclick = function(event) {
        event = event || window.event;
        // 如果触发事件的对象是我们期望的元素，则执行，否则不执行
        if (event.target.className == "link") {
            console.log("我是ul的单击相应函数");
        }
    };
</script>
</body>
```

### 3.4.10 事件绑定

我们以前绑定事件代码只能一个事件绑定一个函数，那我们要是想一个事件对应多个函数，并且不存在兼容性的问题该如何解决呢？

```javascript
/*为元素绑定事件兼容性代码*/
function addEventListener(element, type, fn) {
	if(element.addEventListener) {
		// 正常浏览器
        element.addEventListener(type, fn, false);
	} else if(element.attachEvent) {
		// IE8
        element.attachEvent("on" + type, fn);
	} else {
		element["on" + type] = fn;
	}
}

/*为元素解绑事件兼容性代码*/
function removeEventListener(element, type, fnName) {
	if(element.removeEventListener) {
		element.removeEventListener(type, fnName, false);
	} else if(element.detachEvent) {
		element.detachEvent("on" + type, fnName);
	} else {
		element["on" + type] = null;
	}
}
```

- 为按钮1的单击事件绑定两个函数，然后点击按钮2取消按钮1的单击事件绑定函数f1

```javascript
<body>
<button id="btn1">按钮1</button>
<button id="btn2">按钮2</button>
<script>
    function f1() {
        console.log("output1...");
    };
    function f2() {
        console.log("output2...");
    };

    // 为按钮1的单击事件绑定两个函数
    addEventListener(document.getElementById("btn1"), "click", f1);
    addEventListener(document.getElementById("btn1"), "click", f2);

    // 点击按钮2取消按钮1的单击事件绑定函数f1
    document.getElementById("btn2").onclick = function() {
        removeEventListener(document.getElementById("btn1"), "click", f1);
    };

    /*为元素绑定事件兼容性代码*/
    function addEventListener(element, type, fn) {
        if (element.addEventListener) {
            element.addEventListener(type, fn, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, fn);
        } else {
            element["on" + type] = fn;
        }
    }

    /*为元素解绑事件兼容性代码*/
    function removeEventListener(element, type, fnName) {
        if (element.removeEventListener) {
            element.removeEventListener(type, fnName, false);
        } else if (element.detachEvent) {
            element.detachEvent("on" + type, fnName);
        } else {
            element["on" + type] = null;
        }
    }
</script>
</body>
```



### 3.4.11 事件传播

1. 捕获阶段：在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件
2. 目标阶段：事件捕获到目标元素，捕获结束开始在目标元素上触发事件
3. 冒泡阶段：事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件

<img src="https://img-blog.csdnimg.cn/img_convert/606369d19295725d727fd8a04285f3e2.png" alt="image-20201021145200137" style="zoom:67%;" />

注意：如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true，一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false，并且注意，IE8及以下的浏览器中没有捕获阶段，我们可以使用`event.stopPropagation();`取消事件传播。

# 第四章 JavaScript BOM

## 4.1 BOM概述

常见BOM对象：

- Window：代表的是整个浏览器的窗口，同时window也是网页中的全局对象
- Navigator：代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器
- Location：代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，或者操作浏览器跳转页面
- History：代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录，由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页，而且该操作只在当次访问时有效
- Screen：代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息

这些BOM对象在浏览器中都是作为window对象的属性保存的，可以通过window对象来使用，也可以直接使用

## 4.2 Window对象

### 4.2.1 弹出框

**JavaScript 有三种类型的弹出框：警告框、确认框和提示框。**

```javascript
<script>
    // 三个方法都可以不写window前缀
    /*
        警告框:
        如果要确保信息传递给用户，通常会使用警告框。当警告框弹出时，用户将需要单击"确定"来继续.
        语法：window.alert("sometext");
    */
    alert("我是一个警告框");
    /*
        确认框：
        如果希望用户验证或接受某个东西，则通常使用确认框.
        当确认框弹出时，用户将不得不单击"确定"或"取消"来继续.
        如果用户单击"确定"，该框返回true，如果用户单击"取消"，该框返回false.
        语法：window.confirm("sometext");    
    */
    let r = confirm("请按按钮");
    if (r == true) {
        console.log("确认！");
    } else {
        console.log("取消！");
    }
    /*
        提示框：
        如果希望用户在进入页面前输入值，通常会使用提示框
        当提示框弹出时，用户将不得不输入值后单击"确定"或点击"取消"来继续
        如果用户单击"确认"，该框返回输入值。如果用户单击"取消"，则该框返回NULL
        语法：window.prompt("sometext", "defalutText");    
    */
    let person = prompt("请输入您的姓名", "比尔盖茨");
    if (person != null) {
        console.log(person);
    } else {
        console.log("取消！");
    }
</script>
```

### 4.2.2 定时事件

**JavaScript可以在时间间隔内执行，这就是所谓的定时事件。**

window对象允许以指定的时间间隔执行代码，这些时间间隔称为定时事件。

通过JavaScript使用的有两个关键的方法（都属于window对象）：

- setTimeout(*function*, *milliseconds*)

  在等待指定的毫秒数后执行函数。

- setInterval(*function*, *milliseconds*)

  等同于 setTimeout()，但持续重复执行该函数。

```javascript
<body>
<button id="btn">按钮</button>
<button id="btn2">按钮2</button>
<script>
    /**
     * setTimeout()方法：延时器
     * 语法：window.setTimeout(function, milliseconds);
     * 第一个参数：要执行的函数
     * 第二个参数：指示执行之前的毫秒数
    */
    // 单击按钮，等待3秒，然后控制台会输出"hello"
    let btn = document.getElementById("btn");
    btn.onclick = function() {
        // 创建延时器
        let timer = setTimeout(function() {
            console.log("延迟的Hello");
        }, 3000);
        
        // 清除延时器
        // clearTimeout(timer);
    };

    /**
     * setInterval()方法：定时器
     * setInterva()方法在每个给定的时间间隔重复给定的函数
     * 语法：window.setInterval(funciton, milliseconds);
     * 第一个参数：执行的函数
     * 第二个参数：每个执行之间的时间间隔
    */

   // 单击按钮，每隔1s向控制台输出"Hello"
   let btn2 = document.getElementById("btn2");
   btn2.onclick = function() {
        // 创建定时器
        let timer = setInterval(function() {
            console.log("Hello");
        }, 1000);

        // 清除定时器
        // clearInterval(timer);
    };
</script>
</body>
```

**拓展：**

做一个通用移动函数来实现小汽车（黑色方块）移动的效果

### 4.2.3 常用窗口属性

两个属性可用于确定浏览器窗口的尺寸

这两个属性都以像素返回尺寸：

- window.innerHeight - 浏览器窗口的内高度（以像素计）
- window.innerWidth - 浏览器窗口的内宽度（以像素计）

浏览器窗口（浏览器视口）不包括工具栏和滚动条。

对于 Internet Explorer 8, 7, 6, 5：

- document.documentElement.clientHeight
- document.documentElement.clientWidth

或

- document.body.clientHeight
- document.body.clientWidth

**一个实用的 JavaScript 解决方案（包括所有浏览器）：该例显示浏览器窗口的高度和宽度（不包括工具栏和滚动条）**

```javascript
<script>
    var w = window.innerWidth
        || document.documentElement.clientWidth
        || document.body.clientWidth;

    var h = window.innerHeight
        || document.documentElement.clientHeight
        || document.body.clientHeight;

    console.log(w);
    console.log(h);
</script>
```

### 4.2.4 其它窗口方法

```javascript
<body>
<button onclick="openWin()">打开窗口</button>
<button onclick="closeWin()">关闭窗口</button>
<button onclick="moveWin()">移动窗口</button>
<button onclick="resizeWin()">调整窗口</button>
<script>
    /**
     * window.open()：打开新的窗口
     * 语法：window.open(URL, name, specs, replace);
     * 
    */
   function openWin() {
       myWindow = window.open("", "", "width=200, height=100");
       myWindow.document.write("<p>新建的窗口</p>");
   }

   /**
    * window.close()：关闭当前窗口
    * 
   */
    function closeWin() {
        myWindow.close();
    }

    /**
     * window.moveTo(x, y)：移动当前窗口
     * 语法：window.moveTo(x, y);
    */
   function moveWin() {
       myWindow.moveTo(300, 300);
       myWindow.focus();
   }

   /**
    * window.resizeTo()：调整当前窗口
    * 语法：window.resizeTo(width, height);
   */
  function resizeWin() {
      myWindow.resizeTo(300, 300);
      myWindow.focus();
  }
</script>
</body>
```

## 4.3 Navigator对象

Navigator代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器，由于历史元素，Navigator对象中的大部分属性都已经不能帮助我们识别浏览器了，一般我们只会使用userAgent来判断浏览器的信息，userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，不同的浏览器会有不同的userAgent，如下代码：

```javascript
let ua = navigator.userAgent;
console.log(ua);
```

```javascript
let ua = navigator.userAgent;
if (/firefox/i.test(ua)) {
	alert("火狐浏览器");
} else if (/chrome/i.test(ua)) {
	alert("谷歌浏览器");
} else if (/msie/i.test(ua)) {
	alert("IE5-IE10浏览器");
} 
/* 在IE11中已经将微软和IE相关的标识都已经去除了，所以我们基本已经不能通过UserAgent来识别一个浏览器是否是IE了，如果通过UserAgent不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息，比如：ActiveXObject */
else if ("ActiveXObject" in window) {
	alert("IE11");
}
```

## 4.4 Location对象

Location对象中封装了浏览器的地址栏信息，如果直接打印location，则可以获取到地址栏的信息（当前页面的完整路径）

### 4.4.1 常用属性

```javascript
/**
 * 修改地址
 * location = "https://www.baidu.com";
 * location.href = "https://www.baidu.com";
*/
console.log(location);          // 输出location对象
console.log(location.href);     // 输出当前地址的全路径地址
console.log(location.origin);   // 输出当前地址的来源
console.log(location.protocol); // 输出当前地址的协议
console.log(location.hostname); // 输出当前地址的主机名
console.log(location.host);     // 输出当前地址的主机
console.log(location.port);     // 输出当前地址的端口号
console.log(location.pathname); // 输出当前地址的路径部分
console.log(location.search);   // 输出当前地址的?后边的参数部分
```

![image-20220410112428536](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220410112428536.png)

### 4.4.2 常用方法

assign()：用来跳转到其他的页面，作用和直接修改location一样

```
location.assign("https://www.baidu.com");
```

reload()：用于重新加载当前页面，作用和刷新按钮一样，如果在方法中传递一个true，作为参数，则会强制清空缓存刷新页面

```javascript
location.reload(true);
```

replace()：可以使用一个新的页面替换当前页面，调用完毕也会跳转页面，它不会生成历史记录，不能使用回退按钮回退

```javascript
location.replace("https://www.baidu.com");
```

## 4.5 History对象

History对象可以用来操作浏览器向前或向后翻页

### 4.5.1 常用属性

```javascript
console.log(history); 			// 输出history对象
console.log(history.length);	// 可以获取到当前访问的连接数量
```

### 4.5.2 常用方法

`history.back()`：可以回退到上一个页面，作用和浏览器的回退按钮一样

`history.forward()`：可以跳转到下一个页面，作用和浏览器的前进按钮一样

`history.go()`：可以用来跳转到指定的页面，它需要一个整数作为参数

- 1：表示向前跳转一个页面，相当于forward()
- 2：表示向前跳转两个页面
- -1：表示向后跳转一个页面，相当于back()
- -2：表示向后跳转两个页面

## 4.6 Screen 对象

Screen对象包含有关客户端显示屏幕的信息。虽然没有应用于screen对象的公开标准，不过所有浏览器都支持该对象。

每个Window对象的screen属性都引用一个Screen对象。Screen对象中存放着有关显示浏览器屏幕的信息。JavaScript将利用这些信息来优化它们的输出，以达到用户的显示要求。例如，一个程序可以根据显示器的尺寸选择使用大图像还是使用小图像，它还可以根据显示器的颜色深度选择使用16位色还是使用8位色的图形。另外，JavaScript程序还能根据有关屏幕尺寸的信息将新的浏览器窗口定位在屏幕中间。

**属性：**

![image-20220410155208944](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220410155208944.png)

# 第五章 JavaScript高级语法





# 第六章 ECMAScript6的新特性

## 6.1 let 关键字

let 关键字用来声明变量，使用 let 声明的变量有几个特点：

- 不允许重复声明
- 块儿级作用域
- 不存在变量提升
- 不影响作用域链（函数调用变量的寻找）

> 注意：以后声明变量使用 let 就对了

**练习：点击div使其变色**

```javascript
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div .item {
            width: 100px;
            height: 100px;
            border: 1px black solid;
            float: left;
            margin-right: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2 class="page-header">点击切换颜色</h2>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
</body>

<script>
    // 获取div元素对象
    let items = document.getElementsByClassName("item");
    for (let i = 0; i < items.length; ++i) { // 使用let定义
        items[i].onclick = function() {
            // 正确写法
            // this.style.background = 'pink'; 
            items[i].style.background = 'pink';
        }
    }
</script>
```

## 6.2 const 关键字

const 关键字用来声明常量，const 声明有以下特点：

- 一定要赋初始值
- 一般常量使用大写
- 常量的值不能修改
- 块儿级作用域
- 对于数组和对象的元素修改（如果指向的地址不变），不算做对常量的修改，不会报错

## 6.3 变量的解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构赋值。

> 注意：频繁使用对象方法、数组元素，就可以使用解构赋值形式，以简化代码

**数组的解构赋值：**

```javascript
char arr = ["张学友"，"刘德华"，"黎明","郭富城"];
let [zhang, liu, li, guo] = arr;
console.log(zhang);
console.log(liu);
console.log(li);
console.log(guo);
```

**简单对象的解构赋值：**

```javascript
const lin = {
	name: "林志颖",
	tags: ["车手","歌手","小旋风","演员"]
};
let {name, tags} = lin;
console.log(name);
console.log(tags);
```

**复杂对象的解构赋值：**

```javascript
let wangfei = {
	name: "王菲",
	age: 18,
	songs: ["红豆","流年","暧昧"],
	history: [
		{name: "窦唯"},
		{name: "李亚鹏"},
		{name: "谢霆锋"}
	]
};
let {name, age, songs: [one, two, three], history:[first, second, third]} = wangfei;
console.log(name);
console.log(age);
console.log(one);
console.log(two);
console.log(three);
console.log(first);
console.log(second);
console.log(third);
```

## 6.4 模板字符串

模板字符串（template string）是增强版的字符串，用反引号（`）标识，特点：

- 字符串中可以出现换行符
- 可以使用 ${xxx} 形式输出变量

> 注意：当遇到字符串与变量拼接的情况使用模板字符串

```javascript
<script>
	// 模板字符串
    // 1.声明
    // let str = `字符串1`;
    // console.log(str, typeof str);

    // 2.内容重可以直接出现换行符
    let str = `<ul>
                <li>沈腾</li>
                <li>玛丽</li>
                <li>魏翔</li>
                <li>艾伦</li>
                </ul>`
    
    // 3.变量拼接
    let lovest = '魏翔';
    let out = `${lovest}是我心目中最搞笑的演员！`;
    console.log(out);
</script>
```

## 6.5 简化对象写法

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法，这样的书写更加简洁。

> 注意：对象简写形式简化了代码，所以以后用简写就对了

```javascript
    let name = '尚硅谷';
    let change = function() {
        console.log('我们可以改变你！');
    }

    const school = {
        name, 
        change,
        // improve: function() {
        improve() {
            console.log("我们可以提高你的技能");
        }
    }
    console.log(school);
```

## 6.6 箭头函数

ES6 允许使用「箭头」（=>）定义函数，通用写法如下：

箭头函数的注意点：

- 如果形参只有一个，则小括号可以省略
- 函数体如果只有一条语句，则花括号可以省略，函数的返回值为该条语句的执行结果
- 箭头函数 this 指向声明时所在作用域下 this 的值，箭头函数不会更改 this 指向，用来指定**回调函数**会非常合适
- 箭头函数不能作为构造函数实例化
- 不能使用 arguments 实参

```javascript
<script>
    // ES6 允许使用 => 定义函数
    // 声明一个函数
    // let fu = function() {

    // }
    // let fn = (a, b) => {
    //     return a + b;
    // }

    // 调用函数
    // let result = fn(1, 2);
    // console.log(result);

    // 1.this 是静态的,this始终指向函数声明时所在作用域下的this的值
    function getName() {
        console.log(this.name);
    }

    let getName2 = () => {
        console.log(this.name);
    }

    // 设置window对象的name属性
    window.name = '尚硅谷';
    const school = {
        name: 'atguigu'
    }

    // 直接调用
    // getName();  // "尚硅谷"
    // getName2(); // "尚硅谷"

    // call 方法调用
    getName.call(school);    // "aiguigu"
    getName2.call(school);   // "尚硅谷"

    // 2.不能作为构造函数实例化对象
    // let Person = (name, age) => {
    //     this.name = name;
    //     this.age = age;
    // }

    // let me = new Person('xiao', 30);
    // console.log(me); // TypeError: Person is not a constructor

    // 3.不能使用arguments变量
    // let fn = () => {
    //     console.log(arguments);
    // }
    // fn(1, 2, 3); // ReferenceError: arguments is not defined

    // 4.箭头函数的简写
        // 1.省略小括号,当形参有且只有一个时
        let add = n => {
            return n + n;
        }
        console.log(add(9));

        // 2.省略花括号,当代码体只有一条语句时,此时return可以省略
        // 而且语句的执行结果就是函数的返回值
        let pow = n => n * n;
        console.log(pow(9));
</script>
```

**箭头函数的实践与使用场景：**

- 箭头函数适合与`this`无关的回调。如：定时器，数组方法的回调；
- 箭头函数不适合与`this`有关的回到。如：事件回调，对象的方法；

```javascript
<script>
    // 需求1 点击div 2s后 颜色变成粉色
    // 获取元素
    let ad = document.getElementById("ad");
    // 绑定事件
    ad.addEventListener("click", function() {
        // 保存this的值常用变量名：(that, _this, self)
        let _this = this;
        // 定时器
        setTimeout(function() {
            // 修改背景颜色 this
            // this指向有问题,指向window
            // this.style.background = 'pink';
            _this.style.background = 'pink';
        }, 2000)

        // 新解决方案
        // setTimeout(() => { // 此时指向的是事件源ad的this
        //     this.style.background = 'pink';
        // }, 2000);
    });

    // 需求2 从数组重返回偶数的元素
    const arr = [1, 6, 9, 10, 100, 23];
    /*
        filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表，
        接收两个参数，一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判断
        
        filter() 函数用于过滤序列序列的每个元素作为参数传递给函数进行判断，
        然后返回 True 或 False，最后将返回 True 的元素放到新列表中。
    */
    // 解决方案1
    // const result = arr.filter(function(item) {
    //     if (item % 2 === 0) {
    //         return true;
    //     } else {
    //         return false;
    //     }
    // });
    
    // 解决方案2
    // const result = arr.filter(item => {
    //     if (item % 2 === 0) {
    //         return true;
    //     } else {
    //         return false;
    //     }
    // });

    // 简化方案2
    const result = arr.filter(item => item % 2 === 0);
    console.log(result);
</script>
```



## 6.7 函数参数的默认值设置

ES6 允许给函数参数赋值初始值。

```javascript
	// 1. 形参初始值 具有默认值的参数，一般位置要靠后
    function add(a, b, c = 10) {
        return a + b + c;
    }
    let result = add(1, 2);
    console.log(result);

    // 2.与解构赋值结合
	// 未结合之前：
    // function connect(options) {
    //     let host = options.host;
    //     let username = options.username;
    //     let password = options.password;
    //     let port = options.port;
    // }
    function connect({host="127.0.0.1", username, password, port}) {
        console.log(host);
        console.log(username);
        console.log(password);
        console.log(port);
    }
    connect({
        host: 'aiguigu.com',
        username: 'root',
        password: 'root',
        port: 3306
    })
```

## 6.8 rest 参数

ES6 引入 rest 参数，用于获取函数的实参，用来代替 arguments 参数。

> 注意：rest 参数非常适合不定个数参数函数的场景

```javascript
<script>
    // ES6 引入 rest参数，用于获取函数的实参，用来代替 arguments
    // ES5 获取实参的方式
    // function date() {
    //     console.log(arguments);
    // }
    // date('柏芝','阿娇','思慧');

    // rest 参数
    function date(...args) {
        console.log(args); // 数组方式可以使用filter some every map等API方法，提高对数据处理的灵活程度
    }
    date('柏芝','阿娇','思慧');

    // rest 参数必须放到参数最后
    function fn(a, b, ...args) {
        console.log(a);
        console.log(b);
        console.log(args);
    }
    fn(1, 2, 3, 4, 5, 6);
</script>
```

## 6.9 spread 扩展运算符

扩展运算符（spread）也是三个点（…），它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列，对数组进行解包。

```javascript
let tfboys = ['易烊千玺','王源','王俊凯'];
function fn() {
	console.log(arguments);
}
fn(...tfboys);
```

**spread扩展运算符的运用**

```javascript
<script>
    // 1.数组的合并
    const kuaizi = ['王太利', '肖央'];
    const fenghuang = ['曾毅', '玲花'];
    // const xiaopingguo = kuaizi.concat(fenghuang);
    const xiaopingguo = [...kuaizi, ...fenghuang];
    console.log(xiaopingguo);

    // 2.数组的克隆    <div id="ad"></div>

    const sanzhihua = ['E', 'G', 'M'];
    const sanyecao = [...sanzhihua];
    console.log(sanyecao);

    // 3.将为数组转为真正的数组
    const divs = document.querySelectorAll('div');
    const divArr = [...divs];
    console.log(divArr);
</script>
```

## 6.10 Symbol类型

### 6.10.1 Symbol的使用

ES6 引入了一种新的原始数据类型 Symbol，表示独一无二的值，它是 JavaScript 语言的第七种数据类型，是一种类似于字符串的数据类型，Symbol 特点如下：

- Symbol 的值是唯一的，用来解决命名冲突的问题
- Symbol 值不能与其它数据进行运算

- Symbol 定义的对象属性不能使用 for…in 循环遍 历 ，但是可以使用 Reflect.ownKeys 来获取对象的所有键名

> 注意：遇到唯一性的场景时要想到 Symbol

```javascript
<script>
    // 创建Symbol
    let s1 = Symbol();
    console.log(s1);
    console.log(typeof s1);

    // 添加标识的Symbol
    let s2 = Symbol('张三');
    let s2_2 = Symbol('张三');
    console.log(s2);
    console.log(s2_2);
    console.log(s2 === s2_2); // false

    // 使用Symbol for 定义
    let s3 = Symbol.for('张三');
    let s3_2 = Symbol.for('张三');
    console.log(s3);
    console.log(s3_2);
    console.log(s3 === s3_2); // true

    // 在方法中使用Symbol
    let game = {
        name: '狼人杀',
        [Symbol('say')]: function() {
            console.log('我可以发言')
        },
        [Symbol('boom')]: function() {
            console.log('我可以自爆')
        }
    };
    console.log(game);
</script>
```

### 6.10.2 Symbol内置值

除了定义自己使用的 Symbol 值以外，ES6 还提供了 11 个内置的 Symbol 值，指向语言内部使用的方法。

可以称这些方法为魔术方法，因为它们会在特定的场景下自动执行。

![image-20220425084437535](C:\Users\W-208 枼\AppData\Roaming\Typora\typora-user-images\image-20220425084437535.png)

**Symbol.hasInstance演示：**

```javascript
class Person {
	static [Symbol.hasInstance](param) {
		console.log("我被用来检测类型了");
	}
}
let o = {};
console.log(o instanceof Person); // false
```

**Symbol.isConcatSpreadable演示:**

```
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
arr2[Symbol.isConcatSpreadable] = true;
console.log(arr1.concat(arr2)); // (6) [1, 2, 3, 4, 5, 6]

const arr3 = [1, 2, 3];
const arr4 = [4, 5, 6];
arr4[Symbol.isConcatSpreadable] = false;
console,log(arr3.concat(arr4)); // (4) [1, 2, 3, Array(3)]
```

## 6.11 迭代器

遍历器（Iterator）就是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。ES6 创造了一种新的遍历命令 for…of 循环，Iterator 接口主要供 for…of 消费，原生具备 iterator 接口的数据：

- Array
- Arguments
- Set
- Map
- String
- TypedArray
- NodeList

> 注意：需要自定义遍历数据的时候，要想到迭代器

工作原理：

1. 创建一个指针对象，指向当前数据结构的起始位置
2. 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员
3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
4. 每调用 next 方法返回一个包含 value 和 done 属性的对象

> 注意: for (let v in arr)循环 此时v保存的是键名, 而for(let v of arr)循环v保存的是键值

**案例演示: 遍历数组**

```javascript
// 声明一个数组
    const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
    // 使用for...of 遍历数组
    for (let v of xiyou) {
        console.log(v);
    }
    console.log("==============");

    // 获取迭代器对象
    let iterator = xiyou[Symbol.iterator]();
    // 调用对象的next方法
    console.log(iterator.next());
    console.log(iterator.next());
    console.log(iterator.next());
    console.log(iterator.next()); // {value: '沙僧', done: false}
    console.log(iterator.next()); // {value: undefined, done: true}
```

**案例演示: 自定义遍历数据**

```javascript
<script>
    const banji = {
        name: '五班',
        stus: [
            '张三',
            '李四',
            '王五',
            '小六'
        ],
        [Symbol.iterator]() {
            // 索引变量
            let index = 0;
            let _this = this; // 也可以改为箭头函数
            return {
                next: function() { // next: () => {
                    if (index < _this.stus.length) {
                        const result = {value: _this.stus[index], done: false};
                        // 下标自增
                        index++;
                        return result;
                    } else {
                        return {value: undefined, done: true};
                    }
                }
            };
        }
    }
    for (let v of banji) {
        console.log(v);
    }
</script>
```

## 6.12 生成器

生成器函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。

### 6.12.1 生成器函数使用

- \* 的位置没有限制
- 生成器函数返回的结果是迭代器对象，调用迭代器对象的 next 方法可以得到 yield 语句后的值
- yield 相当于函数的暂停标记，也可以认为是函数的分隔符，每调用一次 next 方法，执行一段代码
- next 方法可以传递实参，作为 yield 语句的返回值

```javascript
function *gen() {
	console.log(111);
	yield '一只没有耳朵';
	console.log(222);
	yield '一直没有尾巴';
	console.log(333);
	yield '真奇怪'
	console.log(444);
}
let iterator = gen();
iterator.next();
iterator.next();
iterator.next();
iterator.next();
console.log("=========");

for (let v of gen()) {
	console.log(v);
}
```

### 6.12.2 生成器函数参数

```javascript
<script>
    function *gen(arg) {
        console.log(arg);
        let one = yield 111;
        console.log(one);
        let two = yield 222;
        console.log(two);
        let three = yield 333;
        console.log(three);
    }

    // 执行获取迭代器对象
    let iterator = gen('AAA');
    console.log(iterator.next());
    // next方法可以传入实参
	// next传入的参数将作为上一个yield语句的返回结果
    console.log(iterator.next('BBB'));
    console.log(iterator.next('CCC'));
    console.log(iterator.next('DDD'));
</script>
```

### 6.12.3 生成器函数实例

```javascript
<script>
    // 异步编程：文件操作 网络操作（ajax，request） 数据库操作
    // 1s后控制台输出111 然后2s后输出222 然后3s后输出333
    // 回调地狱(多个异步任务会出现代码不断向前推进，阅读和调试都不方便)
    // setTimeout(() => {
    //     console.log(111);
    //     setTimeout(() => {
    //         console.log(222);
    //         setTimeout(() => {
    //             console.log(333);
    //         }, 3000);
    //     }, 2000);
    // }, 1000);

    function one() {
        setTimeout(() => {
            console.log(111);
            // iterator是在全局作用域里声明的,可以在函数中调用
            iterator.next(); 
        }, 1000)
    }

    function two() {
        setTimeout(() => {
            console.log(222);
            iterator.next();
        }, 2000)
    }

    function three() {
        setTimeout(() => {
            console.log(333);
            iterator.next();
        }, 3000)
    }

    function *gen() {
        yield one();
        yield two();
        yield three();
    }

    // 调用生成器函数
    let iterator = gen();
    iterator.next();
</script>
```

### 6.12.4 生成器函数实例2

```javascript
<script>
    // 模拟获取 用户数据 订单数据 商品数据
    function getUsers() {
        setTimeout(() => {
            let data = '用户数据';
            /*
                因为iterator.next传入的参数将作为上一个yield语句的返回结果
                这是第二次调用next，所以他的实参data将作为第一个yield语句的返回结果
            */
            iterator.next(data);
        }, 1000);
    }

    function getOrders() {
        setTimeout(() => {
            let data = '订单数据';
            // 这是第三次调用next，所以他的实参data将作为第二个yield语句的返回结果
            iterator.next(data);
        }, 1000);
    }

    function getGoods() {
        setTimeout(() => {
            let data = '商品数据';
            // 这是第四次调用next，所以他的实参data将作为第三个yield语句的返回结果
            iterator.next(data);
        }, 1000);
    }

    // 调用生成器函数
    function *gen() {
        let user = yield getUsers();
        console.log(user);
        let order = yield getOrders();
        console.log(order);
        let good = yield getGoods();
        console.log(good);
    }
    let iterator = gen();
    // 让生成器函数跑起来，第一次调用next
    iterator.next();
</script>
```

## 6.13 Promise

Promise 是 ES6 引入的异步编程的新解决方案，语法上 Promise 是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

### 6.13.1 Promise基本使用

```javascript
<script>
    // 实例化 Promise 对象
    const p = new Promise(function(resolve, reject) {
        setTimeout(function() {
            // 成功调用resolve()处理
            // let data = '数据库中的用户数据';
            // resolve(data);

            // 失败调用reject()处理
            let err = '数据读取失败';
            reject(err);
        }, 1000);
    });

    // 调用 Promise 对象中的 then 方法
    p.then(function(value){
        console.log(value);
    }, function(reason) {
        console.error(reason);
    });
</script>
```

### 6.13.2 Promise案例演示

**1.Promise封装读取文件:**

```javascript
// 1. 引入node.js中的fs模块(必须使用link形式读取外部js文件,在js文件中引用js,即可成功)
const fs = require('fs');

// 2.调用方法读取文件
// fs.readFile('./note.md', (err, data)=>{
//     // 如果失败，则抛出错误
//     if (err) throw err;
//     // 如果没有出错，则输出内容
//     console.log(data.toString());
// });

// 3. 使用Promise封装
const p = new Promise(function(resolve, reject){
    fs.readFile("./note.md", (err, data)=>{
        // 判断如果失败
        if (err) reject(err);
        // 如果成功
        resolve(data);
    })
});

p.then(function(value){{
    console.log(value.toString());
}}, function(reason){
    console.log("读取失败！！");
});
```

**2.Promise封装AJAX请求:**

```javascript
<script>
    const p = new Promise((resolve, reject) => {
        // 1.创建对象
        const xhr = new XMLHttpRequest();
        // 2.初始化
        xhr.open("GET", "https://api.apiopen.top/getJoke");
        // 3.发送
        xhr.send();
        // 4.绑定事件，处理响应结果
        xhr.onreadystatechange = function () {
            // 判断
            if (xhr.readyState === 4) {
                // 判断响应状态码 200-299
                if (xhr.status >= 200 && xhr.status < 300) {
                    // 表示成功
                    // console.log(xhr.response);
                    resolve(xhr.response);
                } else {
                    // 如果失败
                    // console.error(xhr.status);
                    reject(xhr.status);
                }
            }
        }
    });

    p.then(function(value){
        console.log(value);
    }, function(reason){
        console.log(reason);
    });
</script>
```

### 6.13.3 Promise.prototype...then方法:

调用 then 方法，then 方法的返回结果是 Promise 对象，对象状态由回调函数的执行结果决定，如果回调函数中返回的结果是 非 promise 类型的属性，状态为成功，返回值为对象的成功的值

```javascript
<script>
    // 创建 promise 对象
    const p = new Promise((resolve, reject) => {
        setTimeout(() => {
            // resolve('用户数据');
            reject('出错啦');
        }, 1000)
    });

    // 调用then方法 then 方法的返回结果是 Promise 对象，对象状态由回调函数的执行结果决定
    // 1.如果回调函数中返回的结果是 非promise类型的属性，状态为成功，返回值为对象的成功的值

    const result = p.then(value => {
        console.log(value);
        // 1.非 promise 类型的属性
        // return 'iloveyou';
        // 2.是promise对象
        // return new Promise((resolve, reject)=>{
        //     resolve('ok');
        //     reject('error');
        // });
        // 3.抛出错误
        // throw new Error('出错了');
        throw '出错了';
    }, reason=>{
        console.warn(reason);
    })

    // 链式调用(避免了回调地狱)
    p.then(value=>{

    }).then(value=>{

    });

    console.log(result);
</script>
```

**案例演示:多个文件读取**

```javascript
const fs = require('fs');
const { resolve } = require('path');

// 容易出现回调地狱且变量名重复的问题
// fs.readFile('./note.md', (err, data1) => {
//     fs.readFile('./山居秋暝.md', (err, data2) => {
//         fs.readFile('./月下独酌.md', (err, data3) => {
//             // let result = `${data1}
//             // ${data2}
//             // ${data3}`;
//             let result = data1 + '\n' + data2 + '\n' + data3;
//             console.log(result.toString());
//         });
//     });
// });

// 使用 promise 实现
const p = new Promise((resolve, reject) => {
    fs.readFile("./note.md", (err, data) => {
        resolve(data);
    });
});

p.then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./山居秋暝.md", (err, data) => {
            resolve([value, data]);
        });
    });
}).then(value => {
    return new Promise((resolve, reject) => {
        fs.readFile("./月下独酌.md", (err, data) => {
            // value.push(data);
            resolve([...value, data]);
        });
    }); 
}).then(value => {
    console.log(value.join('\r\n'));
});
```

### 6.13.4 Promise-catch方法

如果只想处理错误状态,我们可以使用catch方法

```javascript
<script>
    const p = new Promise((resolve, reject) => {
        setTimeout(() => {
            // 设置 p 对象的状态为失败，并设置失败的值
            reject("出错啦");
        }, 1000)
    });

    // p.then(function(value){}, function(reason){
    //     console.error(reason);
    // });

    p.catch(function(reason){
        console.warn(reason);
    });
</script>
```

## 6.14 Set

ES6 提供了新的数据结构 Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历，集合的属性和方法：

- size：返回集合的元素个数
- add()：增加一个新元素，返回当前集合
- delete()：删除元素，返回 boolean 值
- has()：检测集合中是否包含某个元素，返回 boolean 值
- clear()：清空集合，返回 undefined

```javascript
<script>
    // 创建一个空集合
    let s = new Set();
    // 创建一个非空集合;
    let s1 = new Set([1, 2, 3, 1, 2, 3]);
    // 返回集合的元素个数
    console.log(s1.size);
    // 添加新元素
    console.log(s1.add(4));
    // 删除元素
    console.log(s1.delete(1));
    // 检测是否存在某个值
    console.log(s1.has(2));
    // 清空集合
    console.log(s1.clear());

    console.log("================");

    let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let arr2 = [4, 5, 6, 5, 6];
    // 1.数组去重
    let result = [...new Set(arr)];
    console.log(result);
    // 2.交集
    let intersection = [...new Set(arr)].filter(item => new Set(arr2).has(item));
    console.log(intersection);
    // 3.并集
    let union = [...new Set([...arr, ...arr2])];
    console.log(union);
    // 4.差集（arr与arr2的差集）
    let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)));
    console.log(diff);
</script>
```

## 6.15 Map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合。但是“键” 的范围不限于字符串，各种类型的值（包括对象）都可以当作键。Map 也实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历。Map 的属性和方法:

- size：返回 Map 的元素个数
- set()：增加一个新元素，返回当前 Map
- get()：返回键名对象的键值
- has()：检测 Map 中是否包含某个元素，返回 boolean 值
- clear()：清空集合，返回 undefined

```javascript
<script>
	// 创建一个空 map
    let mp = new Map();
    // 创建一个非空 map
    let mp2 = new Map([
        ["name", "张三"],
        ["gender", "女"]
    ]);
    // 获取映射元素的个数
    console.log(mp2.size);
    // 添加映射值
    console.log(mp2.set("age", 6));
    // 获取映射值
    console.log(mp2.get("age"));
    // 检测是否有该映射
    console.log(mp2.has("age"));
    // 清除
    console.log(mp2.clear());

    console.log("==============");

	// 声明 Map
    let m = new Map();

    // 添加元素
    m.set('name', '尚硅谷');
    m.set('change', function(){
        console.log('我们可以改变你！');
    });
    let key = {
        school: 'atguigu'
    };
    m.set(key, ['北京', '上海', '深圳']);
    // map遍历
    for (let v of m) {
        console.log(v);
    }
</script>
```

## 6.16 Class

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。基本上，ES6 的 class 可以看作只是 一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已，它的一些如下：

- class：声明类
- constructor：定义构造函数初始化
- extends：继承父类
- super：调用父级构造方法
- static：定义静态方法和属性

**class介绍与初体验:**

```javascript
<script>
    // ES5 用构造函数实例化对象
    // 手机
    function Phone(brand, price) {
        // 对实例对象的属性作初始化
        this.brand = brand;
        this.price = price;
    }
    // 添加方法
    Phone.prototype.call = function() {
        console.log('我可以打电话！');
    }

    // 实例化对象
    let Huawei = new Phone('华为', 5999);
    Huawei.call();
    console.log(Huawei);

    // ES6 class
    class Shouji {
        // 构造方法 名字不能修改
        // new操作时会自动执行
        constructor(brand, price) {
            this.brand = brand;
            this.price = price;
        }
        // 方法必须使用该语法，不能使用ES5对象的完整形式
        call() {
            console.log("我可以打电话！");
        }
    }
    // 实例化对象
    let onePlus = new Shouji("1+", 1999);
    console.log(onePlus);
</script>
```

**class静态成员:**

```javascript
<script>
    // 构造函数本身也是一个对象
    function Phone() {

    }
    /*
        函数对象
        name和change两个属性属于函数对象，不属于实例对象
        对于这样的属性我们称之为静态成员，
        即在class中，这两个属性是属于类的，而不属于实例对象
    */
    // 实例对象和函数对象的属性是不通的
    Phone.name = '手机';
    Phone.change = function() {
        console.log('我可以改变世界');
    }
    // 实例对象的属性和构造函数原型对象是相通的
    Phone.prototype.size = '5.5inch';
    // 实例对象
    let nokia = new Phone();
    console.log(nokia.name);
    // nokia.change();
    console.log(nokia.size);
    
    console.log("==============");

    class Shouji {
        // 静态属性
        // static标注的属性和方法，属于类而不属于实例对象
        static name = '手机';
        static change() {
            console.log("我可以改变世界");
        }
    }
    let nokia2 = new Shouji();
    // 实例对象
    console.log(nokia2.name); // undefined
    // 类
    console.log(Shouji.name); // 手机
</script>
```

**ES5构造函数的继承:**

```javascript
<script>
    // 手机
    function Phone(brand, price) {
        this.brand = brand;
        this.price = price;
    }

    Phone.prototype.call = function() {
        console.log("我可以打电话");
    }

    // 智能手机
    function SmartPhone(brand, price, color, size) {
        Phone.call(this, brand, price);
        this.color = color;
        this.size = size;
    }

    // 设置子级构造函数的原型
    SmartPhone.prototype = new Phone;
    // 校正
    SmartPhone.prototype.constructor = SmartPhone;

    // 声明子类的方法
    SmartPhone.prototype.photo = function() {
        console.log("我可以拍照");
    }
    SmartPhone.prototype.playGame = function() {
        console.log("我可以玩游戏");
    }

    const chuizi = new SmartPhone('锤子', 2499, '黑色', '5.5inch');
    console.log(chuizi);
</script>
```

**ES6class的继承:**

```javascript
<script>
    // 父类
    class Phone {
        // 构造方法
        constructor(brand, price) {
            this.brand = brand;
            this.price = price;
        }
        // 父类的成员属性
        call() {
            console.log("我可以打电话");
        }
    }
    // 子类
    class SmartPhone extends Phone{
        // 构造方法
        constructor(brand, price, color, size) {
            // 父类的constructor方法 
            // 等价于Phone.call(this, brand, price);
            super(brand, price);
            this.color = color;
            this.size = size;
        } 
        photo() {
            console.log("拍照");
        }
        playGame() {
            console.log("玩游戏");
        }
        call() {
            // 子类不能直接调用父类的同名方法,也不能用super实现
            console.log("我可以进行视频通话");
        }
    }
    const xiaomi = new SmartPhone('小米', 7999, '黑色', '5.5inch');
    console.log(xiaomi);
    xiaomi.call();
    xiaomi.photo();
    xiaomi.playGame();
</script>
```

**class中getter和setter设置:**

```javascript
class Phone {
	get price() {
		console.log("价格属性被读取了");
		return "iloveyou";
	}
	set price() { // 可以对price属性赋值时进行判断是否合法
		console.log("价格属性被修改了");
	}
	// 实例化对象
	let s = new Phone();
	console.log(s.price);
	s.price = 'free';
}
```

## 6.17 数值扩展

```javascript
<script>
    // 0.Number.EPSILON是JavaScript表示的最小精度
    // EPSILON属性的值接近于 2.2204460492503130808472633361816E-16
    function equal(a, b) {
        if (Math.abs(a - b) < Number.EPSILON) {
            return true;
        } else {
            return false;
        }
    }
    console.log(0.1 + 0.2 === 0.3);
    console.log(equal(0.1 + 0.2, 0.3));
    // 1.二进制和八进制
    let b = 0b1010; // 二进制
    let o = 0o777;  // 八进制
    let d = 100;
    let x = 0xff;   // 十六进制
    console.log(x);
    // 2.Number.isFinite 检测一个数值是否为有限数
    console.log(Number.isFinite(100));
    console.log(Number.isFinite(100 / 0));
    console.log(Number.isFinite(Infinity));
    console.log("=============");
    // 3.Number.isNaN 检测一个数值是否为NaN
    console.log(Number.isNaN(123));
    // 4.Number.parseInt Number.parseFloat 字符串转整数
    console.log(Number.parseInt('5211314love'));
    console.log(Number.parseFloat('3.1415926生i去'));
    // 5.Number.isInteger 判断一个数是否为整数
    console.log(Number.isInteger(5));
    console.log(Number.isInteger(2.5));
    // 6.Math.trunc 将数字的小数部分抹掉
    console.log(Math.trunc(3.5));
    // 7.Math.sign 判断一个数为正数 负数 还是零
    console.log(Math.sign(100));
    console.log(Math.sign(0));
    console.log(Math.sign(-2000));
</script>
```

## 6.18 对象方法扩展

```javascript
<script>
    // 1.Object.is 判断两个值是否完全相等
    console.log(Object.is(120, 120)); // true
    console.log(Object.is(NaN,NaN)); // true
    console.log(NaN === NaN); // false
    // 2.Object.assign 对象的合并
    const config1 = {
        host: 'localhost',
        port: 3306,
        name: 'root',
        pass: 'root',
        test: 'test'
    };

    const config2 = {
        host: 'http://aiguigu.com',
        port: 33060,
        name: 'aiguigu.com',
        pass: 'iloveyou',
        test2: 'test2'
    };
    console.log(Object.assign(config1, config2));
    // 3.Object.setPrototypeOf 设置原型对象 
	//   Object.getPrototypeof获取原型对象
    const school = {
        name: '尚硅谷'
    }
    const cities = {
        xiaoqu: ['北京', '上海', '深圳']
    }
    // 虽然能改变原型对象，但不推荐这样去做，
    // 一般在创建对象时，就把原型对象设置上，这样效率最高
    Object.setPrototypeOf(school, cities);
    console.log(school);
    console.log(Object.getPrototypeOf(school));
</script>
```

## 6.19 模块化

模块化是指将一个大的程序文件，拆分成许多小的文件，然后将小文件组合起来。

### 6.19.1 模块化的好处

- 防止命名冲突
- 代码复用
- 高维护性

### 6.19.2 模块化的产品

ES6之前的模块化规范有：

CommonJS => NodeJS、Browserify

AMD => requireJS

CMD => seaJS

### 6.19.3 模块化的语法

模块功能主要由两个命令构成：export 和 import。

- export 命令用于规定模块的对外接口
- import 命令用于输入其它模块提供的功能

**模块化的暴露：**

```javascript
// 分别暴露
export let school = '尚硅谷';
export function teach() {
    console.log('我们可以教给你开发技能');
}
```

```javascript
// 统一暴露
let school = '尚硅谷';
function findJob() {
    console.log("我们可以帮助你找到工作");
}
export {school, findJob};
```

```javascript
// 默认暴露
export default {
    school: 'ATGUIGU',
    change: function() {
        console.log("我们可以改变你");
    }
}
```

**模块化的导入：**

1.直接使用标签`<srcipt type="module">...</srcipt>`

```javascript
<script type="module">
    // 1.通用导入方式
    // 引入m1.js模块内容
    // import * as m1 from "./m1.js";
    // console.log(m1);
    // 引入m2.js模块内容
    // import * as m2 from "./m2.js";
    // console.log(m2);
    // 引入m3.js模块内容
    // import * as m3 from "./m3.js";
    // console.log(m3);
    // 调用时必须加上default
    // m3.default.change();

    // 2.解构赋值形式
    import {school, teach} from "./m1.js";
    console.log(school, teach);
    // 重名时要取别名
    import {school as guigu, findJob} from "./m2.js";
    console.log(guigu, findJob);
    // 固定写法，必须取别名
    import {default as m3} from "./m3.js";
    console.log(m3);
    
    // 3.简便形式，只能针对默认暴露
    // import m3 from "./m3.js";
    // console.log(m3);
</script>
```

2.用script标签中src引入一个入口文件，并且type属性为module，`<script src="./app.js" type="module"></script>`

```javascript
// 入口文件 app.js
// 模块引入
import * as m1 from "./m1.js";
import * as m2 from "./m2.js";
import * as m3 from "./m3.js";
console.log(m1);
console.log(m2);
console.log(m3);
```

***了解内容：babel对ES6模块化代码转换、ES6模块化引入NPM包***

## 6.20 浅拷贝和深拷贝

浅拷贝的时候如果数据是基本数据类型，那么就如同直接赋值那种，会拷贝其本身，如果除了基本数据类型之外还有一层对象，那么对于浅拷贝而言就只能拷贝其引用，对象的改变会反应到拷贝对象上；但是深拷贝就会拷贝多层，即使是嵌套了对象，也会都拷贝出来。

# 第七章 ECMAScript7的新特性

## 7.1 数组方法扩展

Array.prototype.includes：此方法用来检测数组中是否包含某个元素，返回布尔类型值

```javascript
const mingzhu = ["西游记", "红楼梦", "三国演义", "水浒传"];
console.log(mingzhu.includes("西游记")); // true
```

之前使用的是mingzhu.indexOf：如果存在则返回数组下标，如果不存在返回结果是-1。

## 7.2 幂运算

`**` 操作符的作用和 `Math.pow` 的作用一样：

```javascript
console.log(2 ** 10);
console.log(Math.pow(2, 10));
```

# 第八章 ECMAScript8的新特性

## 8.1 async函数

**async 和 await 两种语法结合可以让异步代码像同步代码一样**

async 函数的返回值：

1. 返回的结果不是一个 Promise 类型的对象，返回的结果就是成功 Promise 对象
2. 返回的结果如果是一个 Promise 对象，具体需要看执行resolve方法还是reject方法
3. 抛出错误，返回的结果是一个失败的 Promise

```javascript
// async 函数
async function fn() {
	return new Promise((resolve, reject) => {
		resolve("成功的数据");
		reject("失败的数据");
	});
}
const result = fn();
// 调用then方法
result.then(value => {
	console.log(value);
}, reason => {
	console.warn(reason);
})
```

## 8.2 await 表达式

await 表达式的注意事项：

1. await 必须写在 async 函数中，async函数中不一定有await
2. await 右侧的表达式一般为 promise 对象
3. await 返回的是 promise 成功的值
4. await 的 promise 失败了, 就会抛出异常, 需要通过 try…catch 捕获处理

```javascript
<script>
    // 创建 promise 对象
    const p = new Promise((resolve, reject) => {
        resolve("用户数据");
        // reject("失败了");
    });

    // await要放在async函数中
    async function fn() {
        try {
            let result = await p;
            console.log(result);
        } catch (e) {
            console.log(e);
        } 
    }
    // 调用函数
    fn();
</script>
```

**案例1：async和await结合读取文件内容**

```javascript
// 1.引入 fs 模块
const fs = require("fs");

// 读取note
function readNote() {
    return new Promise((resolve, reject) => {
        fs.readFile("./note.md", (err, data) => {
            // 如果成功
            resolve(data);
            // 如果失败
            reject(err);
        });
    })
}

// 读取山居秋暝
function readShanju() {
    return new Promise((resolve, reject) => {
        fs.readFile("./山居秋暝.md", (err, data) => {
            // 如果成功
            resolve(data);
            // 如果失败
            reject(err);
        });
    })
}

// 读取月下独酌
function readYuexia() {
    return new Promise((resolve, reject) => {
        fs.readFile("./月下独酌.md", (err, data) => {
            // 如果成功
            resolve(data);
            // 如果失败
            reject(err);
        });
    })
}

// 声明一个async函数
async function main() {
    // 获取note内容
    let note = await readNote();
    // 获取山居秋暝内容
    let shanju = await readShanju();
    // 获取月下独酌内容
    let yuexia = await readYuexia();
    console.log(note.toString());
    console.log(shanju.toString());
    console.log(yuexia.toString());
}

main();
```

**案例2：async和await封装AJAX请求**

```javascript
<script>
    // 发送AJAX请求，返回的结果是 Promise 对象
    function sendAJAX(url) {
        return new Promise((resolve, reject) => {
            // 1.创建对象
            const x = new XMLHttpRequest();
            // 2.初始化
            x.open('GET', url);
            // 3.发送
            x.send();
            // 4.事件绑定
            x.onreadystatechange = function() {
                if (x.readyState === 4) {
                    if (x.status >= 200 && x.status < 300) {
                        // 成功
                        resolve(x.response);
                    } else {
                        // 失败
                        reject(x.status);
                    }
                }
            }
        });
    }

    // promise then 方法测试
    // sendAJAX("https://api.apiopen.top/getJoke").then(value=>{
    //     console.log(value);
    // }, reason=>{});
    
    // async 与 await 测试
    async function main() {
        // 发送AJAX请求1
        let result = await sendAJAX("https://api.apiopen.top/getJoke");
        // 发送AJAX请求2
        let tianqi = await sendAJAX('https://www.tianqiapi.com/api/?version=v1&city=%E5%8C%97%E4%BA%AC&appid=23941491&appsecret=TXoD5e8P');
        console.log(result);
        console.error(tianqi);//为了区别数据，我这里用红色的error输出
    }

    main();
</script>
```

## 8.3 对象方法拓展

- Object.keys()方法：返回一个给定对象的所有可枚举键值的数组。
- Object.values()方法：返回一个给定对象的所有可枚举属性值的数组。
- Object.entries()方法：返回一个给定对象自身可遍历属性 [key,value] 的数组。

```javascript
//声明对象
const person = {
    name: "张三",
    age: 20
};

//获取对象所有的键
console.log(Object.keys(person));
//获取对象所有的值
console.log(Object.values(person));
//获取对象所有的键值对数组
console.log(Object.entries(person));
//创建 Map
const m = new Map(Object.entries(person));
console.log(m.get("name"));
```

- Object.getOwnPropertyDescriptors()方法返回：指定对象所有自身属性的描述对象。

```javascript
//声明对象
const person = {
    name: "张三",
    age: 20
};
//对象属性的描述对象
console.log(Object.getOwnPropertyDescriptors(person));

//声明对象
const obj = Object.create(null, {
    name: {
        //设置值
        value: "李四",
        //属性特性
        writable: true,
        configurable: true,
        enumerable: true
    },
    age: {
        //设置值
        value: 21,
        //属性特性
        writable: true,
        configurable: true,
        enumerable: true
    }
});
//对象属性的描述对象
console.log(Object.getOwnPropertyDescriptors(obj));
```

# 第九章 ECMAScript9的新特性

## 9.1 对象拓展

Rest 参数与 spread 扩展运算符在 ES6 中已经引入，不过 ES6 中只针对于数组，在 ES9 中为对象提供了像数组一样的 rest 参数和扩展运算符

**对象展开：**

```javascript
function connect({host, port, ...user}) {
	console.log(host);
	console.log(port);
	console.log(user);
}
connect({
	host: '127.0.0.1',
	port: 3306,
	username: 'root',
	password: 'root',
	type: 'master'
});
```

**对象合并：**

```javascript
const skillOne = {
	q: '天音波'
};
const skillTwo = {
	w: '金钟罩'
};
const skillThree = {
	e: '天雷破'
};
const skillFour = {
	r: '猛龙摆尾'
};
const mangseng = {...skillOne, ...skillTwo, ...skillThree, ...skillFour};
console.log(mangseng);
```

## 9.2 正则表达式拓展

### 9.2.1 命名捕获分组

ES9 允许命名捕获组使用符号 `?<name>` ，这样获取捕获结果可读性更强。为何不适用数组下标吗？因为如果一旦你想要获取的元素一旦增加，数组下标就改变了，所以建议使用命名捕获分组。

```javascript
let str = '<a href="http://www.aiguigu.com">尚硅谷</a>';
// 之前
// const reg = /<a href="(.*)">(.*)<\/a>/;
// console.log(result);
// console.log(result[1]);
// console.log(result[2]);
const reg = /<a href="(?<url>.*)">(?<text>.*)<\/a>/;
const result = reg.exec(str);
console.log(result);
console.log(result.groups.url);
console.log(result.groups.text);
```

### 9.2.2 正向断言和反向断言

```javascript
<script>
    // 声明字符串
    let str = 'JS5211314你知道吗555啦啦啦';

    // 正向断言: 通过对匹配结果后面的内容进行判断，对匹配进行筛选
    // const reg = /\d+(?=啦)/;
    // const result = reg.exec(str);

    // 反向断言: 通过对匹配结果前面的内容进行判断，对匹配进行筛选
    const reg = /(?<=吗)\d+/;
    const result = reg.exec(str);

    console.log(result);
</script>
```

### 9.2.3 dotAll模式

```javascript
let str = `
<ul>
     <li>
         <a>肖生克的救赎</a>
         <p>上映日期: 1994-09-10</p>
         </li>
     <li>
         <a>阿甘正传</a>
         <p>上映日期: 1994-07-06</p>
     </li>
</ul>`;

//声明正则
const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/gs;

// 执行匹配
let result;
let data = [];
while (result = reg.exec(str)) {
    data.push({title: result[1], time: result[2]});
}

//输出结果
console.log(data);
```

# 第十章 ECMAScript10的新特性

## 10.1 对象方法拓展

`Object.fromEntries()`方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象。

```javascript
//ES6：Map
//ES10：Object.fromEntries
const m = new Map();
m.set("name", "张三");
m.set("age", 20);
const result = Object.fromEntries(m);
console.log(result);

//ES8：Object.entries
const arr = Object.entries(result);
console.log(arr);
```

## 10.2 字符串方法拓展

```javascript
let str = "   iloveyou   ";
console.log(str.trimStart());//只去除前边的空格
console.log(str.trimEnd());//只去除后边的空格
```

## 10.3 数组方法拓展

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回，说白了就是将多维数组转化为低维数组。

```javascript
const arr1 = [1, 2, 3, 4, [5, 6]];
console.log(arr1.flat());
const arr2 = [1, 2, 3, 4, [5, 6, [7, 8, 9]]];
console.log(arr2.flat());
console.log(arr2.flat(1));//参数为深度是一个数字
console.log(arr2.flat(2));//参数为深度是一个数字
```

flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 连着深度值为1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。

```javascript
var arr1 = [1, 2, 3, 4];
console.log(arr1.map(item => item * 2));

var arr2 = [1, 2, 3, 4];
console.log(arr2.flatMap(item => item * 2));
```

## 10.4 Symbol属性拓展

Symbol.prototype.description用来读取Symbol的描述值。

```javascript
// 创建Symbol
let s = Symbol('张三');
console.log(s.description);
```

# 第十一章 ECMAScript11新特性

## 11.1 class私有属性

私有属性只能在class中访问

```javascript
<script>
    class Person {
        // 公有属性
        name;
        // 私有属性
        #age;
        #weight;
        // 构造方法
        constructor(name, age, weight) {
            this.name = name;
            this.#age = age;
            this.#weight = weight;
        }
        // 普通方法
        intro() {
            console.log(this.name);
            console.log(this.#age);
            console.log(this.#weight);
        }
    }
    // 实例化(在类外部不能直接访问私有属性)
    const girl = new Person("戚薇", '23', '45kg');
    girl.intro();
</script>
```

## 11.2 Promise.allSettled

该Promise.allSettled()方法返回一个在所有给定的promise已经fulfilled或rejected后的promise，并带有一个对象数组，每个对象表示对应的promise结果。当有多个彼此不依赖的异步任务成功完成时，或者想知道每个promise的结果时，通常使用它。

相比之下，Promise.all()更适合彼此互相依赖或者在其中任何一个reject时立即结束。

```javascript
// 声明两个promise对象
const p1 = new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve("商品数据 - 1");
	}, 1000)
});
const p2 = new Promise((resolve, reject) => {
	setTimeout(() => {
		// resolve("商品数据 - 2");
		reject("出错啦");
	}, 1000);
});
// 调用 allsettled 方法
const result1 = Promise.allSettled([p1, p2]);
console.log(result1);
// 调用 all 方法
const result2 = Promise.all([p1, p2]);
console.log(result2);
```

## 11.3 字符串方法扩展

`String.prototype.matchAll()` 方法返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器。

```javascript
let str =
`<ul>
    <li>
        <a>肖生克的救赎</a>
        <p>上映日期: 1994-09-10</p>
    </li>
    <li>
        <a>阿甘正传</a>
        <p>上映日期: 1994-07-06</p>
    </li>
</ul>`;

//声明正则
const reg = /<li>.*?<a>(.*?)<\/a>.*?<p>(.*?)<\/p>/sg;

//调用方法
const result = str.matchAll(reg);
// 使用for...of遍历
// for (let v of result) {
//     console.log(v);
// }
// 用扩展运算符对result进行展开
const arr = [...result];
console.log(arr);
```

## 11.4 可选链操作符

当我们要使用传进来的一个属性值的时候，我们不知道这个属性值到底有没有传，我们可以使用&&运算符一级一级判断，就像`const dbHost = config && config.db && config.db.host;`当时这样就显得很麻烦，所以在ES11中就提供了可选链操作符，简化代码，变成`const dbHost = config?.db?.host;`另一方面，即使用户没有传入这个属性，也不会报错，会返回undefined。

```javascript
function connect(config) {
	// const dbHost = config && config.db && config.db.host;
	const dbHost = config?.db?.host;
	console.log(dbHost);
}
connect ({
	db: {
		host: '192.168.1.100',
		username: 'root'
	},
	cache: {
		host: '192.168.1.200',
		username: 'admin'
	}
})
```

## 11.5 动态import

以前我们import导入模块在一开始就全部导入，这样在模块较多时，会显得网页速度加载很慢，在ES11中就提供了一种动态import。

**m1.js**

```javascript
// 分别暴露
export let school = "华北理工大学";
export function study() {
	console.log("我们要学习");
}
```

**index.html**

````javascript
<body>
<button id="btn">点击</button>
</body>
<script type="module">
    const btn = document.getElementById('btn');
	btn.onclick = function() {
		import("./m1.js").then(module => {
      		module.study();      
        });
    };
</script>
````

## 11.6 Bight类型

`BigInt`数据类型的目的是比`Number`数据类型支持的范围更大的整数值。在对大整数执行数学运算时，以任意精度表示整数的能力尤为重要。使用`BigInt`，整数溢出将不再是问题。

此外，可以安全地使用更加准确时间戳，大整数ID等，而无需使用变通方法。 它就是JS 第二个数字数据类型，也将是 JS 第8种基本数据类型：

- Boolean
- Null
- Undefined
- Number
- BigInt
- String
- Symbol
- Object

U are S NB

```javascript
let max = Number.MAX_SAFE_INTEGER;

console.log(max);
console.log(max + 1);
console.log(max + 2);

console.log(BigInt(max));
console.log(BigInt(max) + BigInt(1));
console.log(BigInt(max) + BigInt(2));
```

## 11.7 globalThis

全局属性 `globalThis` 包含全局的 `this` 值，类似于全局对象（global object），无论是浏览器，还是NodeJs都支持。如果你想操作全局变量，就可以忽略环境直接使用`globalThis`，它始终是执行全局对象的。

```javascript
console.log(globalThis);
```

