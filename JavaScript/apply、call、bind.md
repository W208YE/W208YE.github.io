# apply()、call()、bind()

每个 `Function` 对象都存在 `apply()`、`call()`、`bind()`方法，其作用都是可以在特定的作用域中调用函数，即设置函数体内 `this` 对象的值，以扩充函数赖以运行的作用域。



# 使用

`apply(), call(), bind()` 都能改变函数对象的 `this` 指向。

```js
window.name = 'window';
document.name = 'document';
var s = { // 自定义对象s
  name: 'S'
};

var rollCall = {
  name: 'Teacher',
  sayName() {
    console.log(this.name);
  }
};

rollCall.sayName(); // Teacher

// apply
rollCall.sayName.apply(); // window, 不传参默认绑定 window
rollCall.sayName.apply(window); // window, 绑定 window 对象
rollCall.sayName.apply(document); // document, 绑定 document 对象
rollCall.sayName.apply(s); // S, 绑定自定义对象

// call
rollCall.sayName.call(); // window, 不传参默认绑定 window
rollCall.sayName.call(window); // window, 绑定 window 对象
rollCall.sayName.call(document); // document, 绑定 document 对象
rollCall.sayName.call(s); // S, 绑定自定义对象

// bind, 最后一个()为函数执行
rollCall.sayName.bind()(); // window, 不传参默认绑定 window
rollCall.sayName.bind(window)(); // window, 绑定 window 对象
rollCall.sayName.bind(document)(); // document, 绑定 document 对象
rollCall.sayName.bind(s)(); // S, 绑定自定义对象
```



## 区别

虽然 `apply()、call()、bind()` 都能够达到改变 `this` 指针的目的，但是其使用还是又区别的。

```js
// apply 与 call 传参方式不同，bind 和 call 传参方式相同

window.name = 'Teacher';
var rollCall = {
  sayAllName(...args) {
    console.log(this.name);
    args.forEach((v) => console.log(v));
  }
};  

// apply 将参数作为一个数组传递
rollCall.sayAllName.apply(window, ['a', 'b', 'c']);
// call 将参数直接传递，使用逗号分割
rollCall.sayAllName.call(window, 'a', 'b', 'c');
// bind 仅仅将对象绑定，并不立即执行，其返回值是一个函数
rollCall.sayAllName.bind(window, 'a', 'b', 'c')();
```

