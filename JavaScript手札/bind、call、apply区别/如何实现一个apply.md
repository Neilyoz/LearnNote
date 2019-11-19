```javascript
"use strict";

// 如何实现一个apply
Function.prototype.myApply = function(context) {
  // 检测调用者是否为函数
  if (typeof this !== "function") {
    throw new TypeError("myApply 绑定的不是一个函数");
  }

  // 确定调用的上下文环境
  let context = context || window;

  // 把当前函数绑定到上下文对应的fn上
  context.fn = this;

  // 声明调用结果
  let result;

  /**
   * 检测是否传入参数
   *    存在：把调用参数解构传入
   *    不存在：直接调用函数
   */
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }

  // 删除绑定在上下文上的函数，防止污染
  delete context.fn;

  // 返回结果
  return result;
};
```
