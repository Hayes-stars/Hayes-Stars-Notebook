# 以卫语句取代嵌套条件表达式(Replace Nested Conditional with Guard Clauses)

## 使用`以卫语句取代嵌套条件表达式`的原因和动机

- 两种风格：
  - 两个条件分支都属于正常行为。
  - 只有一个条件分支是正常行为，另一个分支则是异常的情况。
- 卫语句的精髓就是：给某一条分支以特别的重视。

## 做法

- 选中最外层需要被替换的条件逻辑，将其替换为卫语句。
- 如果所有卫语句都引发同样的结果，可以使用[合并条件表达式](refactoring/conditional/consolidate)合并之。

## 范例

- 需求：计算员工(employee)的工资，只有还在公司上班的员工才需要支付工资，所以这个函数需要检查两种“员工已经不在公司上班”的情况。

```js
function payAmount(employee) {
	let result
	if (employee.isSeparated) {
		result = {
			amount: 0,
			reasonCode: 'SEP',
		}
	} else {
		if (employee.isRetired) {
			result = {
				amount: 0,
				reasonCode: 'RET',
			}
		} else {
			// 相关计算工资逻辑...
			result = someFinalComputation()
		}
	}
	return result
}
```

- 简化最顶上的条件逻辑

```js
function payAmount(employee) {
	let result
	if (employee.isSeparated) return { amount: 0, reasonCode: 'SEP' }

	if (employee.isRetired) {
		result = {
			amount: 0,
			reasonCode: 'RET',
		}
	} else {
		// 相关计算工资逻辑...
		result = someFinalComputation()
	}
	return result
}
```

- 简化下一步条件逻辑

```js
function payAmount(employee) {
	let result
	if (employee.isSeparated) return { amount: 0, reasonCode: 'SEP' }
	if (employee.isRetired) return { amount: 0, reasonCode: 'RET' }
	// 相关计算工资逻辑...
	result = someFinalComputation()
	return result
}
```

- 简化逻辑后，Result 变量就没有用处了，删掉 Result

```js
function payAmount(employee) {
	if (employee.isSeparated) return { amount: 0, reasonCode: 'SEP' }
	if (employee.isRetired) return { amount: 0, reasonCode: 'RET' }
	// 相关计算工资逻辑...
	return someFinalComputation()
}
```

## 范例：将条件反转

- 将条件表达式反转，实现`以卫语句取代嵌套条件表达式`.

```js
function adjustedCapital(anInstrument) {
	let result = 0
	if (anInstrument.capital > 0) {
		if (anInstrument.interestRate > 0 && anInstrument.duration > 0) {
			result =
				(anInstrument.income / anInstrument.duration) *
				anInstrument.adjustmentFactor
		}
	}
	return result
}

// 条件反转第一步
function adjustedCapital(anInstrument) {
	let result = 0
	if (anInstrument.capital <= 0) return result
	if (anInstrument.interestRate > 0 && anInstrument.duration > 0) {
		result =
			(anInstrument.income / anInstrument.duration) *
			anInstrument.adjustmentFactor
	}
	return result
}

// 加入逻辑非操作
function adjustedCapital(anInstrument) {
	let result = 0
	if (anInstrument.capital <= 0) return result
	if (!(anInstrument.interestRate > 0 && anInstrument.duration > 0))
		return result
	result =
		(anInstrument.income / anInstrument.duration) *
		anInstrument.adjustmentFactor
	return result
}

// 逻辑非简化
function adjustedCapital(anInstrument) {
	let result = 0
	if (anInstrument.capital <= 0) return result
	if (anInstrument.interestRate <= 0 || anInstrument.duration <= 0)
		return result
	result =
		(anInstrument.income / anInstrument.duration) *
		anInstrument.adjustmentFactor
	return result
}
```

- 其实两个条件引发的结果是一样的，还可以用[合并条件表达式](refactoring/conditional/consolidate)将其合并

```js
function adjustedCapital(anInstrument) {
	let result = 0
	if (
		anInstrument.capital <= 0 ||
		anInstrument.interestRate <= 0 ||
		anInstrument.duration <= 0
	)
		return result
	result =
		(anInstrument.income / anInstrument.duration) *
		anInstrument.adjustmentFactor
	return result
}
```

- 简化逻辑后，Result 变量就没有用处了，删掉 Result

```js
function adjustedCapital(anInstrument) {
	if (
		anInstrument.capital <= 0 ||
		anInstrument.interestRate <= 0 ||
		anInstrument.duration <= 0
	) {
		return 0
	}
	return (
		(anInstrument.income / anInstrument.duration) *
		anInstrument.adjustmentFactor
	)
}
```
