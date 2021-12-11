# 内联函数(Inline Function)

## 使用`内联函数`的动机

- 间接层太多使得系统中的所有函数都似乎只是对另一个函数的简单委托，在这些委托动作之间容易让人晕头转向，那么通常会使用内联函数。
- 当然，间接层有其价值，但不是所有间接层都有价值。通过内联手法，我可以找出那些有用的间接层，同时将无用的间接层去除。

## 做法

- 检查函数，确定它不具多态性。如果该函数属于一个类，并且有子类继承了这个函数，那么就无法内联。
- 找出这个函数的所有调用点。
- 将这个函数的所有调用点都替换为函数本体。
- 替换之后，测试调试。
- 删除该函数的定义。

## 范例

- 简单的一个例子

```js
  function rating(aDriver) {
    return moreThanFiveLateDeliveries(aDriver) ? 2 : 1
  }
  function moreThanFiveLateDeliveries(aDriver) {
    return aDriver.numberOfLateDeliveries > 5
  }
```

- 把return语句复制出来，粘贴到调用处取代函数

```js
  function rating(aDriver) {
    return aDriver.numberOfLateDeliveries > 5 ? 2 : 1
  }
```

- 稍微复杂一些的

```js
  function reportLines(aCustomer) {
    const lines = []
    gatherCustomerData(lines, aCustomer)
    return lines
  }
  function gatherCustomerData(out, aCustomer) {
    out.push(["name", aCustomer.name])
    out.push(["location", aCustomer.location])
  }
 ```

- 把gatherCustomerData 内联到 reportLines中

```js
  function reportLines(aCustomer) {
    const lines = []
    lines.push(["name", aCustomer.name])
    gatherCustomerData(lines, aCustomer)
    return lines
  }
  function gatherCustomerData(out, aCustomer) {
    // out.push(["name", aCustomer.name])
    out.push(["location", aCustomer.location])
  }
```

- 继续处理后面，最终代码如下

```js
  function reportLines(aCustomer) {
    const lines = []
    lines.push(["name", aCustomer.name])
    lines.push(["location", aCustomer.location])
    return lines
  }
  // function gatherCustomerData(out, aCustomer) {
  // out.push(["name", aCustomer.name])
  // out.push(["location", aCustomer.location])
  // }
```