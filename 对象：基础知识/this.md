# this

## 检查语法

这段代码的结果是什么？

let user = {
  name: "John",
  go: function() { alert(this.name) }
}

(user.go)()
提示：有一个陷阱哦 :)

### 解决方案

报错。

出现此错误是因为在 user = {...} 后面漏了一个分号。

JavaScript 不会在括号 (user.go)() 前自动插入分号，所以解析的代码如下：

`let user = { go:... }(user.go)()`

(user.go)() 和 user["go"]() 相同

## 解释 "this" 的值

在下面的代码中，我们试图连续调用 obj.go() 方法 4 次。

但是前两次和后两次调用的结果不同，为什么呢？

``` javascript
let obj, method;

obj = {
  go: function() { alert(this); }
};

obj.go();               // (1) [object Object]

(obj.go)();             // (2) [object Object]

(method = obj.go)();    // (3) undefined

(obj.go || obj.stop)(); // (4) undefined
```

### 解决方案

要解释 (3) 和 (4) 得到这种结果的原因，我们需要回顾一下属性访问器（点符号或方括号）返回的是引用类型的值。

除了方法调用之外的任何操作（如赋值 = 或 ||），都会把它转换为一个不包含允许设置 this 信息的普通值。

## 在对象字面量中使用 "this"

这里 makeUser 函数返回了一个对象。

访问 ref 的结果是什么？为什么？

function makeUser() {
  return {
    name: "John",
    ref: this
  };
};

let user = makeUser();

alert( user.ref.name ); // 结果是什么？

### 解决方案

报错
this由函数调用时的上下文决定，`makeUser()`调用时this指向undefined，因此user.ref是undefined

## 创建一个计算器

创建一个有三个方法的 calculator 对象：

read() 提示输入两个值，并将其保存为对象属性。
sum() 返回保存的值的和。
mul() 将保存的值相乘并返回计算结果。
let calculator = {
  // ……你的代码……
};

calculator.read();
alert( calculator.sum() );
alert( calculator.mul() );

### 解决方案

``` javascript
let calculator = {
    read () {
        this.num1 = +prompt("input first number", 0);
        this.num2 = +prompt("input second number", 0);
    },
    sum () {
        return this.num1 + this.num2;
    },
    mul () {
        return this.num1 * this.num2;
    }
};
```
