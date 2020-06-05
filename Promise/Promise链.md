# Promise链

## Promise：then 对比 catch

这两个代码片段是否相等？换句话说，对于任何处理程序（handler），它们在任何情况下的行为都相同吗？

promise.then(f1).catch(f2);
对比：

promise.then(f1, f2);

### 解决方案

不相同

- `resolve`和`reject`在同一个`then`中，因此`reject`不会处理`resolve`中的错误
- `.then` 将 result/error 传递给下一个 `.then/.catch`。所以`resolve/reject`中的错误将被传到`catch`处理

