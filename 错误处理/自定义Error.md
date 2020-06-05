# 自定义Error

## 继承 SyntaxError

创建一个继承自内建类 SyntaxError 的类 FormatError。

它应该支持 message，name 和 stack 属性。

用例：

let err = new FormatError("formatting error");

alert( err.message ); // formatting error
alert( err.name ); // FormatError
alert( err.stack ); // stack

alert( err instanceof FormatError ); // true
alert( err instanceof SyntaxError ); // true（因为它继承自 SyntaxError）

### 解决方案

``` javascript
class FormatError extends SyntaxError {
    constructor(message) {
        super(message);
        this.name = this.constructor.name;
    }
}
```
