# 提炼变量(extract-variable)

## 使用`提炼变量`的目的

- 如果这个变量名只在当前函数作用域中有意义，那么提炼变量是非常有效的方法。
- 当然如果这个变量用于更宽的上下文，建议以[提炼函数](refactoring/first/extract-function)的方法。
- 遇到工作量很大的时候，可以使用[以查询取代临时变量](refactoring/encapsulate/replace-temp-query)来处理。
- 还有一种是当前要处理的代码是属于一个类的情况，也是可以对新的变量使用[提炼函数](refactoring/first/extract-function)。

## 好处

- 表达式复杂难以阅读的情况下，局部变量可以将表达式分解变得容易管理。
- 你会发现提炼后的变量在调试和打印语句的时候非常方便。

## 做法

- 首先确认提炼的表达式是否存在副作用。
- 声明一个不可修改的变量，把想要提炼的表达式复制一份，以该表达式的结果的值给这个变量赋值。
- 用新设置的变量取代原来的表达式。

## 范例

### 函数作用域内计算表达式的提炼

```js
function price(order) {
	return (
		order.quantity * order.itemPrice -
		Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
		Math.min(order.quantity * order.itemPrice * 0.1)
	)
}
```

- 上面这个表达式可以发现，底价 = 数量*单价，因此可以创建一个变量取代表达式

```js
function price(order) {
	const basePrice = order.quantity * order.itemPrice
	return (
		basePrice -
		Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
		Math.min(order.quantity * order.itemPrice * 0.1, 100)
	)
}

// 使用了同表达式的也可以用新变量替换
function price(order) {
	const basePrice = order.quantity * order.itemPrice
	return (
		basePrice -
		Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
		Math.min(basePrice * 0.1, 100)
	)
}
```

- 接下来是提炼计算折扣的表达式

```js
function price(order) {
	const basePrice = order.quantity * order.itemPrice
	const quantityDiscount =
		Math.max(0, order.quantity - 500) * order.itemPrice * 0.05
	return basePrice - quantityDiscount + Math.min(basePrice * 0.1, 100)
}
```

- 最后再把 min 这个计算表达式提炼出来

```js
function price(order) {
	const basePrice = order.quantity * order.itemPrice
	const quantityDiscount =
		Math.max(0, order.quantity - 500) * order.itemPrice * 0.05
	const shipping = Math.min(basePrice * 0.1, 100)
	return basePrice - quantityDiscount + shipping
}
```

### 在一个类中

```js
class Order {
	constructor(aRecord) {
		this._data = aRecord
	}
	get quantity() {
		return this._data.quantity
	}
	get itemPrice() {
		return this._data.itemPrice
	}
	get price() {
		return (
			this.quantity * this.itemPrice -
			Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
			Math.min(basePrice * 0.1, 100)
		)
	}
}
```

- 提炼的还是同样的变量，适用于整个Order类

```js
class Order {
	constructor(aRecord) {
		this._data = aRecord
	}
	get quantity() {
		return this._data.quantity
	}
	get itemPrice() {
		return this._data.itemPrice
	}
	get price() {
		return (
			this.quantity * this.itemPrice -
			Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
			Math.min(basePrice * 0.1, 100)
		)
	}
  get basePrice() {
    return this.quantity * this.itemPrice
  }
  get quantityDiscount() {
    return Math.max(0, this.quantity - 500) * this.itemPrice * 0.05
  }
  get shipping() {
    return Math.min(this.basePrice * 0.1, 100)
  }
}
```
