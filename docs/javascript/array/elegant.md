# 更优雅的编写 JavaScript

在国内很多开发项目都是需要考虑 IE8 的兼容，为了兼容很多 JavaScript 好用的方法和技巧都被埋没了。但是我发现近几年开始，很多开发项目已经完全抛弃了 IE 这个魔鬼了。如果你不需要兼容“石器时代”的 IE 浏览器了，那就要开始熟悉一下这几个方法来处理数组。

## .map()

让我用一个简单的例子告诉你如何使用这个方法。假如你现在有多对象的数组数据 - 每一个对象代表着一个员工的信息。现在你想要的最终结果就是取出所有员工的唯一 ID 值。

```javascript
// 员工数据
var employees = [
  {id: 20, name: '001'},
  {id: 24, name: '002'},
  {id: 56, name: '003'},
  {id: 88, name: '004'},
][
  // 你想要的结果
  (20, 24, 56, 88)
]
```

其实要实现这个结果有很多数组处理方式。传统的处理方法就是先定义一个空数组，然后使用.forEach()，.for(...of)，或者是最简单的.for()来组装 ID 到你定义的数组里面。

我们来对比一下传统的处理方式和.map()的区别。

使用.forEach()：

```javascript
var employeeIds = []
employees.forEach(function (employee) {
  employeeIds.push(officer.id)
})
```

注意使用传统的方式，我们必须有一个预定义的空数组变量才行。但是如果是.map()就会更简单了。

```javascript
var employeeIds = employees.map(function (employee) {
  return employee.id
})
```

甚至我们可以用更简洁的方式，使用箭头方法（但是需要 ES6 支持，Babel，或者 TypeScript）。

```javascript
const employeeIds = employees.map((employee) => employee.id)
```

所以.map()到底是怎么运作的呢？这个方法有两个参数，第一是回调方法，第二是可选内容（会在回调方法中做为 this）。数组里的每个数值/对象会被循环进入到回调方法里面，然后返回新的数值/对象到结果数组里面。

注意结果数组的长度永远都会和被循环的数组的长度一致。

---

## .reduce()

与.map()相识，.reduce()也是循环一个回调方法，数组里面的每一个元素对回进入回调方法。区别是回调方法返回的值会被传递到下一个回调方法，如此类推（等同于一个累加器）。

.reduce()里的累加值可以是任何属性的值，包括 integer，string，object 等等。这个累加值会被实力化或者传递到下一个回调方法。

来上代码，做个简单的例子！假如你有一个飞行员的数组，数组里面有每个飞行员的工龄。

```javascript
var pilots = [
  {
    id: 10,
    name: '飞行员001',
    years: 14,
  },
  {
    id: 2,
    name: '飞行员002',
    years: 30,
  },
  {
    id: 41,
    name: '飞行员003',
    years: 16,
  },
  {
    id: 99,
    name: '飞行员004',
    years: 22,
  },
]
```

现在我们需要计算所有飞行员累计的总工龄。使用.reduce()就是比吃饭还简单的事情。

```javascript
var totalYears = pilots.reduce(function (accumulator, pilot) {
  return accumulator + pilot.years
}, 0)
```

注意我这里第二个参数我传了 0。第二个参数是一个累加值的初始值。当然如果场景需要这个初始值也可以传入一个变量或者你需要的值。循环了数组里的每一个元素后，reduce 方法会返回最终累加后的值（在我们这个例子中就是 82）。

例子里面的 acc 和 accumulator 就是累加值变量

如果是使用 ES6 箭头写法，我们可以写的更加优雅简洁。一行就可以搞定的事情！

```javascript
const totalYears = pilots.reduce((acc, pilot) => acc + pilot.years, 0)
```

现在如果我们需要找到哪一位是最有经验的飞行员。这种情况我们一样可以使用.reduce()。

```javascript
var mostExpPilot = pilots.reduce(function (oldest, pilot) {
  return (oldest.years || 0) > pilot.years ? oldest : pilot
}, {})
```

这里我把 accumulator 变量改为 oldest 代表飞行员里面的老司机。这时候 reduce 里面的回调方法对比每一个飞行员，每一次飞行员的值进入这个回调方法，工龄更高的就会覆盖 oldest 变量。最终循环后得到的 oldest 就是工龄最高的飞行员。

通过这几个例子，你可以看到使用.reduce()可以简单又优雅的在一个数组里面获取到单个最终值或者对象。

---

## .filter()

如果你现在的场景是需要在一个数组里面过滤一部分的数据，这个时候.filter()就是你的最好的朋友了。

我们用回飞行员的数据，并且加入了所属航空公司的值:

```javascript
var pilots = [
  {
    id: 2,
    name: '飞行员007',
    faction: '南航',
  },
  {
    id: 8,
    name: '飞行员008',
    faction: '川航',
  },
  {
    id: 40,
    name: '飞行员009',
    faction: '川航',
  },
  {
    id: 66,
    name: '飞行员010',
    faction: '南航',
  },
]
```

加入现在我们想分别筛选出'南航'和'川航'两个航空公司的飞行员，使用.filter()就是轻而易举的事情！

```javascript
var rebels = pilots.filter(function (pilot) {
  return pilot.faction === '南航'
})
var empire = pilots.filter(function (pilot) {
  return pilot.faction === '川航'
})
```

就这么简单，如果使用箭头方法（ES6）就更加优雅了：

```javascript
const rebels = pilots.filter((pilot) => pilot.faction === '南航')
const empire = pilots.filter((pilot) => pilot.faction === '川航')
```

其实原理很简单，只要你的回调方法返回的是 true，这个值或者对象就会在新的数组里面了。如果返回的是 false 就会被过滤掉了。

## 结合使用 .map()，.reduce()，.filter()

既然我们刚刚学到的三个函数都是可以用于数组的，并且.map()和.filter()都是返回数组的。那我们就可以串联起来使用。不说多了上代码试试！

我们用一个有趣一点的数据试验一下，假如现在我们有一个星球大战里面的人物的数组。每个字段的定义如下：

- Id: 人物唯一 ID
- name: 人物名字
- pilotingScore: 飞行能力指数
- shootingScore: 射击能力指数
- isForceUser: 是否拥有隔空操控能力

我们的目标：获取拥有隔空操控能力的飞行员的总飞行能力指数。我们先分开一步一步实现这个目标！

首先我们需要先获取到拥有隔空操控能力的飞行员。

```javascript
var jediPersonnel = personnel.filter(function (person) {
  return person.isForceUser
})
// 结果集: [{...}, {...}, {...}] (Luke, Ezra and Caleb)
```

这段代码我们获得了 3 个飞行员对象，分别都是拥有隔空操控能力的飞行员。使用这个对象我们来获取每个飞行员的飞行能力指数值。

```javascript
var jediScores = jediPersonnel.map(function (jedi) {
  return jedi.pilotingScore + jedi.shootingScore
})
// 结果: [154, 110, 156]
```

获取到每个飞行员的飞行能力指数值后，我们就可以用累加器（.reduce()）获取总飞行能力指数了。

```javascript
var totalJediScore = jediScores.reduce(function (acc, score) {
  return acc + score
}, 0)
// 结果: 420
```

这里分开实现方式可以达到我们的目标，但是其实我们可以串联起来，可以写的更加简洁又优雅！我们来玩玩更好玩的吧！

```javascript
var totalJediScore = personnel
  .filter(function (person) {
    return person.isForceUser
  })
  .map(function (jedi) {
    return jedi.pilotingScore + jedi.shootingScore
  })
  .reduce(function (acc, score) {
    return acc + score
  }, 0)
```

这样写是不是很优雅！都被这段代码给美到了！

如果我们使用箭头写法 ES6，就更加优雅了！

```javascript
const totalJediScore = personnel
  .filter((person) => person.isForceUser)
  .map((jedi) => jedi.pilotingScore + jedi.shootingScore)
  .reduce((acc, score) => acc + score, 0)
```

其实我们只需要使用.reduce()就可以得到我们的目标结果了，以上例子做为教学例子，所以使用了 3 个我们学到的函数。

我们来看看只用.reduce()怎么实现的，来我们一起来刷新一下三观吧！

```javascript
const totalJediScore = personnel.reduce(
  (acc, person) =>
    person.isForceUser
      ? acc + person.pilotingScore + person.shootingScore
      : acc,
  0
)
```

尝试用.map()，.reduce()，.filter()来替换你传统的 for 循环吧！我保证你的代码会越来越简洁，可读性更高。
