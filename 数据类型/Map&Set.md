# Map & Set

## 过滤数组中的唯一元素

定义 arr 为一个数组。

创建一个函数 unique(arr)，该函数返回一个由 arr 中所有唯一元素所组成的数组。

例如：

function unique(arr) {
  /* 你的代码 */
}

let values = ["Hare", "Krishna", "Hare", "Krishna",
  "Krishna", "Krishna", "Hare", "Hare", ":-O"
];

alert( unique(values) ); // Hare, Krishna, :-O
P.S. 这里用到了 string 类型，但其实可以是任何类型的值。

P.S. 使用 Set 来存储唯一值。

### 解决方案

``` javascript
function unique(arr) {
  return [...new Set(arr)];
}

let values = ["Hare", "Krishna", "Hare", "Krishna",
  "Krishna", "Krishna", "Hare", "Hare", ":-O"
];

console.log( unique(values) );
```

## 过滤字谜（anagrams）

Anagrams 是具有相同数量相同字母但是顺序不同的单词。

例如：

nap - pan
ear - are - era
cheaters - hectares - teachers
写一个函数 aclean(arr)，它返回被清除了字谜（anagrams）的数组。

例如：

let arr = ["nap", "teachers", "cheaters", "PAN", "ear", "era", "hectares"];

alert( aclean(arr) ); // "nap,teachers,ear" or "PAN,cheaters,era"
对于所有的字谜（anagram）组，都应该保留其中一个词，但保留的具体是哪一个并不重要。

### 解决方案

``` javascript
function aclean (arr) {
    return [...new Set(arr.map(str => str.toLowerCase().split("").sort().join("")))]
}

let arr = ["nap", "teachers", "cheaters", "PAN", "ear", "era", "hectares"];
console.log(aclean(arr));
```

## 迭代键

我们期望使用 map.keys() 得到一个数组，然后使用特定的方法例如 .push 等，对其进行处理。

但是运行不了：

``` javascript
let map = new Map();

map.set("name", "John");

let keys = map.keys();

// Error: keys.push is not a function
keys.push("more");
```

为什么？我们应该如何修改代码让 keys.push 工作？

### 解决方案

keys并不是数组，而是一个可迭代对象

`Array.from(keys)`