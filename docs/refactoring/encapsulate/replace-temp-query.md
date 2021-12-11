# 以查询取代临时变量(Replace Temp with Query)

## 使用`以查询取代临时变量`的动机

- 以查询取代临时变量手法只适用于处理某些类型的临时变量：只被计算一次且之后不再被修改的变量。
- 如果在复杂的代码片段里，变量被多次赋值的话，此时应该将这些计算代码提炼到查询函数中。

## 做法

- 检查变量在使用前是否已经完全计算完毕，检查计算它的那段代码是否每次都能得到一样的值。
- 如果变量目前不是只读的，但是可以改造成只读变量，那就先改造它。
- 调试，测试
- 将为变量赋值的代码段提炼成函数。要注意提炼的函数没有副作用，如果有，先应用[将查询函数和修改函数分离](refactoring/api/separate-query-modify)手法隔离副作用。
- 应用[内联变量](refactoring/first/inline-variable)手法移除临时变量。

## 范例

- 如以下类的代码情况

```js
class Order {
	constructor(quantity, item) {
		this._quantity = quantity
		this._item = item
	}
	get price() {
		const basePrice = this._quantity * this._item.price
		const discountFactor = 0.98
		if (basePrice > 1000) {
			discountFactor -= 0.03
		}
		return basePrice * discountFactor
	}
}
```

- 把 basePrice 和 discountFactor 两个临时变量变成函数

```js
class Order {
	constructor(quantity, item) {
		this._quantity = quantity
		this._item = item
	}
	get price() {
		const basePrice = this.basePrice
		const discountFactor = 0.98
		if (basePrice > 1000) {
			discountFactor -= 0.03
		}
		return basePrice * discountFactor
	}
	get basePrice() {
		return this._quantity * this._item.price
	}
}
```

- 测试，然后应用内联变量

```js
class Order {
	constructor(quantity, item) {
		this._quantity = quantity
		this._item = item
	}
	get price() {
		const discountFactor = 0.98
		if (this.basePrice > 1000) {
			discountFactor -= 0.03
		}
		return this.basePrice * discountFactor
	}
	get basePrice() {
		return this._quantity * this._item.price
	}
}
```

- 接下来就是如法炮制 discountFactor 变量

```js
class Order {
	constructor(quantity, item) {
		this._quantity = quantity
		this._item = item
	}
	get price() {
		const discountFactor = 0.98
		if (this.basePrice > 1000) {
			discountFactor -= 0.03
		}
		return this.basePrice * discountFactor
	}
	get basePrice() {
		return this._quantity * this._item.price
	}
	get discountFactor() {
		const discountFactor = 0.98
		if (this.basePrice > 1000) {
			discountFactor -= 0.03
		}
		return discountFactor
	}
}
```

- 最终代码展示如下：

```js
class Order {
	constructor(quantity, item) {
		this._quantity = quantity
		this._item = item
	}
	get price() {
		return this.basePrice * this.discountFactor
	}
	get basePrice() {
		return this._quantity * this._item.price
	}
	get discountFactor() {
		const discountFactor = 0.98
		if (this.basePrice > 1000) {
			discountFactor -= 0.03
		}
		return discountFactor
	}
}
```
