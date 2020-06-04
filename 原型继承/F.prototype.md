# F.prototype

## 修改 "prototype"

在下面的代码中，我们创建了 new Rabbit，然后尝试修改它的 prototype。

最初，我们有以下代码：

``` javascript
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

alert( rabbit.eats ); // true
```

我们增加了一个字符串（强调）。现在 alert 会显示什么？

``` javascript
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

Rabbit.prototype = {};

alert( rabbit.eats ); // ?
```

……如果代码是这样的（修改了一行）？

``` javascript
function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

Rabbit.prototype.eats = false;

alert( rabbit.eats ); // ?
```

像这样呢（修改了一行）？

``` javascript

function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

delete rabbit.eats;

alert( rabbit.eats ); // ?
```

最后一种变体：

``` javascript

function Rabbit() {}
Rabbit.prototype = {
  eats: true
};

let rabbit = new Rabbit();

delete Rabbit.prototype.eats;

alert( rabbit.eats ); // ?
```

### 解决方案

true，`[[Prototype]]`赋值为`F.prototype`
false
true
undefined

## 使用相同的构造函数创建一个对象

想象一下，我们有一个由构造函数创建的对象 obj —— 我们不知道使用的是哪个构造函数，但是我们想使用它创建一个新对象。

我们可以这样做吗？

let obj2 = new obj.constructor();
请给出一个可以使这样的代码正常工作的 obj 的构造函数的例子。再给出会导致这样的代码无法正确工作的例子。

### 解决方案

可以，但有风险

``` javascript
function CustomObject () {};
let obj = new CustomObject();
let obj2 = new obj.constructor();

CustomObject.prototype = {};
let obj1 = new CustomObject();
let obj3 = new obj.constructor();
```