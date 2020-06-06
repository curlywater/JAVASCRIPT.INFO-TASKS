# Async Iterator & Generator

## 分页请求

目前，有很多在线服务都是发送的分页数据（paginated data）。例如，当我们需要一个用户列表时，一个请求只返回一个预定义数量的用户（例如 100 个用户）— “一页”，并提供了指向下一页的 URL。

这种模式非常常见。不仅可用于获取用户列表，这种模式还可以用于任意东西。例如，GitHub 允许使用相同的分页提交（paginated fashion）的方式找回 commit：

我们应该提交一个请求到这种格式的 URL：https://api.github.com/repos/<repo>/commits。
它返回一个包含 30 条 commit 的 JSON，并在返回的 Link header 中提供了指向下一页的链接。
然后我们可以将该链接用于下一个请求，以获取更多 commit，以此类推。
但是我们希望有一个更简单的 API：具有 commit 的可迭代对象，然后我们就可以像这样来遍历它们：

``` javascript
let repo = 'javascript-tutorial/en.javascript.info'; // 用于获取 commit 的 GitHub 仓库

for await (let commit of fetchCommits(repo)) {
  // 处理 commit
}
```

我们想创建一个函数 fetchCommits(repo)，用来在任何我们有需要的时候发出请求，来为我们获取 commit。

### 解决方案

``` javascript
async function* fetchCommits(repo) {
    let url = `https://api.github.com/repos/${repo}/commits`;

    while (url) {
        console.log("fetch:", url);
        const response = await fetch(url);
        // <https://api.github.com/repositories/93253246/commits?page=2>; rel="next"
        const nextUrlMatch = response.headers.get("link").match(/<(.*)>; rel="next"/);
        url = nextUrlMatch && nextUrlMatch[1];
        const body = await response.json();
        for (let commit of body) {
            yield commit;
        }
    }
}

(async () => {
    let repo = 'javascript-tutorial/en.javascript.info';
    for await (commit of fetchCommits(repo)) {
        console.log(commit.author.login);
    }
})();
```
