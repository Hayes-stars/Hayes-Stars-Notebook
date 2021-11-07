# 提炼类(Extract Class)

## 使用`提炼类`的目的

- 原本一个类应该是一个清晰的抽象，**只处理一些明确的责任**。但实际工作迭代中，类在不断的成长和扩展。因此随着责任被不断的增加，这个类会变得过分复杂，很快，类就好变成一团乱麻，可读性可维护性低。
- 一个需要维护大量函数和数据的类，往往不易于理解，此时就可以考虑分离一些独立的类。
- 如果某些数据和函数总是一起出现，某些数据经常同时变化甚至彼此相依，此时应该考虑将它们分离出去。
- 如果发现子类只营销类的部分特性，或者某些特性需要以一种方式来子类化，某些特性则需要以另一种方式子类化，这就意味着需要分解原来的类。

## 做法

- 决定如何分解类所负的责任。
- 创建一个新的类，用以表现从旧类中分离出来的责任。
- 构造旧类时创建一个新类的实例，建立“从旧类访问新类”的连接关系。
- 迁移字段，迁移函数
- 检查两个类的接口，去掉不再需要的函数，必要时为函数重新取一个适合新环境的名字。
- 决定是否公开新的类。如果需求，考虑对新类应用[将引用对象改为值对象](refactoring/reorganizing-data/change-ref-value)使其成为一个值对象。

## 范例

- 有一个简单的 Person 类

```js
class Person {
	get name() {
		return this._name
	}
	set name(arg) {
		this._name = arg
	}
	get telephoneNumber() {
		return `(${this.officeAreaCode}) ${this.officeNumber}`
	}
	get officeAreaCode() {
		return this._officeAreaCode
	}
	set officeAreaCode(arg) {
		this._officeAreaCode = arg
	}
	get officeNumber() {
		return this._officeNumber
	}
	set officeNumber(arg) {
		this._officeNumber = arg
	}
}
```

- 如上类中的代码可以将电话号码相关的行为分离到一个独立的类中

```js
class TelephoneNumber {}
```

- 接着，在构造 Person 类时创建 TelephoneNumber 类的一个实例

```js
class Person {
	constructor() {
		this._telephoneNumber = new TelephoneNumber()
	}
}
class TelephoneNumber {
	get officeAreaCode() {
		return this._officeAreaCode
	}
	set officeAreaCode(arg) {
		this._officeAreaCode = arg
	}
}
```

- 开始运用[搬移字段](refactoring/move/move-field)手法搬移一个字段

```js
class Person {
	get officeAreaCode() {
		return this._telephoneNumber.officeAreaCode
	}
	set officeAreaCode(arg) {
		this._telephoneNumber.officeAreaCode = arg
	}
}
```

- 接下来处理下一个字段 officeNumber

```js
class TelephoneNumber {
	get officeNumber() {
		return this._officeNumber
	}
	set officeNumber(arg) {
		this._officeNumber = arg
	}
}

class Person {
	get officeNumber() {
		return this._telephoneNumber.officeNumber
	}
	set officeNumber(arg) {
		this._telephoneNumber.officeNumber = arg
	}
}
```

- 再搬移对电话号码的取值函数

```js
class TelephoneNumber {
	get telephoneNumber() {
		return `(${this.officeAreaCode}) ${this.officeNumber}`
	}
}

class Person {
	get telephoneNumber() {
		return this._telephoneNumber.telephoneNumber
	}
}
```

- 最后开始做清理工作，重命名变量

```js
class TelephoneNumber {
	get areaCode() {
		return this._areaCode
	}
	set areaCode(arg) {
		this.areaCode = arg
	}
	get number() {
		return this.number
	}
	set number(arg) {
		this.number = arg
	}
}

class Person {
	get officeNumber() {
		return this._telephoneNumber.number
	}
	set officeNumber(arg) {
		this._telephoneNumber.number = arg
	}
	get officeAreaCode() {
		return this._telephoneNumber.areaCode
	}
	set officeAreaCode(arg) {
		this._telephoneNumber.areaCode = arg
	}
}
```

- TelephoneNumber 类上有一个对自己(telephone number)的取值函数也没什么道理，因此对它应用[改变函数声明](refactoring/first/change-func-delcare)手法修改函数名

```js
class TelephoneNumber {
	toTemplateString() {
		return `(${this.officeAreaCode}) ${this.officeNumber}`
	}
}
class Person {
	get telephoneNumber() {
		return this._telephoneNumber.toTemplateString()
	}
}
```

- 完整代码展示

```js
class Person {
	constructor() {
		this._telephoneNumber = new TelephoneNumber()
	}
	get telephoneNumber() {
		return this._telephoneNumber.toTemplateString()
	}
	get officeNumber() {
		return this._telephoneNumber.number
	}
	set officeNumber(arg) {
		this._telephoneNumber.number = arg
	}
	get officeAreaCode() {
		return this._telephoneNumber.areaCode
	}
	set officeAreaCode(arg) {
		this._telephoneNumber.areaCode = arg
	}
}

class TelephoneNumber {
	get areaCode() {
		return this._areaCode
	}
	set areaCode(arg) {
		this.areaCode = arg
	}
	get number() {
		return this.number
	}
	set number(arg) {
		this.number = arg
	}
	toTemplateString() {
		return `(${this.officeAreaCode}) ${this.officeNumber}`
	}
}
```

> “电话号码”对象一般还具有复用价值，因此可以考虑提炼的类暴露给更多的客户端。此时可以使用[将引用对象改为值对象](refactoring/reorganizing-data/change-ref-value)手法，具体示例查看该手法的范例。