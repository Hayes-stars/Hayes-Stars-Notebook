# 以多态取代条件表达式(Replace Conditional with Polymorphism)

## 使用`以多态取代条件表达式`的目的

- 常见的场景：商品类型，每个类型处理各自的一种条件逻辑。例如，图书、音乐、食品的处理方式不同，这是因为它们分属于不同类型的商品。最明显的征兆就是有好几个函数都有基于类型代码的 Switch 语句。若果真如此，就可以针对 Switch 语句中的每种分支逻辑创建一个类，用多态来承载各个类型特有的行为，从而去除重复的分支逻辑。

## 好处

- 对复杂的条件逻辑，多态是改善这种情况的有力工具。

## 做法

- 如果现有的类尚不具备多态的行为，就用工程函数创建之，令工厂函数返回恰当的对象实例。
- 在调用方代码中使用工程函数获得对象实例。
- 将带有条件逻辑的函数移到超类中。
  - 如果条件逻辑还未提炼到独立的函数，首先对其使用[提炼函数](refactoring/first/extractFunction)。
- 任选一个子类，在其中建立一个函数，使之覆写超类中容纳条件表达式的那个函数。将于子类相关的条件表达式分支复制到新函数中，并对它进行适当调整。
- 在超类函数中保留默认情况的逻辑。如果超类是抽象的，就把该函数声明为 abstract，或在其中直接抛出异常，表明计算责任都在子类中。

## 范例

- 有一群鸟儿, 想知道鸟飞的多快，羽毛是什么样的。通过下面这段程序来判断这些信息。

```js
function plumages(birds) {
  return new Map(birds.map(b = > [b.name, plumage(b)]))
}
function speeds(birds) {
  return new Map(birds.map(b => [b.name, airSpeedVelocity(b)]))
}
function plumage(bird) {
  switch(bird.type) {
    case 'EuropeanSwallow':
      return "average"
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? 'tired' : 'average'
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? 'scorched' : 'beautiful'
    default:
      return "unknown"
  }
}
function airSpeedVelocity(bird) {
  switch(bird.type) {
    case 'EuropeanSwallow':
      return 35
    case 'AfricanSwallow':
      return 40 - 2 * bird.numberOfCoconuts
    case 'NorwegianBlueParrot':
      return (bird*isNailed) ? 0 : 10 + bird.voltage / 10
    default:
     return null
  }
}
```

- 行为随着“鸟的类型”发生变化，因此可以创建出对应的类，用多态来处理各类型特有的行为。
- 对 airSpeedVelocity 和 plumage 两个函数使用[函数组合成类](refactoring/first/combine-func-class)

```js
function plumage(bird) {
	return new Bird(bird).plumage
}

function airSpeedVelocity(bird) {
	return new Bird(bird).apirSpeedVelocity
}

class Bird {
	constructor(birdObject) {
		Object.assign(this, birdObject)
	}
	get plumage() {
		switch (this.type) {
			case 'EuropeanSwallow':
				return 'average'
			case 'AfricanSwallow':
				return bird.numberOfCoconuts > 2 ? 'tired' : 'average'
			case 'NorwegianBlueParrot':
				return bird.voltage > 100 ? 'scorched' : 'beautiful'
			default:
				return 'unknown'
		}
	}
	get airSpeedVelocity() {
		switch (this.type) {
			case 'EuropeanSwallow':
				return 35
			case 'AfricanSwallow':
				return 40 - 2 * bird.numberOfCoconuts
			case 'NorwegianBlueParrot':
				return bird * isNailed ? 0 : 10 + bird.voltage / 10
			default:
				return null
		}
	}
}
```

- 然后针对每种鸟创建一个子类，用一个工厂函数来实例化合适的子类对象

```js
function plumage(bird) {
	return createBird(bird).plumage
}
function airSpeedVelocity(bird) {
	return createBird(bird).airSpeedVelocity
}
function createBird(bird) {
	switch (bird.type) {
		case 'EuropeanSwallow':
			return new EuropeanSwallow(bird)
		case 'AfricanSwallow':
			return new AfricanSwallow(bird)
		case 'NorwegianBlueParrot':
			return new NorwegianBlueParrot(bird)
		default:
			return new Bird(bird)
	}
}
class EuropeanSwallow extends Bird {}
class AfricanSwallow extends Bird {}
class NorwegianBlueParrot extends Bird {}
```

- 类结构创建好，可以开始处理两个条件逻辑了，先从 plumage 函数开始，从 switch 语句中选一个分支，在适当的子类中覆写这个逻辑。

```js
// 超类保留处理默认情况
class Bird {
	get plumage() {
		return 'unknown'
	}
}

class EuropeanSwallow extends Bird {
	get plumage() {
		return 'average'
	}
}

class AfricanSwallow extends Bird {
	get plumage() {
		return this.numberOfCoconuts > 2 ? 'tired' : 'average'
	}
}

class NorwegianBlueParrot extends Bird {
	get plumage() {
		return this.voltage > 100 ? 'scorched' : 'beautiful'
	}
}
```

- airSpeedVelocity 也如法炮制。完成以后，代码大致如下：

```js
function plumages(birds) {
	return new Map(birds.map((b) => createBird(b))).map((bird) => [
		bird.name,
		bird.plumage,
	])
}
function speeds(birds) {
	return new Map(birds).map((b) =>
		createBird(b).map((bird) => [bird.name, bird.airSpeedVelocity])
	)
}
function createBird(bird) {
	switch (bird.type) {
		case 'EuropeanSwallow':
			return new EuropeanSwallow(bird)
		case 'AfricanSwallow':
			return new AfricanSwallow(bird)
		case 'NorwegianBlueParrot':
			return new NorwegianBlueParrot(bird)
		default:
			return new Bird(bird)
	}
}

class Bird() {
  constructor(birdObject) {
    Object.assign(this, birdObject)
  }
  get plumage() {
    return 'unknown'
  }
  get airSpeedVelocity() {
    return null
  }
}
class EuropeanSwallow extends Bird {
	get plumage() {
		return 'average'
	}
  get airSpeedVelocity() {
    return 35
  }
}

class AfricanSwallow extends Bird {
	get plumage() {
		return this.numberOfCoconuts > 2 ? 'tired' : 'average'
	}
  get airSpeedVelocity() {
    return 40 - 2 * this.numberOfCoconuts
  }
}

class NorwegianBlueParrot extends Bird {
	get plumage() {
		return this.voltage > 100 ? 'scorched' : 'beautiful'
	}
  get airSpeedVelocity() {
    return (this.isNailed) ? 0 : 10 + this.voltage / 10
  }
}
```
