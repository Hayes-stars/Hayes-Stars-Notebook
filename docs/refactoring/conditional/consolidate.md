# 合并条件表达式(Consolidate Conditional Expression)

## 使用`合并条件表达式`的原因和动机

- 检查条件各不相同，最终行为一致。这种情况应该使用“逻辑或”和“逻辑与”将它们合并为一个条件表达式。
- 有合并的理由，同时就有不合并的理由：如果认为这些条件检查的确是彼此独立的，的确不应该被视为同一次检查，就不需要使用本项的重构。

## 好处

- 可以为使用[提炼函数](refactoring/first/extractFunction)做好准备。把检查条件提炼成一个独立的函数，把描述“做什么”的语句换成了“为什么这样做”。

## 做法

- 确定条件表达式没有副作用
  - 如果某个条件表达式有副作用，可以先用[将查询函数和修改函数分离](refactoring/api/sqfm)处理。
- 使用适当的逻辑运算符，将两个相关条件表达式合并为一个。
  - 顺序执行的条件表达式用逻辑或来合并，嵌套的 if 语句用逻辑与来合并。
- 考虑对合并后的条件表达式实施[提炼函数](refactoring/first/extractFunction)

## 范例 逻辑或

```js
function disabilityAmount(anEmployee) {
	if (anEmployee.seniority < 2) {
		return 0
	}
	if (anEmployee.monthsDisabled > 12) {
		return 0
	}
	if (anEmployee.isPartTime) {
		return 0
	}
}

// 顺序执行的条件检查，用逻辑或运算符来合并

function disabilityAmount(anEmployee) {
	if (
		anEmployee.seniority < 2 ||
		anEmployee.monthsDisabled > 12 ||
		anEmployee.isPartTime
	) {
		return 0
	}
}
```

- 合并后，再对条件表达式使用[提炼函数](refactoring/first/extractFunction)

```js
function disabilityAmount(anEmployee) {
  if(isNotEligibleForDisability()) {
    return 0
  }
}

function isNotEligibleForDisability() {
	return (
		anEmployee.seniority < 2 ||
		anEmployee.monthsDisabled > 12 ||
		anEmployee.isPartTime
	)
}
```

## 范例逻辑与

```js

// 嵌套if语句的情况
if(anEmployee.onVacation) {
  if(anEmployee.seniority > 10) {
    return 1
  }
}
return 0.5

// 逻辑与运算符将其合并
if((anEmployee.onVacation) && (anEmployee.seniority > 10)) {
  return 1
}
return 0.5

```