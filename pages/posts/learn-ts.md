---
title: TypeScript
date: 2021/12/16 12:21:00
description: TypeScript是现代前端开发者必须掌握强化技能之一
tag: TypeScript
author: Mowtwo
---

# TypeScript

> 编写者：Mowtwo

TypeScript 是现代前端开发者必须掌握强化技能之一

## 简介

### TypeScript 的作者

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211105542506.png" alt="image-20211211105542506" style="zoom:50%;" />

> Anders Hejlsberg，计算机科学家，现在是微软.Net 的首席架构师，同时也是 Delphi（基于 Pascal 的可视化 GUI 开发工具），C#之父。

### 什么是 TypeScript

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211105702149.png" alt="image-20211211105702149" style="zoom: 67%;" />

> Typescript 是有类型的 JavaScript，同时它也是一门完全独立的编程语言。
> 它拥有强大的类型系统，可以完美的兼容 JavaScript，同时它可以帮助你弥补很多 JavaScript 欠缺的东西。

## 使用 TypeScript

在真实项目中，TypeScript 往往是作为项目的一部分存在，而现代化的项目脚手架都会帮忙配置好 TypeScript 的使用环境，但是有时候我们也需要配置一些基础的环境来完成开发工作。

### 安装编译器

```bash
npm install -g typescript
```

安装完成后，npm link 会自动在我们的环境变量中释放一个`tsc`命令，我们可以通过`tsc`命令来查看安装情况

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211110503805.png" alt="image-20211211110503805" style="zoom: 67%;" />

其中`tsc`的版本号就代表了我们当前语言能支持的最高语法版本（LTS），不同版本的语法会带来很多新的特性，包括对于 ECMAScript 新规范的支持，强化的 TypeScript 特性，更强大的类型工具等等，这些东西都会持续的在 TypeScript 中被不断添加

### 查看帮助

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211110800977.png" alt="image-20211211110800977" style="zoom:67%;" />

`tsc`本身也是一个强大的 cli 工具，提供了基于命令行的快捷指令，通过不同的参数可以快速使用对应的功能

### 创建新的项目

一个 TypeScript 的最小项目可以很简单，不需要复杂的配置，只需要在某个文件夹下，创建一个`.ts`文件就可以，不过散乱的源码文件会让项目管理最终变得无法完成，任何工作都应该从规范开始。

规范本身只是提供一个约定俗成的作用，但是如果讲约定俗成的东西形成习惯，那么有时候它也可以成为规则，所以 TypeScript 对于一个完整的**工程**本身也定下了*规则*。

不过在创建 TypeScript 项目之前要注意，TypeScript 大部分情况下，是依赖于 npm 进行包管理的，所以我们也应当给 TypeScript 项目先进行一次 npm 的初始化。

```bash
mkdir test-demo && cd test-demo # 场景并进入目录
npm init -y # 初始化package.json
```

此时查看路径，我们就可以看到一个简单项目结构

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211112818940.png" alt="image-20211211112818940" style="zoom:67%;" />

### 初始化配置

在 TypeScript 中，用来形成约束规则的存在，即为一个配置文件`tsconfig.json`，这个文件本身其实由来已久，它并不是单纯出自于 TypeScript 本身，而是微软对于自家产品的联合应用。

前端常用的编辑器`VSCode`，在早期就是专门设计出来给 JavaScript 开发者使用的，所以内置了很多针对 JavaScript 的功能，但是 JavaScript 由于其特有的灵活性，如果将功能一口气全部集成或者应用，会让编辑器本身变得臃肿，并且性能低下（虽然本身性能就不算很好）。所以，模块化的配置与插件化的功能解耦出现了，针对于 JavaScript 天生对`JSON`的亲和力，`JSON`成为了`VSCode`所有配置的第一选择。

当你在 VSCode 中创建了一个名为`jsconfig.json`的文件时，这个文件就会自动对你项目中的 JavaScript 生效，它可以帮助你指定 VSCode 的代码提示范围，JavaScript 应用的 ECMAScript 版本，以及对应的模块化（AMD，CMD，UMD，ESM 等等）类型等等。

对应的在 TypeScript 中，`tsconfig.json`也充当起了对应的角色，但是它更加与众不同，因为它不但作为配置存在，它还是 tsc 本身用来约束项目本身的存在。也就是说，当你在项目的根目录下创建了这个文件，在 tsc 被调用并且没有指定配置入口时，它会去主动读取默认的这个`tsconfig.json`.

tsc 内置了对一些通用配置的生成，你只需要在你的项目根路径上开启一个终端（Terminal，Windows 上指的是 CMD/PowerShell/Windows Terminal，\*nix 系统则为各类 Shell）便可创建

```bash
tsc -init
```

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211112859453.png" alt="image-20211211112859453" style="zoom:67%;" />

我们可以通过编辑器查看生成出来的`tsconfig.json`，发现里面有很多被注释起来的配置项，这些其实暂时都是用不到的，所以我们对配置进行一些精简，最终可以得到一个空配置。

```json
{
  "compilerOptions": {
    "target": "es5", // 指定编译目标版本
    "module": "ES6", // 指定编译后的模块系统类型
    "esModuleInterop": true, // 是否兼容cmd的export
    "forceConsistentCasingInFileNames": true, // 是否区分模块文件名的大小写
    "strict": true, // 是否开启强类型严格检查
    "skipLibCheck": true // 跳过对.d.ts文件的检查
  }
}
```

可以看到这里的 JSON 实际上并不是传统意义上的标准 JSON，而是支持注释并且更灵活的一种 JSON 实现，在 VSCode 一般就是给编辑器配置使用。

不过最小配置并不能完全满足开发的使用，因为如果使用这种配置，那么最终编译出来的文件是会跟源文件放在相同的位置，这样依然不利于项目管理，所以我们可以添加一个配置实现这个功能

```json
{
  "compilerOptions": {
    "target": "es5", // 指定编译目标版本
    "module": "ES6", // 指定编译后的模块系统类型
    "esModuleInterop": true, // 是否兼容cmd的export
    "forceConsistentCasingInFileNames": true, // 是否区分模块文件名的大小写
    "strict": true, // 是否开启强类型严格检查
    "skipLibCheck": true, // 跳过对.d.ts文件的检查
    "outDir": "./dist"
  }
}
```

然后我们按照习惯，将所有的源文件所在的路径设定为`./src`，所以我们还需要创建一个`./src`目录，并且在里面放入一个`index.ts`。

### 运行项目

在`index.ts`写入简单的代码后，我们就可以尝试编译了。

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211134801627.png" alt="image-20211211134801627" style="zoom:67%;" />

```typescript
// ./src/index.ts
interface Greet {
  (a: string): string
}

const greet: Greet = (a: string) => `hello ${a}`

console.log(greet('world'))
```

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211135053877.png" alt="image-20211211135053877" style="zoom:67%;" />

## 类型系统

在上面的最小程序创建的时候，我们应用到了一些 TypeScript 的基础知识，但是其实可以看出跟 JavaScript 最不一样的地方，应该就是关于类型的描述。在 TypeScript 的官方定义上，TypeScript 的最初的本质目的就是为了解决 JavaScript 动态类型带来的一些问题，所以 TypeScript 是一门静态类型的编程语言，虽然它的静态类型的特性以及对于类型的检查仅仅停留在编译期，但是已经在很大程度上产生了作用。

但是 TypeScript 又不同于更传统一些的静态类型语言，它在产生的过程中，就已经吸收了很多来自于不同静态语言的特点，并且对它们的优势进行了过滤，特别是从 C#中吸收了大量了特性（毕竟是同一个作者），C#作为微软推出的一门大杀器，在语法糖跟语言特性方面推进的一直都是十分大胆。但是抛去 C#这门的影响，其实 JavaScript 本身的**函数是一等公民\***（First-class Function）的特性，又从各类 ocaml 语言中吸收的很多新特性，提供了很多帮助函数式编程的功能。

不过在了解高层次的类型系统应用之前，必须了解 TypeScript 的基础类型系统，这里面包含了很多关于 JavaScript 兼容性的使用，以及 TypeScript 如何处理特有的类型系统与 JavaScript 已有的类型系统的转行过程，这些过程往往是在底层做了一些 builtin 处理，或者更加 hacker 的做法，所以在使用上不存在困难，反而应该是非常平滑的。

### 鸭子类型

> 当你看到一个对象，它看起来像鸭子，吃起来像鸭子，走路也像鸭子，那你就可以把它当成鸭子

TS 的类型系统不同于场景的标明类型系统（Nominal Type System），而是更加灵活的结构类型系统（Structural Type System）。这种类型并不会将类型的名字写入签名作为类型检查的根据，而仅仅比较类型本身的结构组成，既支持鸭子类型（Duck Type）。

### 基础数据类型

> 这部分只描述数据类型，也就是所谓的字面量类型，在 JS 中也有关于字面量类型跟包装类型的差别，但是 TS 将这部分差别在表面上已经体现出来
>
> 在 TS 中，字面量类型应该是小写的，而大写的类型代表了其对应的包装类型的构造函数，这样就会产生很大的不同，因为包装类型对象本身是引用类型对象（Reference），在内存存储结构上是不同的

#### number

TypeScript 的数值类型跟 JS 的保持一致，其底层就是双精度浮点类型，正常情况下是 64 位长度（double float）

#### string

与 JS 保持一致

#### boolean

与 JS 保持一致

#### null

与 JS 保持一致，不过由于在 JS 里设计的特殊性，在 TS 的类型系统里，为它增加了特殊的 hack，使其是任意引用类型的`subtype`

#### undefined

与 JS 保持一致，有点类似 null 的 hack，但是比 null 更特殊，本身即是值，又是自己的类型

#### void

这个是区别于 JS 的类型，它的作用更接近 C 语言的 void，但是又像是 ocaml 语言中的 unit，代表了本身并不关心返回值类型，一般用于函数的返回类型的标注，与 JS 中的 void 关键字区分，因为 JS 的 void 关键字是为了产生真正的 Undefined 来使用的（JS 的**u**ndefined 本身并不是一个关键字，相反它可以被覆盖，所以底层还有一个真正的 Undefined）。

#### never

TS 特有类型，是一个辅助类型，一般用于函数返回，本身代表了不关心值的类型，并且这个值永远不会返回有意义的值

#### any/unknown

> any 是 TS 留给不熟悉 TS 类型的开发者的一个 hack 通道，使用 any 是一个非常危险的事情，会导致类型丢失，这些是无法预测的安全隐患，请不要在项目里大量使用 any，如果可以，请使用 unknown 代替

any 代表了它不关心变量的类型，也不关系类型本身的存储方式（引用或者值类型），被标记上 any 的变量可以像 JS 里的变量一样使用，而且编译器将 any 做了特殊的 hack，使其即是任意类型的 super type，也是任意类型的 sub type。

unknown 是 TS 在 3.0 版本被引入的特殊类型，它被理解为类型安全版本的 any，但是它其实在行为上并不等同于 any。在使用它时，处理所标记的变量需要手动检查类型转换问题，与 any 最大的区别就是：any 放弃了类型检查，让程序自身自灭，而 unknown 是让编译器信任开发者，让开发者处理好类型检查的事情。

### 集合数据类型

> 其实所谓的集合数据类型在官方分类中，也是数据基础类型，但是实际上在表达上应该被区分

#### object

与 JS 保持一致，但是这个类型其实挺危险的，使用上应当参考 any 的危险性

JS 的 object 在底层实现上实际上是一个特殊的 hashmap，而 hashmap 本身就代表了不确定性，而在 TS 中，被标记了 object 类型的变量也会丢失很大一部分的类型检查，比起 any 仅仅只是少了一些空指针异常的风险，可是在访问属性的时候，依然存在空属性异常的风险。

#### array

TS 的数组本质与 JS 保持一致，但是对于类型安全上的一些改造，让 array 变得更值得使用。（虽然在 JS 中，array 只是特殊的 object）

TS 的 array 支持在声明的时候，通过泛型，或者特定的结构语法进行声明，可以在声明时就确定集合的类型范围，这样对于数据子项的类型安全是有利的。

并且 TS 支持类型推导，还有联合类型，让数组声明本身变得没那么麻烦。

```typescript
const arr1 = [1] // number[]
const arr2 = [1, 'a'] // (number|string)[]
```

#### tuple

TS 提供了一种相对静态的数组类型，那就是元组，声明元组跟声明数组很像，并且二者在底层上都是一样的 JS 数组，不过元组可以让我们将类型收缩的更小。

```typescript
const arr1: [number] = [1]
const arr2: [number, string] = [1, 'a']
```

#### enum

TS 的枚举类型，实际上是对对象的一种特殊封装，并且 TS 的枚举在形式上，存在两个形态。也就是在两方面描述了同一个东西，因为不同于其他高级类型的声明，在编译时，enum 不会像他们一样被抹除，而是更接近`class`的行为，被保留下一份值映射的副本。

```typescript
// 声明的枚举
enum Result {
  OK,
  Err
}
```

```javascript
// 编译后的枚举
var Result
;(function (Result) {
  Result[(Result['OK'] = 0)] = 'OK'
  Result[(Result['Err'] = 1)] = 'Err'
})(Result || (Result = {}))
```

> 不过由于枚举本身的特殊性，不是很推荐在项目中使用大量的枚举，而是应该使用静态枚举，或者直接使用字符串**联合类型**（Union Types），这样可以减少很多不必要的样板代码的生成，也可以减少外部调用者对代码的疑惑性

```typescript
// 声明的枚举
const enum Result {
  OK,
  Err
}
const a: Result = Result.OK
```

```javascript
// 编译后的枚举不在保留，而是直接替换成对应的值
var a = 0 /* OK */
```

### 类型关键字

> 其实上面的的 enum 在某种意义上也算是一种特殊的类型关键字，但是由于其行为更接近一个集合，所以不放在这里

在 TS 里，还可以通过一些特定的关键字来创建一些额外的类型，这些类型可能是类型别名（alias），接口（interface），或者类类型（Classes），这些都是合法的类型声明方式，并且都可以在任意合适的场合被使用，因为这些类型可以保证你的项目保持一个相对的功能健壮性，并且可以帮助编译器在输出代码时完成合理的类型安全检查。

不同的关键字，有时候可以输出相同的结果，有时候却会在行为上产生大相径庭的结果。

#### class

这个关键字其实也是 JS 在 ECMAScript2015（即 ES6）中引入的一个特殊的语法关键字，在 JS 中，其实它就是构造函数的一个语法糖，并且也提供了对应的 OOP 的简易实现，但是 JS 中的 class 还是有很大的缺陷，在很多场景下无法担起重要的任务。比如 React 跟 Vue 都在慢慢移出 class 关键字，改用 function 的方式来渲染组件，因为在软件工程的设计中，其实一直都是参照着一种方式，即为数据驱动 UI 的设计理念。

```
UI = Model(Data,Event)
```

不过 TS 在设计的早期，由于 C#的影响，所以依然对 class 进行了大量的完善和改造，这让 TS 的 class 关键字变得能够使用，并且值得被大量使用，特别是自己进行一些设计模式的实践时。

_传统 UI 开发其实还曾经遵循过 MVC，而 OOP 是 MVC 实现的一个关键。_

```typescript
class Person {
  constructor(private firstName: string, private lastName: string) {}
  getFullName() {
    return this.firstName + this.lastName
  }
}

class OfficeWorker extends Person {
  constructor(
    firstName: string,
    lastName: string,
    public readonly wkNo: string
  ) {
    super(firstName, lastName) // 必须调用super关键字，否则无法在上下文使用this关键字
  }
  getWorkerInfo() {
    return `Info(fullName=${this.getFullName()},workerNo=${this.wkNo})` // 这里的getFullName继承自Person
  }
}

const wk = new OfficeWorker('陈', '文程', '80064')
console.log(wk.getWorkerInfo()) // 输出:Info(fullName=陈文程,workerNo=80064)
```

关于 class 的应用更多的在 OOP 层次上的应用，因此要等到 OOP 讲解时，才能扩展更多的内容。

#### interface

interface 就是让 TS 完全区别于 JS 的一个重要特性之一，基于 interface，TS 能够实现的东西将会完全超出普通的静态类型编程语言的范畴，并且能够使其应用高度深入到计算机科学的领域。

> 但是实际上还需要提前感谢 type 关键字与泛型，interface 跟 type 的结合，让 TS 没有沦为一个 C# like in js，type 让 TS 更完整，更强大。而泛型是一门强大的语言必备的一个特性，它直接让 TS 的类型系统本身实现了图灵完备（Turing completeness）的特性
>
> 在技术领域，甚至产生了专门研究 TS 类型体操（**type gymnastics**）的开源组织
>
> 甚至有人用 TS 的类型系统实现了另一门语言（大名鼎鼎的 Lisp）的解释器

使用 interface 可以规定一个对象的实现类型，以及属性约束，类型扩展（这个比较特殊）等功能。

```typescript
interface Person {
  getFullName: () => string
}

interface OfficeWorker {
  wkNo: string
  getWkInfo: () => string
}

const wk: Person & OfficeWorker = {
  wkNo: '80064',
  getFullName() {
    return 'mowtwo'
  },
  getWkInfo() {
    return `Info(fullName=${this.getFullName()},workerNo=${this.wkNo})`
  }
}

console.log(wk.getWkInfo()) // 输出:Info(fullName=mowtwo,workerNo=80064)
```

#### type

type 关键字是一个功能非常多的关键字，它在某些场合是完全可以代替 interface 来使用。但是 type 能做的远不只是声明接口那么简单，它还可以给类型声明别名，声明联合类型（Union Types）与交叉类型(Intersection Types)，这些给 TS 的类型系统带来了很大的灵活性。

```typescript
type SystemString = string // 创建基础数据类型别名

interface Person {
  getFullName: () => string
}

type AnyPerson = Person[] // 将数组声明为单独的类型

type Result = 'Ok' | 'Err' // 联合类型
```

## 面向对象

> TS 的 OOP 本身是一个很大的使用课题，OOP 本身也带来了很多便利，但是 OOP 也有很明显的缺陷，使用 OOP 前需要评估好自己对于软件工程的理解水平，设计模式的使用才是 OOP 的精髓所在

### 继承与多态

在上面 class 的例子中，其实就已经展示了 TS 的 class 在继承方面的应用，但是实际上，OOP 的继承最终目的就是为了实现多态的应用，不过由于 JS 对于 class 本身实现的缺陷，JS 的多态非常有局限性，但是 TS 相对弥补了其中的缺陷，让多态实现上会更加流畅。

> **多态**即为，一个统一接口（interface/protocal）下的多个实例物体的实现不同，但是可以被传递到同一个输入中。

```typescript
abstract class Animal {
  constructor(private typeName: string) {}
  getTypeName() {
    return this.typeName
  }
  setTypeName(typeName: string) {
    this.typeName = typeName
  }
  abstract callSound(): void
}

interface Fly {
  fly: () => void
}
interface Run {
  run: () => void
}

class Cat extends Animal implements Run {
  constructor() {
    super('Cat')
  }
  callSound() {
    console.log('mew mew')
  }
  run() {
    console.log(`${this.getTypeName()} can run`)
  }
}

class Bird extends Animal implements Fly {
  constructor() {
    super('bird')
  }
  callSound() {
    console.log('jo jo')
  }
  fly() {
    console.log(`${this.getTypeName()} can fly`)
  }
}

class AirPlane implements Fly {
  constructor() {}
  fly() {
    console.log(`AirPlane can fly`)
  }
}

function zooListen(ani: Animal) {
  ani.callSound()
}

function skyHave(fly: Fly) {
  fly.fly()
}

const cat = new Cat()
const bird = new Bird()
const airPlane = new AirPlane()

zooListen(cat) // 输出:mew mew
zooListen(bird) // 输出:jo jo
skyHave(bird) // 输出:bird can fly
skyHave(airPlane) // 输出:AirPlane can fly
```

### 重写与重载

由于 OOP 中，子类有很多来自于父类或者更高的基类继承的方法和属性，但是有些时候基类提供的这些实现并不是我们所想的，或者基类是抽象类的时候，我们必须去手动实现对应的方法或者属性时，就会用应用到**重写**（Override），在上面的例子已经表现出对于抽象类的实现重写。

```typescript
class Person {
  constructor() {}
  walk() {
    console.log('walking')
  }
  getType() {
    return 'person'
  }
}
class Student extends Person {
  constructor() {
    super()
  }
  walk() {
    // 完全重写掉父类的walk
    console.log('go to school')
  }
  getType() {
    // 通过super关键字可以访问重写前的函数
    return super.getType() + ' and student'
  }
}
const stu = new Student()
stu.walk() // 输出:go to school
console.log(stu.getType()) // 输出:person and student
```

重写在 JS 中其实就已经可以实现，但是重载则是 TS 完全不同于 JS 的实现，JS 并不支持重载。

> TS 的重载不只是应用于 OOP 中，而是对任何函数都会产生效果，但是实际上并不推荐在 TS 里大量使用重载，而是应该使用可变参数列表，可选参数，命名参数的方式增加扩展性，过多使用重载其实会产生一些不必要的歧义与学习成本。

重载（Overload），代表的是对于同一个函数名的不同实现，而其判断的标准则是参数列表中的类型签名的不同，在实际应用中，TS 的重载其实也不算很好的实现，当然，这些是为了兼容 JS 付出的代价。

```typescript
class A {
  walk(n: number): string
  walk(s: string): string
  walk(a: number | string): string {
    // 再具体实现时必须把所有参数类型的可能都判断出来
    if (typeof a === 'string') {
      return `walk in ${a}`
    } else {
      return `walk ${a} metre`
    }
  }
}

const a = new A()
console.log(a.walk(300)) // 输出:walk 300 mile
console.log(a.walk('集美大桥')) // 输出:walk in 集美大桥
```

## 泛型

**Generic**是静态类型语言里非常重要的特性，TS 的泛型更是直接让 TS 本身的强大上升了一个等级，使其脱离了普通的静态类型编程语言的范畴。

虽然泛型在定义上是非常简单的，即为可变的类型参数。

由于静态类型编程语言的类型限制，有时候我们需要实现一些通用功能函数的时候，需要编写大量的样板代码，或者重复性的类型区分声明。（虽然在 TS 里有 any，unknown，重载等等各种替代方案，但是都好丑，并且 TS 实际上对于 number 类型的宽泛定义，并不会有太大的烦恼）

## 在 class 中使用

在经典泛型使用案例中，class 的使用是比较早的，并且具有较大的相同，因为具有 class 的 OOP 类语言大都是在 class 内实现作用域共享。而这种共享，一般都是伴随着封装与设计所在。

TypeScript 的 class 虽然在延续了 JavaScript 的大部分特性，但是在使用上，也对泛型做了一定程度上的强化支持。

```typescript
// 单链表
class Linked<T> {
  constructor(private _value: T, private next?: Linked<T>) {}
  at(index: number): [lastIndex: number, target: Linked<T>] {
    let _cur: Linked<T> = this
    if (!_cur.next) {
      return [0, _cur]
    }
    for (let i = 1; i <= index; i++) {
      if (!_cur.next) {
        return [i, _cur]
      }
      _cur = _cur.next
    }
    return [index, _cur]
  }
  getValue(at: number) {
    return this.at(at)[1]._value
  }
  last() {
    let _cur: Linked<T> = this
    while (true) {
      if (!_cur.next) {
        return _cur
      }
      _cur = _cur.next
    }
  }
  append(l: Linked<T>) {
    this.last().next = l
  }
  insertAfter(at: number, value: T) {
    const cl = new Linked(value)
    const [i, l] = this.at(at)
    if (i === at) {
      const nl = l.next
      l.next = cl
      cl.next = nl
      return true
    } else {
      return false
    }
  }
  insertBefore(at: number, value: T) {
    const cl = new Linked(value)
    const [i, l] = this.at(at)
    if (i === at) {
      const [_, ll] = this.at(at - 1)
      ll.next = cl
      cl.next = l
      return true
    } else {
      return false
    }
  }
  remove(at: number) {
    if (at === 0) {
      const nl = this.next
      if (nl) {
        const nnl = nl.next
        nl.next = null
        this._value = nl._value
        this.next = nnl
        return true
      } else {
        return false
      }
    } else {
      const [i, rl] = this.at(at)
      if (i === at) {
        const [i, lrl] = this.at(at - 1)
        lrl.next = rl.next
        rl.next = null
        return true
      } else {
        return false
      }
    }
  }
  toArray(ret: Array<T> = []): Array<T> {
    ret.push(this._value)
    if (this.next) {
      return this.next.toArray(ret)
    } else {
      return ret
    }
  }
}
```

## 在 interface 中使用

泛型本身就是为了能够类型编程中做到相对宽松的类型限制的作用，所以在 interface 里也是可以生效的，并且与泛型配合的 interface 会变远比普通 interface 更加强大。

> 泛型最终要在实际场景里被约束，并且完善的约束可以让泛型真正做到宽松的同时保证了代码的安全

```typescript
type VersionString = `${number}.${number}.${number}`

interface Repo<T> {
  name: string
  version: VersionString
  content: T
}

// 有时候泛型可以作为其他函数的泛型持续的传递下去
interface Fork<T> {
  name: string
  withRepo: Repo<T>
}
```

## 在 type 中使用

type 中的泛型行为上与 interface 类型，但是实际上可以提供更强大的类型元编程能力，但是这些是需要对类型系统具备一定程度认知后才能进行的。

> 带约束的泛型就是通过 extends 关键字对类型的特征进行判别

```typescript
type Action<T> = ForkAction<T> | CloneRepo<T>
```

## 在 function 中使用

无论是早期是出于对 JavaScript 的 function 可以作为构造函数的兼容，还是有意实现函数式编程语言特有的泛型系统，TypeScript 都为单独的函数也添加了泛型的支持，但是实际上这项添加是非常有用的。

目前 JavaScript 有大量的函数式语法，TypeScript 在实现对它们的类型标准时，也会用到泛型，这些都是来自由动态类型语言转换到静态类型语言的必要消耗。

当你在纯函数（Pure Function）中使用泛型时，TypeScript 的自动推导系统可以节省你很多编写代码的时间，这些特性是从 OCaml 类语言中继承来的特性。不过这些也仅仅只是在功能上的相似，与 OCaml 语言相比，TypeScript 并没有做到它们那么纯粹的推导，因为 OCaml 语言的推导来自于上下文，而 TypeScript 则完全结合它特有的类型系统系统的推导能力。

> 在 function 内支持的泛型，在某些程度上可以完全被 class 的方法（method，其实也是 function 的一种）完全支持，但是由于 FP 跟 OOP 在设计上的理念不同，在实际应用中二者会在编写时产生一些差异

```typescript
type VersionString = `${number}.${number}.${number}`

interface Repo<T> {
  name: string
  version: VersionString
  content: T
}
interface Fork<T> {
  name: string
  withRepo: Repo<T>
}

interface ForkAction<T> {
  type: 'fork'
  fork(repo: Repo<T>): Fork<T>
}

interface CloneRepo<T> {
  type: 'clone'
  clone(repo: Repo<T>): Repo<T>
}

type Action<T> = ForkAction<T> | CloneRepo<T>

function runActions<T>(repo: Repo<T>, ...actions: Action<T>[]) {
  for (const action of actions) {
    if (action.type === 'fork') {
      console.log(action.fork(repo))
    } else {
      console.log(action.clone(repo))
    }
  }
}

runActions(
  {
    name: 'demo',
    version: '1.0.0',
    content: 'Hello World'
  },
  {
    type: 'fork',
    fork(repo) {
      return {
        name: 'fork1',
        withRepo: repo
      }
    }
  }
)
```

## 泛型与 any/unkown 的区别

对部分对泛型的应用场景还不熟悉的人，大概会对泛型产生一些误解，认为泛型仅仅只是在抹去类型的限制，其实正相反，泛型就是在保护类型的安全，不过它提供了一条帮助我们控制类型判定的后门，是我们与编译器直接交互的一个便捷通道。

> 这些编程语言在语法层面提供的可以控制编译器/运行时行为的语法都统称为元编程，最常见的支持元编程的语言包括 Ruby/PHP/JavaScript

any 是**放弃类型检查**，所以在任何场景下都不要使用 any，如果一定要使用，要避免在 any 相关的变量下做复杂操作与对外释放的封装调用。

unknown 仅仅只是提供了类型的安全转换，更多是在编写代码的开发者能够很好的控制住开发场景的情况下使用，也不推荐完全无脑使用。**任何没有经过设计的业务代码都不应该合理的存在**。

## 使用 extends

上面有提到泛型本质是为了能够让我们更好的去约束类型，而实现这一部的关键，就是**extends**关键字。

> **类型空间**（后面会讲解）下的 extends 并不是语言层面的 extends，这个后在后面详细讲解，与之类似的关键字还有 typeof，class，enum

### 使泛型必须是某个类型的实现

```typescript
// 下面的代码由于ts对js的兼容性，它可以被执行成功
// 但是请注意，这是不合理的，因为它的返回值不再安全，而且不同类型的变量不用被纯粹的比较，要警惕所有的隐式类型转换
function max(a: unknown, ...values: unknown[]) {
  for (const n of values) {
    if (a < n) {
      a = n
    }
  }
  return a
}
console.log(max('1', false, 3, {})) // 3
```

安全的实现版本

```typescript
// 这里无需显式的写上类型T也是可以推断出来的
// 注意，ts的类型推断会尽量往最小的范围收缩，这里在高版本的ts里推断出来的类型应该是 1|2|3|4
function max<T extends number>(a: T, ...values: T[]): T {
  for (const n of values) {
    if (a < n) {
      a = n
    }
  }
  return a
}
console.log(max(1, 2, 3, 4)) // 4
```

### 使泛型之间自我约束

```typescript
function getWithKey<T extends any, K extends keyof T>(o: T, key: K): T[K] {
  return o[key]
}

const person = {
  name: 'mowtwo',
  age: 23
}

// person的参数key被推断为'name'|'age'
const n = getWithKey(person, 'name') // 这里推断出string
const a = getWithKey(person, 'age') // 这里推断出number
```

### 使用 type 进行类型运算

```typescript
type MyRecord<T extends keyof any, U> = {
  [k in T]: U
}

const rc: MyRecord<string, number> = {
  a1: 1,
  a2: 3
}
```

## 使用 infer

### 取出特定类型的某个字段类型

```typescript
type GetTType<T> = T extends { t: infer U } ? U : never

function setT<T extends { t: any }>(tF: T, t: GetTType<T>) {}

setT({ t: 'hello' }, 'hello world') // 这里推断出第二个参数类型是string
setT({ t: 1 }, 2) // 这里推断出第二个参数类型是number
```

### 取出函数第一个参数

```typescript
type GetFnFParam<T> = T extends (p1: infer U) => any ? U : never

function callFParamFn<T extends Function>(fn: T, p: GetFnFParam<T>) {}

callFParamFn((a: number) => {}, 1) // 这里推断出第二个参数类型是number
```

### 取出函数返回值类型

```typescript
type GetReturnType<T> = T extends (...args: any) => infer U ? U : never

function equalResult<T extends Function>(
  fn: T,
  result: GetReturnType<T>
): boolean {
  return fn() === result
}

console.log(equalResult(() => 2, 3)) // 这里推断出第二个参数类型是number
```

### 取出特定类型的 key 值

```typescript
type GetKeyType<T> = T extends { [key in infer U]: any } ? U : never

function fn<T>(o: T, keys: GetKeyType<T>[]) {}

fn({ name: '', age: 23 }, ['age', 'name'])
```

## 已有的高级类型

### Readonly

```typescript
// readonly关键字用法
type Readonly<T> = {
  readonly [U in keyof T]: T[U]
}
```

### Partial

```typescript
// +?跟-?的用法
type Partial<T> = {
  [P in keyof T]+?: T[U] // 这里的+可以省略
}
```

### Required

```typescript
type CusRequired<T> = {
  [P in keyof T]-?: T[U]
}
```

### Record

```typescript
// 单独使用in关键字
type Record<K extends keyof any, T> = {
  [P in K]: T
}
```

> 常用的高级类型非常多，这里需要自己探索

## 使用类型守卫

由于泛型/unknown（实际上还有函数重载的情况）的存在，有时候我们并没有办法在代码里很好的提供类型的一个收缩或者扩展，这样我们得到的类型是不能通过编译以及得到代码提示的，所以我们需要一个能够实现安全类型转换的功能，那就是类型守卫。

### `typeof`关键字/类型判断

```typescript
function fn(n: string | number) {
  if (typeof n === 'string') {
    return n.toUpperCase()
  } else {
    return n.toFixed(2)
  }
}
```

#### JS 的`typeof`跟类型系统的`typeof`的差异

在 ts 的类型定义，泛型定义等等地方也都是可以使用 typeof 的，但是这里的 typeof 一定要注意，它们跟直接写在逻辑代码里的 typeof 是两种东西。

在逻辑代码中的 typeof 返回的类型定义的字符串，并且只支持九大基础数据类型（截止 2021 年 12 月为止），。而在类型定义里的 typeof 返回的是类型定义的元信息，也就是真正的表现表示，可以被当做类型识别，并且在后面跟随的必须是一个变量而不应该是一个字面量值。

因为变量是标识符，可以编译器保留签名信息，而字面量在编译时只会被简单的保存到数据表类。

```typescript
const s = '1'

type S = typeof s // 此时推导的类型的是 '1'
```

#### 类型声明空间跟变量声明空间

在 TypeScript 中，实际上定义了两个存储代码的空间，类型声明空间与变量声明空间，这个是在编译时规定的，而这么做的原因是为了能够更好的分离代码，因为 TypeScript 最终是要编译回 JavaScript 的。

但是并不是所有的代码都只在一个空间内存储，相反，有部分代码会被编译器进行关联，虽然最终类型定义里的那部分会再编译后抹去，可是却会在类型判断时依然生效。

比如 class 关键字与 enum 关键字，class 关键字导出的实际上除了真正的构造函数对象以外，还额外导出了一个类型定义，这种定义类似于 interface 或者抽象类，它们仅仅只是表达了类型本身的结构而不具备被实例化的功能。而 enum 更加特殊，它本身就是一个对象模拟出来的类型，但是这里并不需要深究 enum，因为它并不是被推荐常有的关键字。

TypeScript 也在 4.2 版本之后引入了对类型隔离引入的专用关键字`import type`，这个关键字在编译时仅仅只是作为类型检查的一局，而不会真正的引入对应的代码，这样可以减少检查代码的时间（模块化检测）跟打包后的代码大小。

```typescript
// ./src/lib/TypeDef.ts
export class TypeA {}

export interface TypeB {}

// ./src/main.ts
import type { TypeA, TypeB } from './src/lib/TypeDef.ts'

const a: TypeA = {} // 可以
const _a = new TypeA() // 不行
const b: TypeB = {} // 可以
```

### 实例判断/`instanceof`关键字

typeof 提供的是基础数据类型的判断，而 instanceof 则提供了对复杂类型，也就是引用数据类型的判断。其实这个关键字本身也是来自于 JavaScript，并且并没有在 TypeScript 中进行改造，所以行为上是保持与 JavaScript 用法一致。

但是这里要注意一些容易用错的场景，比如 instanceof 只能对真正的实例类型进行判断，也就是在变量声明空间下定义的 class 类型的实例对象进行判断。

```typescript
import type { B } from './T'
class A {}

const a = new A()
if (a instanceof A) {
} // 可以
const b: B = {}
if (b instanceof B) {
} // 不行
```

### 字面量等值判断/`===,!==,switch,if`

```typescript
interface A {
  type: 'A'
  aName: string
}
interface B {
  type: 'B'
  bAction: () => string
}

function AOrB(o: A | B) {
  switch (o.type) {
    case 'A':
      console.log(o.aName) //此时为类型A
      break
    case 'B':
      console.log(o.bAction()) //此时为类型B
      break
  }
}
```

### 自定义类型守卫

```typescript
interface A {
  type: 'A'
  aName: 'name is a'
}

function isA(target: any): target is A {
  return !!target.type && target.type === 'A'
}

const a = { type: 'A', aName: 'demo' }
if (isA(a)) {
  console.log(a.aName) // 此时为类型A
}
```
