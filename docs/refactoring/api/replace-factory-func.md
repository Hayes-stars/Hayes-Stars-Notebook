# 以工厂函数取代构造函数(Replace Constructor with Factory Function)

## 使用`以工厂函数取代构造函数`的动机

- 由于构造函数的局限性，无法跟进环境或者参数信息返回子类实例或代理对象；
- 构造函数的名字是固定的，因此无法使用比默认名字更清晰的函数名；
- 构造函数需求通过特殊的操作符来调用(new 关键字)，所以在要求普通函数的场合就难以使用；
- 工厂函数不受这些限制。工厂函数的实现内部可以调用构造函数。

## 做法

- 新建一个工厂函数，让它调用现有的构造函数。
- 将调用构造函数的代码改为调用工厂函数。
- 测试代码。
- 缩小构造函数的可见范围。

## 范例

```js
// 员工调薪系统
class Employee {
  constructor(name, typeCode) {
    this._name = name
    this._typeCode = typeCode
  }
  get name() {return this._name}
  get type() {
    return Employee.legalTypeCodes[this._typeCode]
  }
  static get legalTypeCodes() {
    return {
      "E": "Engineer",
      "M": "Manager",
      "S": "Salesman"
    }
  }
}

// 调用方：
candidate = new Employee(document.name, document.empType)
const leadEngineer = new Employee(document.leadEngineer, "E")


// 第一步，创建工厂函数
function createEmployee(name, typeCode) {
  return new Employee(name, typeCode)
}

// 第一处调用方修改
candidate = createEmployee(document.name, document.empType)

// 第二处调用方修改
const leadEngineer = createEmployee(document.leadEngineer, "E")

// 第三处修改，创建一个工厂函数，把“类别”的信息放在函数体里面去实现
const leadEngineer = createEngineer(document.leadEngineer)

function createEngineer(name) {
  return new Employee(name, "E")
}

```
