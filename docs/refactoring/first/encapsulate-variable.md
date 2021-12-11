# 封装变量(Encapsulate-variable)

> 到底应该封装什么，以及如何封装，取决于数据被使用的方式，以及我想要修改数据的方式。
## 好处

- 封装数据能提供一个清晰的观测点，由此监控数据的变化和使用情况；
- 对所有可变的数据，作用域超出单个函数，将其封装起来通过函数访问，数据的作用域越大，封装就越重要，缩小字段可见范围。

## 做法

- 创建封装函数，在其中访问和更新变量值。
- 逐一修改使用该变量的代码，将其改为调用合适的封装函数。每次替换之后都要测试通过。
- 限制变量的可见性。如果没办法阻止直接访问变量，可以试试将变量改名，再测试，找出仍在直接使用该变量的代码。
- 如果变量的值是一个记录，可以考虑使用[封装记录](refactoring/encapsulate/encapsulate-record)。

## 范例


### 数据结构引用封装

```js
  // 全局变量中保存了一些有用的数据
  let defaultOwner = {firstName: "Martin", lastName: "Fowler"}

  // 赋值
  spaceship.owner = defaultOwner

  // 更新
  defaultOwner = {firstName: "Hayes", lastName: "Stars"}

  // 首先定义读取和写入这段数据的函数，先给它做个封装
  function getDefaultOwner() {
    return defaultOwner
  }

  function setDefaultOwner(arg) {
    defaultOwner = arg
  }

  // 然后处理defaultOwner 的调用
  spaceship.owner = getDefaultOwner();

  // 变量赋值就改为调用设值函数
  setDefaultOwner({firstName: "Hayes", lastName: "Stars"})



  // 处理完成后，可以单独放到一个JS文件中
  // defaultOwner.js 代码如下
  let defaultOwner = {firstName: "Martin", lastName: "Fowler"}
  export function getDefaultOwner() {return defaultOwner}
  export function setDefaultOwner(arg) { defaultOwner = arg }

```

### 封装值
> 上面的封装只能控制对该数据结构的访问和重新赋值，但是不能控制对结构内部数据项的修改：

- 两种方法：第一种简单的办法是修改取值函数，返回一份副本数据

```js

  // 控制对变量内容的修改
  // 修改取值函数，使其返回该数据的一份副本

  // defaultOwner.js 代码如下
  // 变量名 defaultOwner 修改为 defaultOwnerData 
  let defaultOwnerData = {firstName: "Martin", lastName: "Fowler"}
  export function getDefaultOwner() {return Object.assign({}, defaultOwnerData)} // 使用副本则不会影响到共享的这份数据
  export function setDefaultOwner(arg) { defaultOwnerData = arg }

```

- 另一种做是通过[封装记录](refactoring/encapsulate/encapsulate-record)，代码如下

```js
  let defaultOwnerData = {firstName: "Martin", lastName: "Fowler"}
  export function getDefaultOwner() {return new Person(defaultOwnerData)} 
  export function setDefaultOwner(arg) { defaultOwnerData = arg }

  class Person {
    constructor(data) {
      this._lastName = data.lastName
      this._firstName = data.firstName
    }
    get lastName() {return this._lastName}
    get firstName() {return this._firstName}
  }
```
