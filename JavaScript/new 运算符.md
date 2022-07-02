# new 运算符

## 构造函数为普通函数

对象实例创建过程：

1. 在内存中创建一个新对象。
2. 这个新对象内部的 `[[Prototype]]` 特性被赋值为构造函数的 `prototype` 属性。
3. 构造函数内部的 `this` 被赋值为这个新对象（即 `this` 指向新对象）。
4. 执行构造函数内部的代码（给新对象添加属性）。
5. 如果构造函数返回对象，则返回改对象；否则，返回刚创建的新对象（空对象）。

摘自 `MDN` 的解释：

当代码 `new Foo(...)` 执行时，会发生以下事情：

1. 一个继承自 `Foo.prototype` 的新对象被创建。
2. 使用指定的参数调用构造函数 `Foo`，并将 `this` 绑定到新创建的对象。`new Foo` 等同于 `new Foo()`，也就是没有指定参数列表，`Foo` 不带任何参数调用的情况。
3. 由构造函数返回的对象就是 `new` 表达式的结果。如果构造函数没有显示返回一个对象，则使用步骤 1 创建的对象。（**一般情况下，构造函数不返回值，但用户可以选择主动返回对象，来覆盖正常对象的创建步骤**）



## 构造函数为箭头函数

普通函数创建时，引擎会按照特定的规则为这个函数创建一个`prototype`属性（指向原型对象）。默认情况下，所有原型对象自动获得一个名为 `constructor` 的属性，指回与之关联的构造函数。

```js
function Person() {
  this.age = 18;
}
Person.prototype;
/*
{
  constructor: f Foo()
  __proto__: Object
}
*/
```

**创建箭头函数时，引擎不会为其创建`prototype`属性，箭头函数没有`constructor`供`new`调用，因此使用`new`调用箭头函数会报错！**

```js
const Person = () => {};
new Person(); // Uncaught TypeError: Person is not a constructor
```



## 手写 new

实现 `new` 的关键问题：

1. 让实例可以访问到私有属性
2. 让实例可以访问构造函数原型（`constructor.prototype`）所在原型链上的属性
3. 构造函数返回的最后结果是引用数据类型

```js
function _new(constructor, ...args) {
  // 构造函数类型合法判断
  if (typeof constructor !== 'function') {
    throw new Error('constructor must be a function');
  }
  // 新建空对象实例，并将构造函数的原型绑定到新创建的对象实例上
  let obj = Object.create(constructor.prototype);
  // 调用构造函数并判断返回值
  let res = constructor.apply(obj, args);
  let isObject = typeof res === 'object' && res !== null;
  let isFunction = typeof res === 'function';
  // 如果有返回值且返回值是对象类型，那么就将它作为返回值，否则就返回之前新建的对象
  return isObject || isFunction ? res : obj;
}
```

这个低配版的 `new` 可以用来创建自定义类的实例，但不支持内置对象，毕竟 `new` 属于操作符，底层实现更加复杂。