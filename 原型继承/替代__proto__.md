# 替代`__proto__`

## 为 dictionary 添加 toString 方法

这儿有一个通过 Object.create(null) 创建的，用来存储任意 key/value 对的对象 dictionary。

为该对象添加 dictionary.toString() 方法，该方法应该返回以逗号分隔的所有键的列表。你的 toString 方法不应该在使用 for...in 循环遍历数组的时候显现出来。

它的工作方式如下：

``` javascript
let dictionary = Object.create(null);

// 你的添加 dictionary.toString 方法的代码

// 添加一些数据
dictionary.apple = "Apple";
dictionary.__proto__ = "test"; // 这里 __proto__ 是一个常规的属性键

// 在循环中只有 apple 和 __proto__
for(let key in dictionary) {
  alert(key); // "apple", then "__proto__"
}

// 你的 toString 方法在发挥作用
alert(dictionary); // "apple,__proto__"
```

### 解决方案

``` javascript
let dictionary = Object.create(null);

// 你的添加 dictionary.toString 方法的代码

// 添加一些数据
dictionary.apple = "Apple";
dictionary.__proto__ = "test"; // 这里 __proto__ 是一个常规的属性键

Object.defineProperty(dictionary, "toString", {
    value: function () {
        return [...Object.keys(this)].join();
    },
    writable: true,
    configurable: true
})

// 在循环中只有 apple 和 __proto__
for(let key in dictionary) {
  console.log(key); // "apple", then "__proto__"
}

// 你的 toString 方法在发挥作用
console.log(dictionary); // "apple,__proto__"
```

``` javascript
let dictionary = Object.create(null, {
  toString: { // 定义 toString 属性
    value() { // value 是一个 function
      return Object.keys(this).join();
    }
  }
});

dictionary.apple = "Apple";
dictionary.__proto__ = "test";

// apple 和 __proto__ 在循环中
for(let key in dictionary) {
  console.log(key); // "apple"，然后是 "__proto__"
}

// 通过 toString 处理获得的以逗号分隔的属性列表
console.log(dictionary); // "apple,__proto__"
```

## 调用方式的差异

让我们创建一个新的 rabbit 对象：

``` javascript
function Rabbit(name) {
  this.name = name;
}
Rabbit.prototype.sayHi = function() {
  alert(this.name);
};

let rabbit = new Rabbit("Rabbit");
```

以下调用做的是相同的事儿还是不同的？

``` javascript
rabbit.sayHi();
Rabbit.prototype.sayHi();
Object.getPrototypeOf(rabbit).sayHi();
rabbit.__proto__.sayHi();
```

### 解决方案

不同

``` javascript
rabbit.sayHi(); // "Rabbit"
Rabbit.prototype.sayHi(); // undefined; Rabbit.prototype: {constructor: Rabbit; sayHi: function};
Object.getPrototypeOf(rabbit).sayHi(); // undefined; Rabbit.sayHi();
rabbit.__proto__.sayHi(); // undefined; Rabbit.sayHi();
```