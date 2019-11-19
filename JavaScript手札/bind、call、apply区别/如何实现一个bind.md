# 如何实现一个 bind

为了实现一个 Function.prototype.bind() 我们需要了解一下 bind 有什么特性。

- bind 是函数的方法，只有函数可以调用
- bind 的第一个参数是`this`指向，剩下的参数作为调用者的参数
- bind 方法返回一个新函数，需要再次调用才会执行

```javascript
"use strict";

// 如何自己实现一个bind方法
Function.prototype.myBind = function(oThis) {
  // 判断调用者是否为函数
  if (typeof this !== "function") {
    throw new TypeError("mybind is try to be bound is not callable.");
  }

  // 保存this(this指向调用bind的函数)
  let fToBind = this;

  // 获取传入mybind函数的第二个及其后面的参数（除去this参数）
  let aArgs = Array.prototype.slice.call(arguments, 1);

  let fBound = function() {
    //this instanceof fBound
    // === true时, 说明返回的fBound被当做new的构造函数调用
    // === false时, 说明当做了普通函数来调用，this 为 myBind 的第一个参数
    return fToBind.apply(
      this instanceof fBound ? this : oThis,
      aArgs.concat(Array.prototype.slice.call(arguments))
    );
  };

  // 返回函数，对应函数的原型对象也需要被创建并重新赋值
  // 这里我们使用Object.create(proto[, propertiesObject]) 来创建新原型对象
  fBound.prototype = Object.create(this.prototype);

  // 返回新函数
  return fBound;
};
```
