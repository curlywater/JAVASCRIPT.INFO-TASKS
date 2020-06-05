# Promise

## 用 promise 重新解决？

下列这段代码会输出什么？

``` javascript
let promise = new Promise(function(resolve, reject) {
  resolve(1);

  setTimeout(() => resolve(2), 1000);
});

promise.then(alert);
```

### 解决方案

1

## 基于 promise 的延时

内建函数 setTimeout 使用了回调函数。请创建一个基于 promise 的替代方案。

函数 delay(ms) 应该返回一个 promise。这个 promise 应该在 ms 毫秒后被 resolve，所以我们可以向其中添加 .then，像这样：

function delay(ms) {
  // 你的代码
}

delay(3000).then(() => alert('runs after 3 seconds'));

### 解决方案

``` javascript
function delay(ms) {
  // 你的代码
  return new Promise(resolve => setTimeout(resolve, ms));
}

delay(3000).then(() => console.log('runs after 3 seconds'));

```

## 带有 promise 的圆形动画

重写任务 带回调的圆形动画 的解决方案中的 showCircle 函数，以使其返回一个 promise，而不接受回调。

新的用法：

showCircle(150, 150, 100).then(div => {
  div.classList.add('message-ball');
  div.append("Hello, world!");
});
以任务 带回调的圆形动画 的解决方案为基础。

### 解决方案

``` html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <style>
    .circle {
      transition-property: width, height, margin-left, margin-top;
      transition-duration: 2s;
      position: fixed;
      transform: translateX(-50%) translateY(-50%);
      background-color: red;
      border-radius: 50%;
    }
  </style>
</head>

<body>

  <script>
  function showCircle(cx, cy, radius) {
    let div = document.createElement('div');
    div.style.width = 0;
    div.style.height = 0;
    div.style.left = cx + 'px';
    div.style.top = cy + 'px';
    div.className = 'circle';
    document.body.append(div);
    return new Promise((resolve) => {
        setTimeout(() => {
            div.style.width = radius * 2 + 'px';
            div.style.height = radius * 2 + 'px';
            // 才知道有transitionend事件，在CSS transition结束的时候触发，孤陋寡闻...
            div.addEventListener('transitionend', function handler() {
                div.removeEventListener('transitionend', handler);
                resolve(div);
            });
        }, 0);
    });
  }
  showCircle(150, 150, 100).then(div => {
    div.classList.add('message-ball');
    div.append("Hello, world!");
  });
  </script>


</body>
</html>
```
