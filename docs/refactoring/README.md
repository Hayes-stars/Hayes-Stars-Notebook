# JavaScript重构方法

- 《重构2》JavaScript重构方法归纳，改善既有代码的设计
- 古人有云：“时时勤拂试，勿使惹尘埃”。代码当如是，专业人士的技艺亦当如是。

> 当今软件开发的速度越来越快，带来的技术债也越来越多，在开发过程中充分认识到重构的重要性————如果我们程序员能理解和掌握重构的原则和方法，我们的系统就不会有那么多沉重的债务。

## 何谓重构

- 对软件内部结构的一种调整，目的是在不改变软件可观察行为的前提下，提高其可理解性，降低其修改成本。
- 使用一系列重构手法，在不改变软件可观察行为的前提下，调整其结构。
- 如果代码在重构过程中有一两天时间不可用，基本上可以确定，这不是重构。
- 重构的唯一目的就是让我们开发更快，用更少的工作量创造更大的价值。

## 为何重构

- 重构改进软件的设计；
- 重构使软件更容易理解；
- 重构帮助找到bug；
- 重构提高编程速度。

## 何时重构

**三次法则**

- 第一次做某件事时只管去做；
- 第二次做类似的事会产生反感，但无论如何还是可以去做；
- 第三次再做类似的事，你就应该重构。俗话说的好"事不过三，三则重构"。

**让添加新功能更容易**

- 重构的最佳时机就在添加新功能之前。

**使代码更易懂**

- 能不能重构这段代码，令其一目了然？

**捡垃圾式的重构**

- 如果每次查看字段代码时都把它变好一点点，积少成多，垃圾总会被处理干净。

**有机会的重构和见机行事的重构**

- 权衡取舍计划重构如：参数化需要做到什么程度？函数之间的边界应该划在哪里？
- 肮脏的代码必须重构，但漂亮的代码也需要很多重构。

**大型重构**

- 替换正在使用的库，或者整块代码抽取到一个组件中共享，再或者要处理一大堆混乱的依赖关系等等。

**代码评审时重构**

- 在代码评审过程中获得的很多有用的建议。

**何时不应该重构**
- 如果重写比重构还容易，就别重构了。当然这个也是比较困难的一个决定，需要花时间去尝试。
## 测试

> 重构风险大，可能会引入bug，要有良好的测试过程。

## 重构方法名录

### 1 函数&变量(Function & Variable)

1. [提炼函数(Extract Function)](refactoring/first/extract-function)
2. [内联函数(Inline Function)](refactoring/first/inline-functions)
3. [提炼变量(Extract Variable)](refactoring/first/extract-variable)
4. [内联变量(Inline Variable)](refactoring/first/inline-variable)
5. [改变函数声明(Change Function Declaration)](refactoring/first/change-func-delcare)
6. [封装变量(Encapsulate Variable)](refactoring/first/encapsulate-variable)
7. [变量改名(Rename Variable)](refactoring/first/rename-variable)
8. [引入参数对象(Introduce Parameter Object)](refactoring/first/introduce-params-obj)
9. [函数组合成类(Combine Function into Class)](refactoring/first/combine-func-class)
10. [函数组合成变换(Combine Functions into Transform)](refactoring/first/combine-func-into-transform)
11. [拆分阶段(Split Phase)](refactoring/first/split-phase)

### 2 封装(Encapsulate)

1. [封装记录(Encapsulate Record)](refactoring/encapsulate/encapsulate-record)
2. [封装集合(Encapsulate Collection)](refactoring/encapsulate/encapsulate-collection)
3. [以对象取代基本类型(Replace Primitive with Object)](refactoring/encapsulate/replace-obj-base-type)
4. [以查询取代临时变量(Replace Temp with Query)](refactoring/encapsulate/replace-temp-query)
5. [提炼类(Extract Class)](refactoring/encapsulate/extract-class)
6. [内联类(Inline Class)](refactoring/encapsulate/inline-class)
7. [隐藏委托关系(Hide Delegate)](refactoring/encapsulate/hide-delegate)
8. [移除中间人(Remove Middle Man)](refactoring/encapsulate/remove-middle-man)
9. [替换算法(Substitute Algorithm)](refactoring/encapsulate/substitute-aligorithm)

### 3 搬移特性(Move Character)

1. [搬移函数(Move Function)](refactoring/move/move-function)
2. [搬移字段(Move Field)](refactoring/move/move-field)
3. [搬移语句到函数(Move Statements into Function)](refactoring/move/move-statement-func)
4. [搬移语句到调用者(Move Statements to Callers)](refactoring/move/move-statements-callers)
5. [以函数调用取代内联代码(Replace Inline Code with Function Call)](refactoring/move/func-replace-inline-code)
6. [移动语句(Slide Statements)](refactoring/move/slide-statements)
7. [拆分循环(Split Loop)](refactoring/move/split-loop)
8. [以管道取代循环(Replace Loop with Pipeline)](refactoring/move/replace-loop-for)
9. [移除死代码(Remove Dead Code)](refactoring/move/remove-dead-code)

### 4 重新组织数据

1. [拆分变量(Split Variable)](refactoring/reorganizing-data/split-variable)
2. [字段改名(Rename Field)](refactoring/reorganizing-data/)
3. [以查询取代派生变量(Replace Derived Variable with Query)](refactoring/reorganizing-data/replace-variable-query)
4. [将引用对象改为值对象(Change Reference to Value)](refactoring/reorganizing-data/change-ref-value)
5. [将值对象改为引用对象(Change Value to Reference)](refactoring/reorganizing-data/change-value-ref)

### 5 简化条件逻辑

1. [分解条件表达式(Decompose Conditional)](refactoring/conditional/decompose)
2. [合并条件表达式(Consolidate Conditional Expression)](refactoring/conditional/consolidate)
3. [以卫语句取代嵌套条件表达式(Replace Nested Conditional with Guard Clauses)](refactoring/conditional/guard-clauses)
4. [以多态取代条件表达式(Replace COnditional with Polymorphism)](refactoring/conditional/polymorphism)
5. [引入特例(Introduce Special Case)](refactoring/conditional/introduce-spec-case)
6. [引入断言(Introduce Assertion)](refactoring/conditional/introduce-spec-case)

### 6 重构API

1. [将查询函数和修改函数分离(Separate Query from Modifier)](refactoring/api/separate-query-modify)
2. [函数参数化(Parameterize Function)](refactoring/api/param-function)
3. [移除标记参数(Remove Flag Argument)](refactoring/api/remove-flag-arg)
4. [保持对象完整(Preserve Whole Object)](refactoring/api/preserve-obj)
5. [以查询取代参数(Replace Parameter with Query)](refactoring/api/replace-query-param)
6. [以参数取代查询(Replace Query with Parameter)](refactoring/api/replace-param-query)
7. [移除设值函数(Remove Setting Method)](refactoring/api/remove-set-method)
8. [以工厂函数取代构造函数(Replace Constructor with Factory Function)](refactoring/api/replace-factory-func)
9. [以命令取代函数(Replace Function with Command)](refactoring/api/replace-command-func)
10. [以函数取代命令(Replace Command with Function)](refactoring/api/replace-func-command)

### 7 处理继承关系

1. [函数上移(Pull Up Method)](refactoring/extends/pull-up-method)
2. [字段上移(Pull UP Field)](refactoring/extends/pull-up-field)
3. [构造函数本体上移(Pull up Constructor Body)](refactoring/extends/pull-up-constructor)
4. [函数下移(Push Down Method)](refactoring/extends/push-down-method)
5. [字段下移(Push Down Field)](refactoring/extends/push-down-field)
6. [以子类取代类型码(Replace Type Code with Subclasses)](refactoring/extends/replace-subclass-tc)
7. [移除子类(Remove Subclass)](refactoring/extends/remove-subcalss)
8. [提炼超类(Extract Superclass)](refactoring/extends/extract-superclass)
9. [折叠继承体系(Collapse Hierarchy)](refactoring/extends/collapse-hierarchy)
10. [以委托取代子类(Replace Subclass with Delegate)](refactoring/extends/replace-subclass-delegate)
11. [以委托取代超类(Replace Superclass with Delegate)](refactoring/extends/replace-superclass-delegate)
