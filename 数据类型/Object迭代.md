# Object迭代

## 属性求和
有一个带有任意数量薪水的 salaries 对象。

编写函数 sumSalaries(salaries)，该函数使用 Object.values 和 for..of 循环返回所有薪水的总和。

如果 salaries 是空对象，那么结果必须是 0。

举个例子：

let salaries = {
  "John": 100,
  "Pete": 300,
  "Mary": 250
};

alert( sumSalaries(salaries) ); // 650

### 解决方案

``` javascript
function sumSalaries (salaries) {
    let sum = 0;
    for (const salary of Object.values(salaries)) {
        sum += salary;
    }
    return sum;
}

let salaries = {
  "John": 100,
  "Pete": 300,
  "Mary": 250
};

console.log( sumSalaries(salaries) ); // 650
```

## 计算属性数量

写一个函数 count(obj)，该函数返回对象中的属性的数量：

let user = {
  name: 'John',
  age: 30
};

alert( count(user) ); // 2
试着使代码尽可能简短。

P.S. 忽略 Symbol 类型属性，只计算“常规”属性。

### 解决方案

``` javascript
function count (obj) {
    return Object.keys(obj).length
}
let user = {
  name: 'John',
  age: 30
};

console.log( count(user) ); // 2
```
