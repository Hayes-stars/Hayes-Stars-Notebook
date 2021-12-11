# 分解条件表达式(Decompose Conditional)

## 使用`分解条件表达式`的动机

- 检查不同的条件分支，根据不同的条件做不同的事，然后很快就会得到一个相当长的函数，使得代码可读性下降。

## 好处

- 可以突出条件逻辑，更清楚地表明每个分支的作用，并且突出每个分支的原因。

## 做法

- 对条件判断和每个条件分支分别运用[提炼函数](refactoring/first/extractFunction)手法。

## 范例

> 假设计算购买某样商品的总价(总价=数量*单价)，而这个商品在冬季和夏季的单价是不同的。

### 范例代码

```js
  let charge = 0
  if(!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)){
    charge = quantity * plan.summerRate
  } else {
    charge = quantity * plan.regularRate + plan.regularServiceCharge
  }

  // 把条件判断提炼到一个独立的函数中：
  is(summer()){
    charge = quantity * plan.summerRate
  } else {
    charge = quantity * plan.regularRate + plan.regularServiceCharge
  }

  function summer() {
    return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)
  }

  // 然后提炼条件判断为真的分支：
  if(summer()) {
    charge = quantity * plan.regularRate + plan.regularServiceCharge
  }

  function summer() {
    return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)
  }

  function summerCharge() {
    return quantity * plan.summerRate
  }

  // 最后提炼条件判断为假的分支：
  if(summer()) {
    charge = summerCharge()
  } else {
    charge = regularCharge()
  }

  function summer() {
    return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)
  }

  function summerCharge() {
    return quantity * plan.summerRate
  }

  function regularCharge() {
    return quantity*plan.regularRate + plan.regularServiceCharge
  }


  // 提炼函数完成后，还可以使用三元运算符重新安排条件语句
  charge = summer() ? summerCharge() : regularCharge()

  function summer() {
    return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)
  }

  function summerCharge() {
    return quantity * plan.summerRate
  }

  function regularCharge() {
    return quantity*plan.regularRate + plan.regularServiceCharge
  }

```
