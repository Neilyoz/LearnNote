# bind、call、apply区别

要看明白一个东西最好方式就是去拆解开来。

首先我们看看bind、call、apply分别是什么？

bind、call、apply其实是Function.prototype上的方法，他们的真身就是以下三个方法：

- Function.prototype.bind(thisArg[,arg1[,arg2[, ...]]]);
- Function.prototype.call(thisArg, arg1, arg2, ...);
- Function.prototype.apply(thisArg, [argsArray]);

以上均来自MDN文档的描述。学习和使用一门技术最好就是从文档入手。



我们学习一样东西最主要的目的是使用，使用的话就必须了解这三个方法的作用是什么？

bind、call、apply的作用都是为了解决改变`this`指向。



#### 为什么要改变this的指向？

在JavaScript中，`this`不是固定不变的，它就像个万金油随着环境改变而改变，不得不佩服有这种能力的东西。

- 在方法中，`this`表示该方法所属的对象

```javascript
const student = {
  name: '韩梅梅',
  title: '班长',
  showIdentity: function() {
    return this.title + ':' + this.name;
  }
}

student.showIdentity(); // 班长：韩梅梅
// 这里的this表示student这个对象
```

- 单独使用， `this` 表示全局对象

```javascript
let name = this;
// 在nodejs环境中，this表示的是全局对象(Object [global])
// 在浏览器环境中，this表示的是window对象
```

- 在函数中，`this` 表示全局对象

```javascript
// 在nodejs环境中，this表示的是全局对象(Object [global])
// 在浏览器环境中，this表示的是window对象
function testFunc() {
  return this;
}

// 严格模式下稍有不同，this是undefined
"use strict";
function testFunc2() {
  return this;
}
```

- 事件中的this，指向接收事件的HTML元素

```html
<button onclick="this.style.display='none'">
	给我消失
</button>
```

由于 `this` 使用的灵活性，为了确保 `this` 能够使用正确，我们可以使用bind、call、apply来给`this`指定一个身份。



#### bind、call、apply相同点与不同点

相同点：

- 第一个参数都是 需要指向的对象

不同点：

- bind的返回值是一个函数
- call、apply返回的是一个函数的执行结果
- bind、call在第一个参数后，可以跟任意多个参数，而apply在第一个参数后，跟的是参数数组

#### 具体使用例子

例子一

```javascript
function showIdentity() {
  let argumentStr = "";
  if (arguments) {
    argumentStr = [...arguments].join(",");
    return this.title + ":" + this.name + " " + argumentStr;
  }

  return this.title + ":" + this.name;
}

let student = {
  name: "韩梅梅",
  title: "班长"
};

const result01 = showIdentity.call(student, "好班长", "拒绝李雷");
console.log(result01); // 班长：韩梅梅 好班长,拒绝李雷

const result02 = showIdentity.call(student);
console.log(result02); // 班长：韩梅梅
```

例子二

```javascript
function showIdentity() {
  let argumentStr = "";
  if (arguments) {
    argumentStr = [...arguments].join(",");
    return this.title + ":" + this.name + " " + argumentStr;
  }
  return this.title + ":" + this.name;
}

let student = {
  name: "韩梅梅",
  title: "班长"
};

const result01 = showIdentity.apply(student, ["三好学生", "好儿女"]);
console.log(result01); // 班长:韩梅梅 三好学生,好儿女

const result02 = showIdentity.apply(student);
console.log(result02); // 班长:韩梅梅
```

例子三

```javascript
function showIdentity() {
  let argumentStr = "";
  if (arguments) {
    argumentStr = [...arguments].join(",");
    return this.title + ":" + this.name + " " + argumentStr;
  }
  return this.title + ":" + this.name;
}

let student = {
  name: "韩梅梅",
  title: "班长"
};

const newShowIdentity = showIdentity.bind(student, ["三好学生", "好儿女"]);
console.log(newShowIdentity('厉害啊')); // 班长:韩梅梅 三好学生,好儿女,厉害啊
```



