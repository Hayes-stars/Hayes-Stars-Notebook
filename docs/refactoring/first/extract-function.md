# 提炼函数(Extract Function)

## 使用`提炼函数`的动机

- 将意图与实现分开：如果需要花时间浏览一段代码才能弄清它到底在干什么，那么就应该将其提炼到一个函数中，并根据它所做的事为其命名。

## 好处

- 短函数可读性高，职责单一，无需担心大量函数调用性能问题，短函数其实可以更容易地被缓存。

## 做法

- 创造一个新函数，根据这个函数的意图来对它命名（以它“做什么”来命名，而不是以它“怎样做”命名）。
- 将待提炼的代码从函数复制到新建的目标函数中。
- 仔细检查提炼出的代码，看看其中是否引用了`作用域限于源函数`、在提炼出的新函数中访问不到的变量。若是，以参数的形式将它们传递给新函数。
- 如果某个变量是在提炼部分之外声明但只在提炼部分使用，就把变量声明也搬移到提炼部分代码中去。
- 如果按值传递给提炼部分并且又在提炼部分被赋值了，此时如果只有一个变量，可以尝试将提炼出的新函数变成一个查询，用其返回值给该变量赋值。
- 提炼部分被赋值的局部变量太多，此时可以先放弃提炼。可优先考虑使用[拆分变量](refactoring/reorganizing-data/split-variable)或者[以查询取代临时变量](refactoring/encapsulate/replace-temp-query)，来简化变量的情况，然后再考虑提炼函数。
- 最后把提炼的函数替换目标函数的代码段。
- 再查看是否有被提炼的代码段是相同或相似的，如果有可以考虑使用[以函数调用取代内联代码](refactoring/move/func-replace-inline-code)令其调用提炼出的新函数。

## 范例

### 无局部变量

```js
function printOwing(invoice) {
	let outstanding = 0
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')

	console.log(`name: ${invoice.customer}`)
	console.log(`amount: ${outstanding}`)
	console.log(`due: ${invoice.dueDate}`)
}
```

- 提炼打印模板

```js
function printOwing(invoice) {
	let outstanding = 0

	printBanner()

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')

	console.log(`name: ${invoice.customer}`)
	console.log(`amount: ${outstanding}`)
	console.log(`due: ${invoice.dueDate}`)
}

// printBanner
function printBanner() {
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')
}
```

- 提炼打印详情

```js
function printOwing(invoice) {
	let outstanding = 0

	printBanner()

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')

	// 嵌套函数 printDetails
	function printDetails() {
		console.log(`name: ${invoice.customer}`)
		console.log(`amount: ${outstanding}`)
		console.log(`due: ${invoice.dueDate}`)
	}
}

// printBanner
function printBanner() {
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')
}
```

### 有局部变量

- 提炼打印详情

```js
function printOwing(invoice) {
	let outstanding = 0

	printBanner()

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')

	printDetails(invoice, outstanding)
}

// printBanner
function printBanner() {
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')
}

// printDetails
function printDetails(invoice, outstanding) {
	console.log(`name: ${invoice.customer}`)
	console.log(`amount: ${outstanding}`)
	console.log(`due: ${invoice.dueDate}`)
}
```

- 提炼时间代码块

```js
function printOwing(invoice) {
	let outstanding = 0

	// 打印模板
	printBanner()

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	recordDueDate(invoice)

	// 打印详情
	printDetails(invoice, outstanding)
}

// printBanner
function printBanner() {
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')
}

// printDetails
function printDetails(invoice, outstanding) {
	console.log(`name: ${invoice.customer}`)
	console.log(`amount: ${outstanding}`)
	console.log(`due: ${invoice.dueDate}`)
}

// 时间获取
function recordDueDate(invoice) {
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')
}
```

### 对局部变量再赋值

```js
function printOwing(invoice) {
	let outstanding = 0

	// 打印模板
	printBanner()

	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	recordDueDate(invoice)

	// 打印详情
	printDetails(invoice, outstanding)
}

// 首先把变量声明移动到使用处之前
function printOwing(invoice) {
	// 打印模板
	printBanner()

	let outstanding = 0
	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	// 时间获取
	recordDueDate(invoice)

	// 打印详情
	printDetails(invoice, outstanding)
}
```

- 把想要提炼的代码复制到目标函数

```js
function printOwing(invoice) {
	// 打印模板
	printBanner()

	let outstanding = calculateOutstanding(invoice)

	// 时间获取
	recordDueDate(invoice)

	// 打印详情
	printDetails(invoice, outstanding)
}

function calculateOutstanding(invoice) {
	let outstanding = 0
	for (const item of invoice.orders) {
		outstanding = item.amount
	}
	return outstanding
}
```

- 最终代码展示

```js
function printOwing(invoice) {
	// 打印模板
	printBanner()

	const outstanding = calculateOutstanding(invoice)

	// 时间获取
	recordDueDate(invoice)

	// 打印详情
	printDetails(invoice, outstanding)
}

// printBanner
function printBanner() {
	console.log('***********************')
	console.log('**** Customer Owes ****')
	console.log('***********************')
}

// printDetails
function printDetails(invoice, outstanding) {
	console.log(`name: ${invoice.customer}`)
	console.log(`amount: ${outstanding}`)
	console.log(`due: ${invoice.dueDate}`)
}

// 时间获取
function recordDueDate(invoice) {
	// https://dayjs.fenxianglu.cn/
	const today = dayjs()
	invoice.dueDate = dayjs().add(30, 'day').format('YYYY-MM-DD')
}

function calculateOutstanding(invoice) {
	let result = 0
	for (const item of invoice.orders) {
		result = item.amount
	}
	return result
}
```
