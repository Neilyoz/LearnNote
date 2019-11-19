```javascript
"use strict";

// 如何实现一个call
Function.prototype.myCall = function(context) {
  // 检测调用者是否是函数
  if (typeof this !== "function") {
    throw new TypeError("myCall 绑定的不是一个函数");
  }

  // 确定当前函数的上下文环境
  let context = context || window;

  // 把当前函数绑定到上下文对应的fn上
  context.fn = this;

  // 排除 this，获取对应的参数
  let args = [...arguments].slice(1);

  // 执行函数返回结果
  let result = context.fn(...args);

  // 删除绑定在上下文上的函数，防止污染
  delete context.fn;

  return result;
};
```
