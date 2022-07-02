# instanceof 操作符的实现原理

`instanceof` 运算符用于判断构造函数的 `prototype` 属性是否出现在对象的原型链中的任何位置。

```js
function myInstanceof(left, right) {
  left = left.__proto__;
  right = right.prototype;
  while(true) {
    if (left === null || right === undefined) return false;
    if (left === right) return true;  
    left = left.__proto__;
  }
}

console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
console.log(BigInt(1) instanceof BigInt);            // false
console.log(Symbol(1) instanceof Symbol);            // false

console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true

console.log('--------------------------------------');
console.log(null instanceof Object);                 // false
console.log(undefined instanceof Object);            // false
// Uncaught TypeError: Cannot read properties of null (reading '__proto__')
// console.log(myInstanceof(null, Object)); 
// Uncaught TypeError: Cannot read properties of undefined (reading '__proto__')
// console.log(myInstanceof(undefined, Object));
console.log('--------------------------------------');

console.log(myInstanceof(2, Number));                // true
console.log(myInstanceof(true, Boolean));            // true
console.log(myInstanceof('str', String));            // true
console.log(myInstanceof(BigInt(1), BigInt));        // true
console.log(myInstanceof(Symbol(1), Symbol));        // true

console.log(myInstanceof([], Array));                // true
console.log(myInstanceof(function(){}, Function));   // true
console.log(myInstanceof({}, Object));               // true
```



