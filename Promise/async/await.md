# async/await

## 用 async/await 来重写

重写下面这个来自 Promise 链 一章的示例代码，使用 async/await 而不是 .then/catch：

function loadJson(url) {
  return fetch(url)
    .then(response => {
      if (response.status == 200) {
        return response.json();
      } else {
        throw new Error(response.status);
      }
    })
}

loadJson('no-such-user.json')
  .catch(alert); // Error: 404

### 解决方案

``` javascript
async function loadJSON(url) {
    const response = await fetch(url);
    if (response.status === 200) {
        return response.json();
    } else {
        throw new Error(response.status);
    }
}
```

## 使用 async/await 重写 "rethrow"

下面你可以看到来自 Promise 链 一章的 “rethrow” 例子。让我们来用 async/await 重写它，而不是使用 .then/catch。

同时，我们可以在 demoGithubUser 中使用循环以摆脱递归：在 async/await 的帮助下很容易实现。

``` javascript
class HttpError extends Error {
  constructor(response) {
    super(`${response.status} for ${response.url}`);
    this.name = 'HttpError';
    this.response = response;
  }
}

function loadJson(url) {
  return fetch(url)
    .then(response => {
      if (response.status == 200) {
        return response.json();
      } else {
        throw new HttpError(response);
      }
    })
}

// 询问用户名，直到 github 返回一个合法的用户
function demoGithubUser() {
  let name = prompt("Enter a name?", "iliakan");

  return loadJson(`https://api.github.com/users/${name}`)
    .then(user => {
      alert(`Full name: ${user.name}.`);
      return user;
    })
    .catch(err => {
      if (err instanceof HttpError && err.response.status == 404) {
        alert("No such user, please reenter.");
        return demoGithubUser();
      } else {
        throw err;
      }
    });
}

demoGithubUser();
```

### 解决方案

``` javascript
async function loadJson(url) {
  const response = await fetch(url)

  if (response.status == 200) {
      return response.json();
  }

  throw new HttpError(response);
}


async function demoGithubUser () {
    let user;
    while (true) {
        const name = prompt("Enter a name?", "iliakan");
        try {
            user = await loadJson(`https://api.github.com/users/${name}`);
            break;
        } catch (err) {
            if (err instanceof HttpError && err.response.status == 404) {
                alert("No such user, please reenter.");
            } else {
                throw err;
            }
        }
    }
    return user;
}
```

## 在非 async 函数中调用 async 函数

我们有一个“普通”函数。如何在这个函数中调用 async 函数并使用其结果？

async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
  // ...这里怎么写？
  // 我们需要调用 async wait() 并等待以拿到结果 10
  // 记住，我们不能使用 "await"
}
P.S. 这个任务其实很简单，但是对于 async/await 新手开发者来说，这个问题却很常见。

``` javascript
async function f() {
    wait().then(value => console.log(value));
}
```

