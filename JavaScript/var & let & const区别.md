# var & let & const之间的区别

- 版本：`var` 在 `ECMAScript` 的所有版本都可使用，但 `let` 和 `const` 只能在 ES6 及更晚的版本中使用。

* `var` 声明的范围是函数作用域，而 `let` 和 `const` 声明的范围是块级作用域。
* `var` 存在声明提前，即被 `var` 声明的变量都会被提升到函数作用域的顶部。`let` 和 `const` 声明的变量不存在声明提前，且具有暂时性死区特点，即在let声明之前的变量执行瞬间被称为暂时性死区，在此阶段引用任何后面才声明的变量都会报错。
* `var` 允许在同一个作用域种重复声明同一个变量，`let` 和 `const` 不允许。
* 在全局作用域种使用 `var` 声明的变量会成为 `window` 对象的属性，`let` 和 `const` 声明的变量则不会。
* `const` 的行为与 `let` 基本相同，区别仅是，使用 `const` 声明变量必须初始化，且不能被修改。



## var

**定义变量**

```js
// 定义单个变量
var message = "varmilo";
message = 208; // 合法，但不推荐
console.log(message); // 208

// 定义多个变量
var message, name = 'W208YE', number = '208';
```



**作用域**

在函数中定义的变量为其局部变量，将在函数退出时被销毁

```js
function demo() {
  var message = 'varmilo'; // 局部变量
}
demo();
console.log(message); // error: message is not defined;
```

但在函数内部定义变量时省略 `var`，可创建全局变量。

虽然可以这样操作，但不推荐。在严格模式下，如果像这样给未声明的变量赋值，会报错。

```js
function demo() {
  message = 'varmilo'; // 全局变量
}
demo();
console.log(message); // varmilo
```

在同一个作用域内反复多次使用 `var` 声明同一个变量也不会报错。

```js
function demo() {
  var message = '111';
  var message = '222';
  var message = '333'; // 在严格模式下不报错
  console.log(message);
}
demo(); // 333
```



**变量提升**

所谓变量提升，就是把所有变量声明都拉到函数作用域的顶部。**注意，只是提升声明，不提升赋值操作。**

```js
function demo() {
  console.log(message);
  var message = 'varmilo';
}
demo(); // undefined
```

等同于：

```js
function demo() {
  var message;
  console.log(message);
  message = 'varmilo';
}
demo(); // undefined
```



## for 循环中的 var, let 声明

由于 `var` 声明的变量没有块级作用域，所以迭代变量 `i` 会渗透到循环体外部，当 `i` 递增到5时退出循环，打印出结果为5。

```js
for (var i = 0; i < 5; ++i) {
    ...
}
console.log(i); // 5
```

使用 `let` 声明迭代变量，就可以把迭代变量的作用域限制在 `for` 循环内部。

```js
for (let i = 0; i < 5; ++i) {
    ...
}
console.log(i); // error: i is not defined
```



**经典面试题**

```js
for (var i = 1; i <= 5; ++i) {
  setTimeout(() => {
    console.log(i);
  }, i * 1000);
}
// Output: 6 6 6 6 6
```

等价于：

```js
var i = 1;
setTimeout(() => {
  console.log(i);
}, i * 1000);

var i = 2;
setTimeout(() => {
  console.log(i);
}, i * 1000);

var i = 3;
setTimeout(() => {
  console.log(i);
}, i * 1000);

var i = 4;
setTimeout(() => {
  console.log(i);
}, i * 1000);

var i = 5;
setTimeout(() => {
  console.log(i);
}, i * 1000);

var i = 6; // i <= 5 结束循环
/*
  Output
  6 1s
  6 2s
  6 3s
  6 4s
  6 5s
*/
```

仔细观察后发现，输出答案时并非一次性等 5s，然后输出5个6，而是依次间隔1s后输出6。

说明，在 `setTimeout` 进入异步宏任务队列之前，`i * 1000` 会对其上方的 `i` 进行解析，所以 `setTimeout` 时间间隔队列为：`1s 2s 3s 4s 5s`。所以在同步代码 `i++` 完毕后，再进行 `console.log(i)` 进行输出，而此时输出的 `i` 就都等于6了。



而在使用`let`声明迭代变量时，JS 引擎在后台会为每个迭代循环声明一个新的迭代变量，每个 setTimeout 引用的都是不同的变量实例，所以 console.log 输出的是我们期望的值，也就是循环执行过程中每个迭代变量的值：

```js
for (let i = 0; i < 5; ++i) {
  setTimeout(() => {
    console.log(i);
  }, 0);
}
// Output: 0 1 2 3 4
```



## 如何用 var 输出1、2、3、4、5

**方法一：立即执行函数 IIFE**

在 `for` 循环中，通过给 `IIFE` 传参，实现输出 `1、2、3、4、5`

```js
for (var i = 1; i <= 5; ++i) {
  (function(i) {
    setTimeout(function timer() {
      console.log(i);
    }, i * 1000);
  })(i);
}
```

**方法二：catch**

在 `for` 循环中，利用 `try/catch` 强制抛出一个错误 `i`，然后在 `catch()` 子句中接收它

```js
for (var i = 1; i <= 5; ++i) {
  try {
    throw i;
  } catch (i) {
    console.log(i);
  }
}
```



## 代码风格建议

优先使用 `const`，`let`次之，`var` 已经被时代所抛弃，不建议再使用（但面试官还是爱问）。使用 `const` 声明可以让浏览器运行时强制保持变量不变，也可以让静态代码分析工具提前发现不合法的赋值操作。只在提前知道未来会有修改时，才使用 `let`。这样可以让开发者更有信息地推断某些变量的值永远不会变，同时也能迅速发现因意外赋值导致的非预期行为。
