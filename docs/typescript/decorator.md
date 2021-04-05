# TS 装饰器

## TypeScript Decorators

- [什么是装饰器](#什么是装饰器)
- [配置](#配置)
- [如何定义](#如何定义)
- [示例：类装饰器](#示例：类装饰器)
- [示例：方法装饰器](#示例：方法装饰器)
- [示例：访问器装饰器](#示例：访问器装饰器)
- [示例：属性装饰器](#示例：属性装饰器)
- [示例：参数装饰器](#示例：参数装饰器)

## 什么是装饰器

- 装饰器的类型有：类装饰器、访问器装饰器、属性装饰器、方法装饰器、参数装饰器。

> 装饰器是一种特殊类型的声明，它能够被附加到类声明，方法，访问符，属性或参数上。 装饰器使用@expression 这种形式，expression 求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。

**[top](#typeScript-decorators)**

### 配置

- 命令行:

```bash

  tsc --target ES5 --experimentalDecorators

```

- tsconfig.json:

```json
{
  "compilerOptions": {
    "target": "ES5",
    "experimentalDecorators": true
  }
}
```

**[top](#typeScript-decorators)**

### 如何定义

> 例如，有一个@helloWord 装饰器，我们会这样定义 helloWord 函数：

```javascript

function helloWord(target: any) {
    console.log('hello Word!');
}

@helloWord
class HelloWordClass {

}

```

**[top](#typeScript-decorators)**

### 示例：类装饰器

类装饰器在类声明之前被声明（紧靠着类声明）。 类装饰器应用于类构造函数，可以用来监视，修改或替换类定义。 类装饰器不能用在声明文件中(.d.ts)，也不能用在任何外部上下文中（比如 declare 的类）。

类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。

如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。

> 注意：如果你要返回一个新的构造函数，你必须注意处理好原来的原型链。 在运行时的装饰器调用逻辑中不会为你做这些。

- 执行时机：装饰器在创建类的时候就会立即执行,并不会等到实例化的时候执行。
- 执行顺序：多个装饰器时,执行顺序是从下到上。

> 下面是类装饰器的例子:

- 普通装饰器【无法传参】

```javascript
function helloWord(target: any) {
    console.log('hello Word!');
}

@helloWord
class HelloWordClass {
    constructor() {
        console.log('我是构造函数')
    }
    name: string = 'zzb';
}

```

- 装饰器工厂【可传参】

```javascript

function helloWord(isTest: boolean) {
    return function(target: any) {
        // 添加静态变量
        target.isTestable = isTest;
    }
}

@helloWord(false)
class HelloWordClass {
    constructor() {
        console.log('我是构造函数')
    }
    name: string = 'zzb';
}
let p = new HelloWordClass();
console.log(HelloWordClass.isTestable);


```

- 重载构造函数

```javascript

function helloWord(target: any) {
    return class extends target {
        sayHello(){
            console.log("Hello")
        }
    }
}

@helloWord
class HelloWordClass {
    constructor() {
        console.log('我是构造函数')
    }
    name: string = 'zzb';
}


```

#### 装饰器执行顺序

类中不同声明上的装饰器将按以下规定的顺序应用：

1. 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。
2. 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个静态成员。
3. 参数装饰器应用到构造函数。
4. 类装饰器应用到类。

**[top](#typeScript-decorators)**

### 示例：方法装饰器

方法装饰器声明在一个方法的声明之前（紧靠着方法声明）。 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义。 方法装饰器不能用在声明文件(.d.ts)，重载或者任何外部上下文（比如declare的类）中。

方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 成员的属性描述符。

- 注意  如果代码输出目标版本小于ES5，属性描述符将会是undefined。

如果方法装饰器返回一个值，它会被用作方法的属性描述符。

- 注意  如果代码输出目标版本小于ES5返回值会被忽略。

> 下面是一个方法装饰器的例子

```javascript

/**
 * 装饰器永远是个方法，方法的装饰器，里面的三个参数是规定好的
 * @param target 普通方法 target 对应的是类的 prototype
 *               静态方法 target 对应的是类的构造函数
 * @param key 装饰方法的名字
 * @param descriptor
 */
function getNameDecorator(target: any, key: string, descriptor: PropertyDescriptor) {
  // console.log(target, key, descriptor);
  descriptor.writable = false;
}

class Test{ 
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  // 方法装饰器
  @getNameDecorator
  getName() {
    return this.name
  }
}

const test = new Test('dell');

// 这里对 getName 做了修改，打印出 123
// test.getName =  () => {
//   return '123'
// }
// console.log( test.getName() );
// 如果想不允许对这个 getName 做修改，就可以在装饰器里面进行修饰, writable 改成 false 就不能改了

test.getName =  () => {
  return '123'
}
console.log(test.getName());
// 这个时候会报错，改成 true ，则不会报错

```

#### 装饰器工厂

如果我们要定制一个修饰器如何应用到一个声明上，我们得写一个装饰器工厂函数。 装饰器工厂就是一个简单的函数，它返回一个表达式，以供装饰器在运行时调用。

我们可以通过下面的方式来写一个装饰器工厂函数：

```javascript

function color(value: string) { // 这是一个装饰器工厂
    return function (target) { //  这是装饰器
        // do something with "target" and "value"...
    }
}

```

#### 装饰器组合

多个装饰器可以同时应用到一个声明上，就像下面的示例：

- 书写在同一行

```javascript
@f @g x
```

- 书写在多行上

```javascript
@f
@g
x
```

在TypeScript里，当多个装饰器应用在一个声明上时会进行如下步骤的操作：

1. 由上至下依次对装饰器表达式求值。
2. 求值的结果会被当作函数，由下至上依次调用。

如果我们使用装饰器工厂的话，可以通过下面的例子来观察它们求值的顺序：

```javascript

function f() {
    console.log("f(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("f(): called");
    }
}

function g() {
    console.log("g(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
}

class C {
    @f()
    @g()
    method() {}
}

// 输出结果
f(): evaluated
g(): evaluated
g(): called
f(): called

```

**[top](#typeScript-decorators)**

### 示例：访问器装饰器

访问器装饰器声明在一个访问器的声明之前（紧靠着访问器声明）。 访问器装饰器应用于访问器的属性描述符并且可以用来监视，修改或替换一个访问器的定义。 访问器装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如declare的类）里。

> 注意  TypeScript不允许同时装饰一个成员的get和set访问器。取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个属性描述符时，它联合了get和set访问器，而不是分开声明的。

访问器装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 成员的属性描述符。

> 注意  如果代码输出目标版本小于ES5，Property Descriptor将会是undefined。

下面是使用了访问器装饰器的例子。

```javascript

class Point {
    private _x: number;
    private _y: number;
    constructor(x: number, y: number) {
        this._x = x;
        this._y = y;
    }

    @configurable(false)
    get x() { return this._x; }

    @configurable(false)
    get y() { return this._y; }
}

```

我们可以通过如下函数声明来定义@configurable装饰器：

```javascript:

function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}

```

**[top](#typeScript-decorators)**

### 示例：属性装饰器

属性装饰器声明在一个属性声明之前（紧靠着属性声明）。 属性装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如declare的类）里。

属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：

1. target, 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. propertyKey, 成员的名字。

> 注意  属性描述符不会做为参数传入属性装饰器，这与TypeScript是如何初始化属性装饰器的有关。 因为目前没有办法在定义一个原型对象的成员时描述一个实例属性，并且没办法监视或修改一个属性的初始化方法。返回值也会被忽略。 因此，属性描述符只能用来监视类中是否声明了某个名字的属性。

如果访问符装饰器返回一个值，它会被用作方法的属性描述符。

- 下面是使用属性装饰器的一个例子。

```javascript

function defaultValue(value: string) {
    return function (target: any, propertyName: string) {
        target[propertyName] = value;
    }
}

class HelloWordClass {
    constructor() {
        console.log('我是构造函数')
    }
    @defaultValue('zzb')
    private name: string | undefined;
}
let p = new HelloWordClass();
console.log(p.name);

// 结果
我是构造函数
zzb // 这里打印出设置的默认值

```

**[top](#typeScript-decorators)**

#### 示例：参数装饰器

参数装饰器声明在一个参数声明之前（紧靠着参数声明）。 参数装饰器应用于类构造函数或方法声明。 参数装饰器不能用在声明文件（.d.ts），重载或其它外部上下文（比如declare的类）里。

参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 参数在函数参数列表中的索引。

> 注意  参数装饰器只能用来监视一个方法的参数是否被传入。

参数装饰器的返回值会被忽略。

> 下面是参数装饰器的例子：

```javascript

function logParameter(target: any, propertyName: string, index: number) {
    // 为相应方法生成元数据键，以储存被装饰的参数的位置
    const metadataKey = `log_${propertyName}_parameters`;
    if (Array.isArray(target[metadataKey])) {
        target[metadataKey].push(index);
    } else {
        target[metadataKey] = [index];
    }
}


class HelloWordClass {
    constructor() {
        console.log('我是构造函数')
    }
    private nameVar: string | undefined;

    sayHello(@logParameter name: string) {
        console.log(name + ' sayHello');
    }
}
let pHello = new HelloWordClass();
pHello.sayHello('zzb');


```

**[top](#typeScript-decorators)**
