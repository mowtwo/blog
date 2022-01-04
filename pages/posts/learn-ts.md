---
title: TypeScript(1)
date: 2021/12/16 12:21:00
description: TypeScript是现代前端开发者必须掌握强化技能之一
tag: TypeScript
author: Mowtwo
---

# TypeScript(1)

> 编写者：Mowtwo

TypeScript是现代前端开发者必须掌握强化技能之一

## 简介

### TypeScript的作者

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211105542506.png" alt="image-20211211105542506" style="zoom:50%;" />

> Anders Hejlsberg，计算机科学家，现在是微软.Net的首席架构师，同时也是Delphi（基于Pascal的可视化GUI开发工具），C#之父。

### 什么是TypeScript

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211105702149.png" alt="image-20211211105702149" style="zoom: 67%;" />

> Typescript是有类型的JavaScript，同时它也是一门完全独立的编程语言。
> 它拥有强大的类型系统，可以完美的兼容JavaScript，同时它可以帮助你弥补很多JavaScript欠缺的东西。

## 使用TypeScript

在真实项目中，TypeScript往往是作为项目的一部分存在，而现代化的项目脚手架都会帮忙配置好TypeScript的使用环境，但是有时候我们也需要配置一些基础的环境来完成开发工作。

### 安装编译器

```bash
npm install -g typescript
```

安装完成后，npm link会自动在我们的环境变量中释放一个`tsc`命令，我们可以通过`tsc`命令来查看安装情况

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211110503805.png" alt="image-20211211110503805" style="zoom: 67%;" />

其中`tsc`的版本号就代表了我们当前语言能支持的最高语法版本（LTS），不同版本的语法会带来很多新的特性，包括对于ECMAScript新规范的支持，强化的TypeScript特性，更强大的类型工具等等，这些东西都会持续的在TypeScript中被不断添加

### 查看帮助

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211110800977.png" alt="image-20211211110800977" style="zoom:67%;" />

`tsc`本身也是一个强大的cli工具，提供了基于命令行的快捷指令，通过不同的参数可以快速使用对应的功能

### 创建新的项目

一个TypeScript的最小项目可以很简单，不需要复杂的配置，只需要在某个文件夹下，创建一个`.ts`文件就可以，不过散乱的源码文件会让项目管理最终变得无法完成，任何工作都应该从规范开始。

规范本身只是提供一个约定俗成的作用，但是如果讲约定俗成的东西形成习惯，那么有时候它也可以成为规则，所以TypeScript对于一个完整的**工程**本身也定下了*规则*。

不过在创建TypeScript项目之前要注意，TypeScript大部分情况下，是依赖于npm进行包管理的，所以我们也应当给TypeScript项目先进行一次npm的初始化。

```bash
mkdir test-demo && cd test-demo # 场景并进入目录
npm init -y # 初始化package.json
```

此时查看路径，我们就可以看到一个简单项目结构

<img src="https://raw.githubusercontent.com/mowtwo/pic-go/main/markdown/image-20211211112818940.png" alt="image-20211211112818940" style="zoom:67%;" />

### 初始化配置

在TypeScript中，用来形成约束规则的存在，即为一个配置文件`tsconfig.json`，这个文件本身其实由来已久，它并不是单纯出自于TypeScript本身，而是微软对于自家产品的联合应用。

前端常用的编辑器`VSCode`，在早期就是专门设计出来给JavaScript开发者使用的，所以内置了很多针对JavaScript的功能，但是JavaScript由于其特有的灵活性，如果将功能一口气全部集成或者应用，会让编辑器本身变得臃肿，并且性能低下（虽然本身性能就不算很好）。所以，模块化的配置与插件化的功能解耦出现了，针对于JavaScript天生对`JSON`的亲和力，`JSON`成为了`VSCode`所有配置的第一选择。

当你在VSCode中创建了一个名为`jsconfig.json`的文件时，这个文件就会自动对你项目中的JavaScript生效，它可以帮助你指定VSCode的代码提示范围，JavaScript应用的ECMAScript版本，以及对应的模块化（AMD，CMD，UMD，ESM等等）类型等等。

对应的在TypeScript中，`tsconfig.json`也充当起了对应的角色，但是它更加与众不同，因为它不但作为配置存在，它还是tsc本身用来约束项目本身的存在。也就是说，当你在项目的根目录下创建了这个文件，在tsc被调用并且没有指定配置入口时，它会去主动读取默认的这个`tsconfig.json`.

tsc内置了对一些通用配置的生成，你只需要在你的项目根路径上开启一个终端（Terminal，Windows上指的是CMD/PowerShell/Windows Terminal，*nix系统则为各类Shell）便可创建

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
    "skipLibCheck": true, // 跳过对.d.ts文件的检查
  },
}
```

可以看到这里的JSON实际上并不是传统意义上的标准JSON，而是支持注释并且更灵活的一种JSON实现，在VSCode一般就是给编辑器配置使用。

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
  },
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

在上面的最小程序创建的时候，我们应用到了一些TypeScript的基础知识，但是其实可以看出跟JavaScript最不一样的地方，应该就是关于类型的描述。在TypeScript的官方定义上，TypeScript的最初的本质目的就是为了解决JavaScript动态类型带来的一些问题，所以TypeScript是一门静态类型的编程语言，虽然它的静态类型的特性以及对于类型的检查仅仅停留在编译期，但是已经在很大程度上产生了作用。

但是TypeScript又不同于更传统一些的静态类型语言，它在产生的过程中，就已经吸收了很多来自于不同静态语言的特点，并且对它们的优势进行了过滤，特别是从C#中吸收了大量了特性（毕竟是同一个作者），C#作为微软推出的一门大杀器，在语法糖跟语言特性方面推进的一直都是十分大胆。但是抛去C#这门的影响，其实JavaScript本身的**函数是一等公民***（First-class Function）的特性，又从各类ocaml语言中吸收的很多新特性，提供了很多帮助函数式编程的功能。

不过在了解高层次的类型系统应用之前，必须了解TypeScript的基础类型系统，这里面包含了很多关于JavaScript兼容性的使用，以及TypeScript如何处理特有的类型系统与JavaScript已有的类型系统的转行过程，这些过程往往是在底层做了一些builtin处理，或者更加hacker的做法，所以在使用上不存在困难，反而应该是非常平滑的。

### 鸭子类型

> 当你看到一个对象，它看起来像鸭子，吃起来像鸭子，走路也像鸭子，那你就可以把它当成鸭子

TS的类型系统不同于场景的标明类型系统（Nominal Type System），而是更加灵活的结构类型系统（Structural Type System）。这种类型并不会将类型的名字写入签名作为类型检查的根据，而仅仅比较类型本身的结构组成，既支持鸭子类型（Duck Type）。

### 基础数据类型

> 这部分只描述数据类型，也就是所谓的字面量类型，在JS中也有关于字面量类型跟包装类型的差别，但是TS将这部分差别在表面上已经体现出来
>
> 在TS中，字面量类型应该是小写的，而大写的类型代表了其对应的包装类型的构造函数，这样就会产生很大的不同，因为包装类型对象本身是引用类型对象（Reference），在内存存储结构上是不同的

#### number

TypeScript的数值类型跟JS的保持一致，其底层就是双精度浮点类型，正常情况下是64位长度（double float）

#### string

与JS保持一致

#### boolean

与JS保持一致

#### null

与JS保持一致，不过由于在JS里设计的特殊性，在TS的类型系统里，为它增加了特殊的hack，使其是任意引用类型的`subtype`

#### undefined

与JS保持一致，有点类似null的hack，但是比null更特殊，本身即是值，又是自己的类型

#### void

这个是区别于JS的类型，它的作用更接近C语言的void，但是又像是ocaml语言中的unit，代表了本身并不关心返回值类型，一般用于函数的返回类型的标注，与JS中的void关键字区分，因为JS的void关键字是为了产生真正的Undefined来使用的（JS的**u**ndefined本身并不是一个关键字，相反它可以被覆盖，所以底层还有一个真正的Undefined）。

#### never

TS特有类型，是一个辅助类型，一般用于函数返回，本身代表了不关心值的类型，并且这个值永远不会返回有意义的值

#### any/unknown

> any是TS留给不熟悉TS类型的开发者的一个hack通道，使用any是一个非常危险的事情，会导致类型丢失，这些是无法预测的安全隐患，请不要在项目里大量使用any，如果可以，请使用unknown代替

any代表了它不关心变量的类型，也不关系类型本身的存储方式（引用或者值类型），被标记上any的变量可以像JS里的变量一样使用，而且编译器将any做了特殊的hack，使其即是任意类型的super type，也是任意类型的sub type。

unknown是TS在3.0版本被引入的特殊类型，它被理解为类型安全版本的any，但是它其实在行为上并不等同于any。在使用它时，处理所标记的变量需要手动检查类型转换问题，与any最大的区别就是：any放弃了类型检查，让程序自身自灭，而unknown是让编译器信任开发者，让开发者处理好类型检查的事情。

### 集合数据类型

> 其实所谓的集合数据类型在官方分类中，也是数据基础类型，但是实际上在表达上应该被区分

#### object

与JS保持一致，但是这个类型其实挺危险的，使用上应当参考any的危险性

JS的object在底层实现上实际上是一个特殊的hashmap，而hashmap本身就代表了不确定性，而在TS中，被标记了object类型的变量也会丢失很大一部分的类型检查，比起any仅仅只是少了一些空指针异常的风险，可是在访问属性的时候，依然存在空属性异常的风险。

#### array

TS的数组本质与JS保持一致，但是对于类型安全上的一些改造，让array变得更值得使用。（虽然在JS 中，array只是特殊的object）

TS的array支持在声明的时候，通过泛型，或者特定的结构语法进行声明，可以在声明时就确定集合的类型范围，这样对于数据子项的类型安全是有利的。

并且TS支持类型推导，还有联合类型，让数组声明本身变得没那么麻烦。

```typescript
const arr1 = [1] // number[]
const arr2 = [1,'a'] // (number|string)[]
```

#### tuple

TS提供了一种相对静态的数组类型，那就是元组，声明元组跟声明数组很像，并且二者在底层上都是一样的JS数组，不过元组可以让我们将类型收缩的更小。

```typescript
const arr1:[number] = [1] 
const arr2:[number,string] = [1,'a'] 
```

#### enum

TS的枚举类型，实际上是对对象的一种特殊封装，并且TS的枚举在形式上，存在两个形态。也就是在两方面描述了同一个东西，因为不同于其他高级类型的声明，在编译时，enum不会像他们一样被抹除，而是更接近`class`的行为，被保留下一份值映射的副本。

```typescript
// 声明的枚举
enum Result {
  OK,Err
}
```

```javascript
// 编译后的枚举
var Result;
(function (Result) {
    Result[Result["OK"] = 0] = "OK";
    Result[Result["Err"] = 1] = "Err";
})(Result || (Result = {}));
```

> 不过由于枚举本身的特殊性，不是很推荐在项目中使用大量的枚举，而是应该使用静态枚举，或者直接使用字符串**联合类型**（Union Types），这样可以减少很多不必要的样板代码的生成，也可以减少外部调用者对代码的疑惑性

```typescript
// 声明的枚举
const enum Result {
  OK,Err
}
const a:Result = Result.OK
```

```javascript
// 编译后的枚举不在保留，而是直接替换成对应的值
var a = 0 /* OK */;
```

### 类型关键字

> 其实上面的的enum在某种意义上也算是一种特殊的类型关键字，但是由于其行为更接近一个集合，所以不放在这里

在TS里，还可以通过一些特定的关键字来创建一些额外的类型，这些类型可能是类型别名（alias），接口（interface），或者类类型（Classes），这些都是合法的类型声明方式，并且都可以在任意合适的场合被使用，因为这些类型可以保证你的项目保持一个相对的功能健壮性，并且可以帮助编译器在输出代码时完成合理的类型安全检查。

不同的关键字，有时候可以输出相同的结果，有时候却会在行为上产生大相径庭的结果。

#### class

这个关键字其实也是JS在ECMAScript2015（即ES6）中引入的一个特殊的语法关键字，在JS中，其实它就是构造函数的一个语法糖，并且也提供了对应的OOP的简易实现，但是JS中的class还是有很大的缺陷，在很多场景下无法担起重要的任务。比如React跟Vue都在慢慢移出class关键字，改用function的方式来渲染组件，因为在软件工程的设计中，其实一直都是参照着一种方式，即为数据驱动UI的设计理念。

```
UI = Model(Data,Event)
```

不过TS在设计的早期，由于C#的影响，所以依然对class进行了大量的完善和改造，这让TS的class关键字变得能够使用，并且值得被大量使用，特别是自己进行一些设计模式的实践时。

*传统UI开发其实还曾经遵循过MVC，而OOP是MVC实现的一个关键。*

```typescript
class Person {
  constructor(
    private firstName: string,
    private lastName: string
  ) { }
  getFullName() {
    return this.firstName + this.lastName
  }
}

class OfficeWorker extends Person {
  constructor(firstName: string, lastName: string, public readonly wkNo: string) {
    super(firstName, lastName) // 必须调用super关键字，否则无法在上下文使用this关键字
  }
  getWorkerInfo() {
    return `Info(fullName=${this.getFullName()},workerNo=${this.wkNo})` // 这里的getFullName继承自Person
  }
}

const wk = new OfficeWorker('陈', '文程', '80064')
console.log(wk.getWorkerInfo()) // 输出:Info(fullName=陈文程,workerNo=80064)
```

关于class的应用更多的在OOP层次上的应用，因此要等到OOP讲解时，才能扩展更多的内容。

#### interface

interface就是让TS完全区别于JS的一个重要特性之一，基于interface，TS能够实现的东西将会完全超出普通的静态类型编程语言的范畴，并且能够使其应用高度深入到计算机科学的领域。

> 但是实际上还需要提前感谢type关键字与泛型，interface跟type的结合，让TS没有沦为一个C# like in js，type让TS更完整，更强大。而泛型是一门强大的语言必备的一个特性，它直接让TS的类型系统本身实现了图灵完备（Turing completeness）的特性
>
> 在技术领域，甚至产生了专门研究TS类型体操（**type gymnastics**）的开源组织
>
> 甚至有人用TS的类型系统实现了另一门语言（大名鼎鼎的Lisp）的解释器

使用interface可以规定一个对象的实现类型，以及属性约束，类型扩展（这个比较特殊）等功能。

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
  getFullName() { return 'mowtwo' },
  getWkInfo() { return `Info(fullName=${this.getFullName()},workerNo=${this.wkNo})` }
}

console.log(wk.getWkInfo()) // 输出:Info(fullName=mowtwo,workerNo=80064)
```



#### type

type关键字是一个功能非常多的关键字，它在某些场合是完全可以代替interface来使用。但是type能做的远不只是声明接口那么简单，它还可以给类型声明别名，声明联合类型（Union Types）与交叉类型(Intersection Types)，这些给TS的类型系统带来了很大的灵活性。

```typescript
type SystemString = string // 创建基础数据类型别名

interface Person {
  getFullName: () => string
}

type AnyPerson = Person[] // 将数组声明为单独的类型

type Result = 'Ok' | 'Err' // 联合类型
```

## 面向对象

>  TS的OOP本身是一个很大的使用课题，OOP本身也带来了很多便利，但是OOP也有很明显的缺陷，使用OOP前需要评估好自己对于软件工程的理解水平，设计模式的使用才是OOP的精髓所在

### 继承与多态

在上面class的例子中，其实就已经展示了TS的class在继承方面的应用，但是实际上，OOP的继承最终目的就是为了实现多态的应用，不过由于JS 对于class本身实现的缺陷，JS的多态非常有局限性，但是TS相对弥补了其中的缺陷，让多态实现上会更加流畅。

> **多态**即为，一个统一接口（interface/protocal）下的多个实例物体的实现不同，但是可以被传递到同一个输入中。

```typescript
abstract class Animal {
  constructor(
    private typeName: string,
  ) { }
  getTypeName() { return this.typeName }
  setTypeName(typeName: string) { this.typeName = typeName }
  abstract callSound(): void;
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
  constructor() { }
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

由于OOP中，子类有很多来自于父类或者更高的基类继承的方法和属性，但是有些时候基类提供的这些实现并不是我们所想的，或者基类是抽象类的时候，我们必须去手动实现对应的方法或者属性时，就会用应用到**重写**（Override），在上面的例子已经表现出对于抽象类的实现重写。

```typescript
class Person {
  constructor() { }
  walk() {
    console.log('walking')
  }
  getType() {
    return 'person'
  }
}
class Student extends Person {
  constructor() { super() }
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

重写在JS中其实就已经可以实现，但是重载则是TS完全不同于JS的实现，JS并不支持重载。

> TS的重载不只是应用于OOP中，而是对任何函数都会产生效果，但是实际上并不推荐在TS里大量使用重载，而是应该使用可变参数列表，可选参数，命名参数的方式增加扩展性，过多使用重载其实会产生一些不必要的歧义与学习成本。

重载（Overload），代表的是对于同一个函数名的不同实现，而其判断的标准则是参数列表中的类型签名的不同，在实际应用中，TS的重载其实也不算很好的实现，当然，这些是为了兼容JS付出的代价。

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

**Generic**是静态类型语言里非常重要的特性，TS的泛型更是直接让TS本身的强大上升了一个等级，使其脱离了普通的静态类型编程语言的范畴。

虽然泛型在定义上是非常简单的，即为可变的类型参数。

由于静态类型编程语言的类型限制，有时候我们需要实现一些通用功能函数的时候，需要编写大量的样板代码，或者重复性的类型区分声明。（虽然在TS里有any，unknown，重载等等各种替代方案，但是都好丑，并且TS实际上对于number类型的宽泛定义，并不会有太大的烦恼）

> 这个地方适合口述，并且结合实例，写文字是写不出来的