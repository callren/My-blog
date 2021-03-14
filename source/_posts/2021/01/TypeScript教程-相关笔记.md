---
title: JS程序员的TypeScript（简单版）
date: 2021-01-28 17:11:01
tags: TypeScript
categories: 教程 笔记 TypeScript
---

关于TS是什么以及为什么要用它就不做赘述了，直接正片开始！

# 前沿

TypeScript和JavaScript有着不同寻常的关系，JavaScript上的所有的功能，在TypeScript上都有，当然，TypeScript还多了一层类型校验。

就好比JavaScript中的原始类型像string、number或者object，但是它不会检查你是否是从一而终，比如定义了一个变量`let a = 1`，当你再写`a = true`的时候，也是没有任何问题，毕竟JavaScript是若类型语言。

当有了这种类型检查，那么你在代码中的一些简单的错误就不会再犯，就好比你的类型是数字但是却调用了字符串的方法。当然也会存在更友好的提示，再也不用担心你写错单词了。

下面就对TypeScript一些内容做一个简要的梳理

# 类型推断

TypeScript在大多数的情况下对你写的代码是有一个类型推断的，比如你声明了一个变量并且给它分配了一个特定的值，那么TypeScript就会使用这个值的类型作为这个变量的默认类型

```JavaScript
let TypeScript = 'FE情报局'
// let TypeScript: string
```

通过了解JavaScript的工作原理，TypeScript可以建立起一个类型系统，它可以接受你的JavaScript代码，但是在这个基础上它已经有了自己的类型。并且无需添加额外的代码，因此这就是上述的例子中`TypeScript`为何知道自己的类型就是`string`

作为一名前端，相信80%的人都在使用vs code，使用这个软件，在写TypeScript时候会让你更得心应手

# 定义Types

在JavaScript中我们可以使用多种设计模式，但是，一些设计模式会让自动的类型推断变的很困难，比如一些动态编程的模式。如果需要涵盖这些情况，TypeScript在JavaScript的基础上做了一些扩展，你可以告诉TypeScript这个地方应该是什么类型

例如，你创建了一个对象，包含`name: string`和`id: number`两个字断，你可以这样写：

```JavaScript
const user = {
    name: 'FE情报局',
    id: 0
}
```

你可以使用`interface`来很明确的描述这个对象：

```JavaScript
interface User {
    name: string;
    id: number;
}
```

然后你可以使用你新创建的`interface`，用`: TypeName`这种形式来声明你的对象内部类型结构

```JavaScript
const user: User = {
    name: 'FE情报局',
    id: 0
}
```

如果如果你的对象不能够匹配你所提供的`interface`，TypeScript会报出对应的错误

![interface错误](https://gitee.com/RenYaNan/wx-photo/raw/master/2021-1-28/1611828689572-iShot2021-01-28%2018.07.42.png)

自从JavaScript支持`class`关键字和面向对象编程，作为TypeScript，你可以对类使用`interface`进行类型声明

```JavaScript
interface User {
    name: string;
    id: number;
}

class UserAccount {
    name: string;
    id: number;
    
    constructor(name: string, id: number) {
        this.name = name;
        this.id = id;
    }
}

const user: User = new UserAccount('FE情报局', 1)
```

还可以使用`interface`来声明函数的参数以及函数的返回值

```JavaScript
function getAdminUser(): User {
    // ...
}

function deleteUser(user: User){
    //...
}
```

JavaScript中已经有一部分的原始类型你可以在`interface`中使用：`boolean bigint null number string symbol object undefined`。TypeScript在这基础上还扩展了一些，比如`any`（允许任何内容），`unknown`（确保使用该类型的人声明该类型是什么），`never`（这个类型不可能发生）`void`（函数return undefined或者没有返回值的时候）

你将会看到有两种声明类型的方式：`Interfaces` `Types`，你应该更倾向于`interfaces`，当你需要特定功能的时候使用`type`

# 组合类型Types

在TypeScript中，你可以在基本类型的基础上创建一个混合类型。有两种常用的方式：联合类型和泛型

## 联合类型

关于联合类型，你可以声明一种类型是多种类型中的一个。例如，你可以将一个`boolean`类型描述为`true`或者`false`

```JavaScript
type MyBool = true | false
```

> 在你本地测试这段代码的时候，将鼠标放上去，你会发现他的类型是`boolean`，这个是结构类型系统的属性

联合类型一个比较流行的事例是描述字符串或者数字的一个集合

```JavaScript
type WindowStates = 'open' | 'closed' | 'minimized'
type LockStates = 'locked' | 'unlocked'
type Numbers = 1 | 3 | 5 | 7 | 9
```

联合类型提供了一种不同于`types`的方法，例如，你在函数的参数中可以将其类型定为`array | string`

```JavaScript
function getLength(obj: string | string[]): number{
    return obj.length
}
```

你还可以在方法中根据不同的传参类型来返回不同的数据

```JavaScript
function wrapInArray(obj: string | string[]){
    if (typeof obj === 'string'){
        return [obj]
    } else {
        return obj
    }
}
```

## 泛型

泛型为类型提供变量。一个常见的例子是数组，没有泛型的数组可以包含任何类型，一个由泛型定义的数组可以通过泛型来描述数组内的内容

```JavaScript
type StringArray = Array<string>
type NumberArray = Array<number>
type ObjectWithNameArray = Array<{ name: string }>
```

你还可以声明你自己使用泛型的类型

```JavaScript
interface Backpack<Type> {
    add: (obj: Type) = void;
    get: () => Type
}

// 这一行是为了告诉TypeScript有一个常量名字叫backpack
declare const backpack: Backpack<string>

// object是字符串类型，因为我们定义了以上的可变部分
const object = backpack.get()

// 当backpack的add方法参数规定为字符串的时候，你不能够使用number，会引发对应的错误警告
// backpack.add(23) ⚠️
```

# 结构类型系统

一个TypeScript的核心原则是重点检查值的结构。也可以称为`duck typing`或者`structural typing`

在结构类型的系统中，如果两个对象拥有共同的结构，他们可以被定义为同一种类型

```JavaScript
interface Point {
    x: number;
    y: number;
}

function logPoint(p: Point) {
    console.log(`${p.x}, ${p.y}`)
}

const point = {x: 12, y: 26}
logPonit(point)
```

point变量永远不会声明为Point类型，但是，TypeScript在类型检查中会将point与Point的结构进行比较，他们有相同的结构，所以代码是OK的

结构匹配只需要同要匹配的结构的子集匹配即可

```JavaScript
const point3 = {x: 12, y: 26, z: 89}
logPoint(point3) //OK

const rect = {x: 33, y: 3, width: 100, height: 200}
logPoint(rect) //OK

const color = {hex: '#000'}
logPoint(color) //fail
```

类和对象匹配的格式没有区别

```JavaScript
class VirtualPoint {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x
        this.y = y
    }
}
const newVPoint = new VirtualPoint(13, 56)
logPoint(newVPoint) // logs "13, 56"
```

如果对象或类具有所有必须的属性，则TypeScript会认为它们匹配
