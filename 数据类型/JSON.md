# JSON

## 将对象转换为 JSON，然后再转换回来

将 user 转换为 JSON，然后将其转换回到另一个变量。

let user = {
  name: "John Smith",
  age: 35
};

### 解决方案

``` javascript
JSON.parse(JSON.stringify(user));
```

## 排除反向引用

在简单循环引用的情况下，我们可以通过名称排除序列化中违规的属性。

但是，有时我们不能只使用名称，因为它既可能在循环引用中也可能在常规属性中使用。因此，我们可以通过属性值来检查属性。

编写 replacer 函数，移除引用 meetup 的属性，并将其他所有属性序列化：

``` javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  occupiedBy: [{name: "John"}, {name: "Alice"}],
  place: room
};

// 循环引用
room.occupiedBy = meetup;
meetup.self = meetup;

console.log( JSON.stringify(meetup, function replacer(key, value) {
    // 排除自身
    return key !== "" && value === meetup ? undefined : value;
}));

```

/* 结果应该是：
{
  "title":"Conference",
  "occupiedBy":[{"name":"John"},{"name":"Alice"}],
  "place":{"number":23}
}
*/