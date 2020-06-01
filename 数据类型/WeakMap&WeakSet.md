# WeakMap & WeakSet

## 存储 "unread" 标识

这里有一个 messages 数组：

let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];
你的代码可以访问它，但是 message 是由其他人的代码管理的。该代码会定期添加新消息，删除旧消息，但是你不知道这些操作确切的发生时间。

现在，你应该使用什么数据结构来保存关于消息“是否已读”的信息？该结构必须很适合对给定的 message 对象给出“它读了吗？”的答案。

P.S. 当一个消息被从 messages 中删除后，它应该也从你的数据结构中消失。

P.S. 我们不能修改 message 对象，例如向其添加我们的属性。因为它们是由其他人的代码管理的，我们修改该数据可能会导致不好的后果。

### 解决方案

WeakSet，以对象为键，对象被删除后集合中的引用被清除。

## 保存阅读日期

这儿有一个和 上一个任务 类似的 messages 数组。场景也相似。

let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];
现在的问题是：你建议采用什么数据结构来保存信息：“消息是什么时候被阅读的？”。

在前一个任务中我们只需要保存“是/否”。现在我们需要保存日期，并且它应该在消息被垃圾回收时也被从内存中清除。

P.S. 日期可以存储为内建的 Date 类的对象，稍后我们将进行介绍。

### 解决方案

WeakMap，以对象为键，日期对象为值。
