---
title: 糖文档-TypeScript
header-img: back.png
header-color: 'rgb(255, 255, 255)'
tag-color: 'rgb(3,50,76)'
catalog: true
toc-number: false
date: 2020-03-08 12:51:44
tags: ['javaScript', 'typeScript']
---

# 前言

---

根据[官方文档]( https://www.tslang.cn/docs/handbook/tsconfig-json.html )整理

# 正文

---

## 1. 基于 VSCode 的配置

### 1.1 全局安装 TypeScript

```js
npm install typescript -g
```

### 1.2 手动编译

使用 `tsc` 即 *typescriipt compile*

```js
tsc test.ts
```

### 1.3 自动化编译

不带任何输入文件的情况下调用 `tsc` ，编译器会从当前目录开始查找 `tsconfig.json` 文件，逐级向上搜索父目录。 

**生成 tsconfig.json**

使用 `tsc –-init` 可以自动生成一个 `tsconfig.json` 

```js
tsc --init
```

`tsconfig.json` 可以配置编译选项，比如输出目录:

```js
"outDir": "./js",  
```

**设置监视**

终端 - 运行任务 - 监视tsconfig.json

![auto](auto.png)

## 2. 基础类型

TypeScript 支持与 JavaScript 基本相同的数据类型，此外 TS 同时提供了使用的枚举类型。

### 2.1 数字

使用 `number` 表示数字

```js
let num: number = 10
```

### 2.2 字符串

使用 `string` 表示字符串

```js
let str: string = '10'
```

### 2.3 布尔型

使用 `boolean` 表示布尔型

```js
let isDone: boolean = true
```

### 2.4 undefined & null 

分别使用 `undefined` 和 `null ` 表示这两个类型。默认情况下它们是任何类型的子类型，即可以赋值给任意类型的变量。但当指定了建议指定的 `–strictNullChecks` 标记，`null` 、`undefined`、`void` 只能赋值给各自，这能避免很多问题。如果在某处想传入一个 `stirng | null | undefined`可以使用联合类型。

```js
let varible1: undefined = undefined
let varible2: null = null
```

### 2.5 object

`object` 表示非原始类型, 也就是除了以上介绍的类型。 使用 object 类型可以更好得表示像 `Object.create()` 这样的 API

```js
declare function create(o: object | null): void
create({name: 'TangBii'})  // 可以通过类型检查
create(1)                 // 无法通过类型检查
```

### 2.6 数组

有两种方式可以定义数组。可以在元素类型后加 `[]`， 也可以使用数组泛型 `Array<类型>`

```js
// 方式一
const nums: number[] = [1, 2, 3]

// 方式二
const nums2: Array<number> = [1, 2, 3]
```

### 2.7 元组

元祖类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。不能越界访问，越界元素类型为 `undefined`

```ts
let tuple: [string, number] = ['10', 10]
tuple[0].split('')  // 可通过编译
tuple[1].split('')  // 不可通过编译，因为 number 无 split() 方法
tuple[2] = 10       // 不可通过编译，因为 10 不能赋值给 undefined
```

### 2.8 枚举

`enum` 是对 JavaScipt 标准数据类型的补充。可以给一组数值赋予友好的名字。默认情况下从 0 开始为元素编号，也可以手动指定

```js
enum Color {Red, Green = 3, White}
const color: Color = Color.Red
const colorName: string = Color[3]
console.log(colorName)  // Green
```

### 2.9 any & void 

可以使用 `any` 类型标记我们不想进行类型检查从而直接通过编译阶段检查的变量。这种变量通常来自用户输入或第三方代码库。

`any` 与 `object` 的区别是虽然 `object` 也可以接收任意类型的变量，但是无法调用其身上的任意方法

```js
let varible1: any = 'hello'
let varible2: Object = 'hello'

varible1.split()	// 可以通过类型检查
varible2.split()	// 不能通过类型检查
```

`void` 与 `any` 相反，表示没有任何类型，常用于函数返回值。声明一个 `void` 变量没什么用，因为只能给它赋值 `undefined` 和 `null`

```js
function print(): void {
  console.log('hello')
}
```

### 2.10 never

`never` 表示永不存在的值的类型，例如 `never` 可以表示总会抛出异常的函数返回值。

`never` 类型是任何类型的子类型，即可以赋值给任意类型。但是没有任何类型的值是 `never` 的子类型，即没有任何值可以赋值给 `never` 包括 `any`

```js
function fn(): never {
  throw new Error('error')
}
```

### 2.11 类型断言

如果你清楚地直到一个实体具有比现在更确切的类型可以使用断言，即告诉编译器 “相信我， 我知道在干什么”。类型断言类似于类型转换，但不进行特殊的数据检查和解构。没有运行时影响，只在编译阶段起作用。

断言可以使用 `<type>` 或 `as` 表示，但在 jsx 中只能使用 `as`

```js
// 方式一
let str: any = 'hello'
let len: number = (<string> str).length

// 方式二
let str: any = 'hello'
let len: number = (str as string).length
```

## 3. 接口

接口的作用是为自定义类型命名和为自己的代码或第三方代码订立契约

### 3.1 定义接口

使用 `interface` 定义

```js
interface Person {
  name: string
}

function sayName(person: Person): string {
  return person.name
}

sayName({name: 'TangBii'})

let p1 = {name: 'TangBii', age: 16}
sayName(p1)
```

以上的例子中声明了一个 `Person` 接口，它有且仅有一个 `string` 类型的属性 `name` 

我们在这里不能说实现了接口。因为 TS 只关注值的外形，只要传入的对象满足接口的必要条件就可以被接受。另外不检查属性的顺序，只要相应的属性存在并且类型匹配即可。

另外我们可以看到函数接收了 `p1` 作为参数，但是 `p1` 相对于接口 `Person` 多了一个 `age` 属性，TS 有时并不会这么宽松，我们将在[检查额外属性](#extra)这一节介绍

### 3.1 可选属性

带有可选属性的接口定义方式于普通接口的定义方式差不多，只需在可选属性名字定义后加一个 `?`

可选属性有两个好处:

- 预定义可能存在的属性
- 捕获引用了不存在属性时的错误

```js
interface Person {
  name: string;
  age?: number
}

function sayName(person: Person): string {
  return person.name
}

sayName({name: 'TangBii'})

sayName({name: 'TangBii', age: 16})  // 编译可通过，因为 age 是可选属性
```

### 3.3 只读属性

可以在属性名前使用 `readonly` 声明可读属性，这些属性只能在对象刚创建的时候修改值。

TS 也可以用 `ReadonlyArray<类型>`  声明可读数组，为防止数组被更改，去掉了所有可变方法，包括 `push`、`通过索引的访问` 等。 可以用断言 `as number[]` 重写

```js
interface Person {
  name: string;
  readonly score: number
}

const person: Person = {
  name: 'TangBii',
  score: 99
}

person.name = 'TangBiiNew'  // 可修改
person.score = 100  // 不可修改
```

### <name id='extra'></name>3.4 检查额外的属性

 当把对象字面量赋值给变量或传递给函数时会对额外的属性进行检查，如果含有任何目标类型不包含的属性则无法通过编译。

我们把起初那个例子稍作修改, 不传入 `p1` 而直接传入对象字面量，可以发现无法通过编译，因为多了一个 `age` 属性

```js
// let p1 = {name: 'TangBii', age: 16}
sayName({name: 'TangBii', age: 16})
```

我们可以使用一些方法饶过这些检查，但要谨慎使用:

- 把对象字面量赋值给一个变量，正如我们开始所做的那样

  ```js
  let p1 = {name: 'TangBii', age: 16}
  sayName(p1)
  ```

- 使用类型断言

  ```js
  sayName({name: 'TangBii', age: 16} as Person)
  ```

- 给接口添加一个字符串索引签名。这样 Person 类型就可以具有任意数量任意类型的其他属性

  ```js
  interface Person {
    name: string
    [others: string]:any
  }
  ```

### 3.5  描述函数类型

接口除了描述带有属性的普通对象外也可以使用一个调用签名让接口表示函数类型，它就像一个只有**参数列表**和**返回值类型**的函数定义，参数列表的每个参数都需要名字和类型。

```js
interface Personable {
  (name: string, age?: number): {name: string, age?: number}
}

const Person: Personable = function (name, age) {
  return {name, age}
}
```

使用函数接口作为类型定义的函数的参数和返回值会自动推断类型

### 3.6 可索引的类型

与调用签名类似，也可以用接口描述 ‘可通过索引得到’ 的类型，比如 `a[10]` 、`person['Tang']`。

可索引类型具有一个索引签名，描述了对象索引的类型，还有一个索引返回值类型

```js
interface stringArr {
  [index: number]: string  // 索引为数字，返回值为字符串
}

const myArr: stringArr = ['str1', 'str2']  // 返回值都为字符串，可通过编译
const myArr2: stringArr = ['str1', 1]   // 返回值包含数字，不可通过编译
```

TypeScript 支持两种索引类型: **数字索引** 和 **字符串索引**。可以同时使用，但是数字索引返回值必须是字符串索引返回值的子类型。因为使用数字索引时，JavaScript 会先把它转换为字符串再去索引对象。索引签名也可以设置为 `readonly` 防止赋值

###  3.7 类类型接口

#### 3.7.1 实现接口

与 C# 或 Java 类似，也可以声明一个类接口。类实现接口时使用关键字 `implements`

```js
interface Sayable {
  say(): string
}

class Person implements Sayable{
  name: string
  constructor(name: string) {
    this.name = name
  }
  say() {
    return this.name
  }
}
```

#### 3.7.2 类的静态部分与实例部分

操作类的时候要注意类有两个类型: **实例部分的类型** 和 **静态部分的类型**。构造函数属于类的静态部分类型，定义接口要单独定义

```js
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick();
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

### 3.8  接口继承

和类一样，接口也可以继承，并且可以继承多个接口

```js
interface Sayable {
  say(): string
}

interface Runable {
  run(): string
}

interface Person extends Sayable, Runable {
  eat(): string
}

const p1: Person = {
  say() {
    return 'I can say'
  },
  run() {
    return 'I can run'
  },
  eat() {
    return 'I can eat'
  }
}
```

### 3.9 混合类型

因为 JavaScript 动态灵活的特点，有时希望一个对象可以同时具有多种类型。比如一个对象可以同时作为函数和对象使用并带有额外属性

```js
interface Callable {
  (content: string): string
  voice: number
}

const createCall = function (voice: number): Callable {
  const call = <Callable> function (content: string): string {
    return content
  }
  call.voice = voice
  return call
}

const call: Callable = createCall(10)

call('Im Callable')       // 作为函数
console.log(call.voice)   // 作为对象
```

### 3.10  接口继承类

当接口集成一个类时，会继承类的所有成员 (包括私有的和受保护的) 但是不包括实现。当接口继承了一个拥有私有或保护类型的类时，这个接口只能由这个类或这个类的子类实现。

```js
class Person {
  protected name: string
  constructor(name: string) {
    this.name = name
  }
}

interface Personable extends Person{
  say(): string
}

class Student extends Person implements Personable{
  say() {
    return this.name
  }
}

const s1 = new Student('TangBii')

console.log(s1.say())  // TangBii
```

## 4. 类

### 4.1 定义

```js
class Person {
  private age: number

  constructor(private name: string, age:number) {
    this.name = name
    this.age = age
  }
  
  sayName():void {
    console.log(this.name)
  }
}
```

可以在构造函数中使用**参数属性**，即在参数前面加一个访问限定符。这样可以在同一个位置声明并初始化一个成员。上例中 `age` 没有使用参数属性，所以需要在构造函数外单独声明一个 `age` 变量，`name` 使用了参数属性

**参数属性只能用于构造函数**

### 4.2 修饰符

**public**

用 `public` 定义的成员可以在程序中自由访问。JavaScript 成员默认为 `public`

```js
class Person {
  public name: string
  constructor(name: string) {
    this.name = name
  }
}

const p1 = new Person('TabgBii')
console.log(p1.name)  // 'TangBii' 在外部仍可访问
```

**protected**

用 `potected` 定义的成员不能在类的外部访问，但是可以在派生类中访问

```js
class Person {
  protected name: string
  constructor(name: string) {
    this.name = name
  }
}

class Student extends Person {
  constructor(name: string) {
    super(name)
  }

  sayName():void {
    console.log(this.name)
  }
}

const s1 = new Student('TabgBii')
console.log(s1.sayName())  // 编译通过，可以在派生类的内部使用
console.log(s1.name)  // 编译不通过，不能在类外部使用
```

`protected` 也可用于构造函数，表示类无法被实例化，只能在派生类中被实例化

```js
class Person {
  protected name: string
  protected constructor(name: string) {
    this.name = name
  }
}

class Student extends Person {
  private p1: Person = new Person('TangBii')  // 编译通过
  constructor(name: string) {
    super(name)
  }
}

const s1 = new Person('TabgBii')  // 编译不通过
```

**private**

用 `private` 定义的成员只能在定义类的内部访问

```js
class Person {
  private name: string
  constructor(name: string) {
    this.name = name
  }
}

class Student extends Person {
  constructor(name: string) {
    super(name)
  }

  sayName():void {
    console.log(this.name)  // 编译不通过, nanme 只能在 Person 类中使用
  }
```

**readonly**

可以使用 `readonly` 把属性设置为只读，只读属性必须在声明时或在构造函数中被初始化

```js
class Person {
  readonly name: string
  constructor(name: string) {
    this.name = name
  }
}

const p1 = new Person('TabgBii')
console.log(p1.name)  // 编译通过，可以读取值 
p1.name = 'TangBiiNew'  // 编译不通过，不可修改
```

### 4.3 存取器

在方法前加一个 `get` 或 `set` 可以设置存区器。只带有 `get` 不带有 `set` 的存取器会被自动推断为 `readonly`

```js
class Person {
  private _age: number = 0

  get age() {
    return this._age
  }

  set age(age: number){
    if(age > 0 && age < 100) {
      this._age = age
    } else {
      throw new Error('输入异常')
    }
  }
}
```

### 4.4 静态属性

使用 `static` 可以声明静态属性。要通过 `类.静态属性` 调用

```js
class Student {
  static class: string = '01'

  static sayClass():void {
    console.log(Student.class)
  }
}

Student.sayClass()
```

### 4.5  抽象类

使用 `abstract` 可以声明抽象类,一般不创建抽象类的实例。与接口不同，抽象类可以包含成员的实现细节。抽象类中不包含具体实现的方法必须在派生类中实现

```js
abstract class Person {
  constructor (protected name: string) {
    this.name = name
  }
  abstract sayName():void
}

class Student extends Person {
  constructor(name: string) {
    super(name)
  }
  sayName():void {
    console.log(this.name)
  }
}
```

## 5. 函数

### 5.1 定义类型

为函数定义类型即定义**参数的类型**和**返回值类型**

```js
const fn: (num1: number, num2: number) => number = 
(value1: number, value2: number) => value1 + value2
```

TypeScript 能根据返回语句自动推断返回值类型，所以我们通常省略它，以上例子可以简化为:

```js
const fn = (value1: number, value2: number) => value1 + value2
```

### 5.2 可选参数

在参数名后 `?` 可以声明可选参数，可选参数必须在必选参数之后

```js
function fn(firstName: string, lastName?: string): string {
  if (lastName) {
    return firstName + lastName
  } else {
    return firstName
  }
}

fn('Tang')                // 编译可通过
fn('Tang', 'Bii')         // 编译可通过
fn('Tang', 'Bii', 'new')  // 编译不可通过
```

### 5.3 默认参数

声明形参的同时可用 `=` 设定默认参数。默认参数都是可选参数，但如果默认参数在前面想应用默认参数时必须显示传入 `undefined` 指定应用默认参数

```js
function fn(firstName = 'Tang', lastName: string): string {
  if (firstName) {
    return firstName + lastName
  } else {
    return firstName
  }
}

fn('Bii')             // 编译不可通过
fn(undefined, 'Bii')  // 编译可通过
```

### 5.4 剩余参数

与 JavaScript 类似，使用 `...变量名` 可以定义一个剩余参数数组

```js
function sum(value1: number, value2: number, ...rest: number[]): number {
  if (rest !== undefined) {
    return value1 + value2 + rest.reduce((pre, curr) => pre + curr, 0)
  } else {
    return value1 + value2
  }
}
```

##### 5.5 重载

可以为同一个函数提供多个类型定义来进行函数重载

```js
function sum(x: number, y: number): number
function sum(x: string, y: string): string
function sum(x, y): any {
  if(typeof x === 'number' && typeof y === 'number') {
    return x + y
  } else {
    return parseInt(x) + parseInt(y)
  }
}
```

## 6. 泛型

### 6.1 定义

使用泛型可以定义可重用的组件，一个组件可以支持多种类型的数据。泛型类似于一个类型的占位符。

以下函数表示传入一个任意类型的数据并原样返回，泛型与 `any` 的区别在于泛型保存了类型信息，比如传入 T 类型数据，返回值也必须是 T 类型数据

```js
function echo<T>(varible: T): T {
  return varible
}
```

### 6.2 调用

定义了泛型函数后可以使用两种方式调用。

- 传入所有的参数，包括类型参数

  ```js
  echo<string>('str')
  ```

- 利用类型推论，不必传入类型参数

  ```js
  echo('str')
  ```

### 6.3 泛型接口

```js
// 定义方式一
interface Iecho<T>{
  (varible: T): T
}

function echo<T>(varible: T): T {
  return varible
}

const echoo: Iecho<number> = echo

// 定义方式二

interface Iecho2 {
  <T> (varible: T): T
}

const echoo2: Iecho2 = echo
```

### 6.4 泛型类

与接口类似，在类名后使用 `<T>` 表示类型

```js
class Person<T> {
  constructor(private name: T) {
    this.name = name
  }
  sayName():void {
    console.log(this.name)
  }
}


const p1 = new Person<string>('TangBii')
const p2 = new Person<number>(10)
```

### 6.5 泛型约束

定义泛型变量 `name:T` 然后使用 `name.length` 时编译可能不通过，因为不受约束的泛型可以表示任意类型，而 `number` 等不存在 `length` 属性。此时我们 就需要对泛型加以一些约束。

让泛型继承于一个接口可以实现对泛型的约束

 ```js
interface Ilength {
  length: string
}

function getLength<T extends Ilength>(str: T): string {
  return str.length
}
 ```

## 7. 类型推论

TypeScript 里没给出类型的地方类型推论会帮助提供类型，这种推断发生在初始化变量和成员，设置默认参数值和决定函数返回值时，比如以下实例中 `x` 会被推断成 `number` 类型

```js
const x = 10
```

### 7.1 最佳通用类型

当需要从几个表达式推断类型时，会使用这些表达式的类型推断出一个最合适的通用类型。

由于最终的通用类型取自候选类型，有时候选类型共用同一个类型，但却没有一个候选类型能作为最终类型，例如

```js
const animals = [new Dog(), new Cat()]
```

我们想让 `animals` 被推断成 `Animal` 类型，但候选类型虽然都是 `Animal`的子类但本身不是 `Animal` 类型，所以无法被推断成 `Animal` , 最终类型为 `Dog | Cat` 联合类型

### 7.2 上下文推断

当表达式的类型与所处的位置相关时会发使用上下文推论获得类型。上下文类型也会作为最佳通用类型的候选类型

```js
function getAnimals(): Animal[] {
  return [new Dog(), new Cat()]
}
```

本例中最佳通用类型的候选类型有 `Dog`、`Cat`、`Animal` ，所以最佳通用类型为 `Animal`

## 8. 类型兼容性

### 8.1 基本规则

如果 `x` 要兼容 `y`, 那么 `y` 至少具有与 `x` 相同的属性

```js
interface IName {
  name: string
}

let x: IName = {
  name: 'TangBii'
}

let y = {
  name: 'TangBii',
  age: 18
}

x = y   // 编译可通过
```

本例中 `y` 可赋值给 `x` ，因为 `y` 中包含 `x`  必须的属性 `name`。

检查函数时使用相同的规则：

```js
function fn(name: IName) {}
fn(y)   // 编译可通过
```

### 8.2 比较两个函数

**比较参数**

```js
let x = function (varible1: number, varible2: string) {}
let y = function (varible1: number) {}

x = y  // 编译可通过
y = x  // 编译不可通过
```

本例中 `y` 可以赋值给 `x`， 因为 `y` 所有的参数在 `x` 中都有相应类型的参数，反之则不能赋值。

**参数双向协变**

当赋值给函数一个参数子类型时，可以通过断言使得编译成功

```js
interface IPerson {
  name: string
}

interface IStudent extends IPerson {
  score: number
}


function sayNameAsync(callback: (p: IPerson) => void) {
  setTimeout(callback,100 )
}

// 编译不通过
sayNameAsync((s: IStudent) => console.log(s.name))

// 左协商
sayNameAsync(<(s: IPerson) => void>((s: IStudent) => console.log(s.name)))

// 右协商
sayNameAsync((s: IPerson) => console.log((<IStudent>s).name))
```

**可选参数和剩余参数**

比较函数兼容性时，可选参数和必须参数是可以互换的。源类型上有额外的可选参数不是错误，目标类型的可选参数在源类型上没有对应的参数也不是错误。当一个函数有剩余参数时，它被当作无数个可选参数

```js
let fn1 = (name: string, ...args:any) => {}
let fn2 = (name: string, sex?: string) => {}

fn1 = fn2	// 编译可通过
```

**比较返回值**

```js
let x = function () {return {name:'TangBii', age: 18}}
let y = function () {return {name: 'TangBii'}}

x = y  // 编译不可通过
y = x  // 编译可通过
```

本例中 `x` 可以赋值给 `y` ，因为 `y` 的返回值类型是 `x` 返回值类型的子类型。系统强制源函数的返回值类型必须是目标函数返回值类型的子类型

**函数重载**

对于有重载的函数，源函数的每个重载都要在目标函数上找到相应的函数签名，这确保了目标函数可以在所有源函数可调用的地方调用

### 8.3 枚举兼容性

枚举类型与数字类型兼容，但不同枚举类型间不兼容

```js
enum Color {Red, Green, Blue}
enum Color2 {Pink, DeepPink, Orange}

let varible1: number = Color.Red  // 编译可通过，枚举与数字类型兼容
let varible2: Color = 2           // 编译可通过，枚举与数字类型兼容
let varible3: Color = Color2.Pink // 编译不可通过，不同枚举类型不兼容
```

### 8.4 类兼容性

类与对象字面量和接口差不多，但是类有静态类型和实例类型，比较时只比较实例类型。静态成员和构造函数不在比较范围内。

类的私有成员和受保护成员会影响兼容性。如果目标类型包含一个私有属性，源类型必须包含来自同一个类的这个私有成员。这允许子类赋值给父类，但不能赋值给其他有同样类型的类。

### 8.5 泛型兼容性

因为 TypeScript 是结构性的类型系统，类型参数只影响使用其作为类型一部分的结果类型

```js
interface Person <T>{}

let a: Person<string> = {}
let b: Person<number> = {}
a = b  // 可通过编译
```

```js
interface Person <T>{name: T}

let a: Person<string> = {name: 'TangBii'}
let b: Person<number> = {name: 10}
a = b  // 不可通过编译
```

## 9. 高级类型

### 9.1 交叉类型

交叉类型是把多种类型合为一个类型，包含了被合成类型的所有成员。 使用 `&` 连接多个类型可生成交叉类型

```js
interface Person<T, K> {
  name: T;
  age: K;
  combine: T & K
}

const p: Person<string, number> = {
  name: 'TangBii',
  age: 18,
  combine: <string & number>{
    name2: 'xiaoTang',
    age: 18
  }
}
```

### 9.2 联合类型

联合类型表示一个值是几种类型之一，用 `|` 分隔，所以 `number | string` 表示一个值可以是 `number` 或 `string`。但要注意如果一个值是联合类型，**只能访问联合类型的共有成员**

```js
interface Person {
  name: string;
  age: number
}

interface Student {
  name: string;
  score: number
}

function say(s: Person | Student) {
  console.log(s.name)  // 编译可通过，name 是公共属性
  console.log(s.age)  // 编译不可通过
  console.log(s.score)  // 编译不可通过
}
```

#### **类型检测**

可以使用**断言**检测交叉类型的具体值

```js
function say(s: Person | Student) {
  if((<Person> s).age) {
    console.log((<Person>s).age)
  }
  if((<Student> s).score) {
    console.log((<Student>s).score)
  }
}
```

#### **类型保护**

但是这就需要多次用到断言，如果我们一旦检测了类型就能在之后的分支清楚知道类型就好了。TypeScript 的**类型保护**机制实现了这个功能。

要定义一个类型保护只需要定义一个返回 `类型谓词` 的函数。谓词为 `parameterName is Type`  格式，`parameterName` 必须来自当前函数的一个参数名。

我们使用类型改造一下之前的例子

```js
function isStudent(s:Person | Student): s is Student {
  return (<Student>s).score !== undefined
}


function say(s: Person | Student) {
  if(isStudent(s)) {
    console.log(s.score)
  } else {
    console.log(s.age)
  }
}
```

类型保护不仅知道 `if` 分支是 `Student` 类型，还知道 `else` 分支中不是 `Student` 类型

**typeof 类型保护**

识别基本类型不必每次都定义一个 `类型谓词` 函数，TypeScript 会把 `typeof` 值等于 `number`、`string`、`boolean` 、`symbol` 的表达式识别为类型保护，如

```js
function fn(arg: number | string): number {
  if (typeof arg === 'string') {
    return arg.length
  } else {
    return String(arg).length
  }
}
```

**instanceof 类型保护**

类似于 `typeof` ，通过构造函数细化类型

```js
function say(s: Person | Student): number {
  if(s instanceof Student) {
    return s.score
  } else {
    return -1
  }
}
```

### 9.3 可以为 null 的类型 

默认情况下 `undefined` 和 `null` 可以赋值给任何类型。使用 `--strictNullChecks` 可以解决这个问题: 声明一个变量时不自动包含 `undefined` 或 `null` ，但可以使用联合声明显示包含。

使用了 `–strictNullChecks` 选项，可选参数和可选属性会自动包含 `| undefined`

```js
let varible1: number
let varible2: number | null | undefined

varible2 = null  // 编译可通过
varible1 = null  // 编译不通过
```

使用 `==` 、`||` 、`! (类型断言)` 都可去除变量中的 `null` 或 `undefined`

```js
function fn1(arg: number | null | undefined): number {
  if (arg == null) {
    return -1
  }
  return arg
}

function fn2(arg: number | null | undefined): number {
  return arg || -1
}

function fn3(arg: string | null | undefined): number {
  return arg!.length  // 类型断言 arg! 去除了 undefined 和 null
}

console.log(fn1(undefined), fn1(null))  // -1 -1
console.log(fn2(undefined), fn2(null))  // -1 -1
```

### 9.4 类型别名

使用 `type` 可以声明一个别名，别名和接口类似， 但是可以作用于原始类型、联合类型、交叉类型、元组等，但不能被 `extends` 或 `implement`

```js
type combine = number | string

function fn(arg: combine): number {
  if(typeof arg === 'string') {
    return arg.length
  }
  return -1
}
```

## 10. 模块

### 10.1 export = & import = require()

为了支持 CommonJS 和 AMD 的 `exports` ,TypeScript 提供了 `export = ` 语法导出，然后使用 `import module = require()` 导入

```js
export = {name: 'TangBii'}
```

```js
import module1 = require('./module1')
```

## 11. 命名空间

使用 `namespace` 可以声明一个命名空间 (内部模块)，在命名空间中使用 `export` 导出，在命名空间外使用 `命名空间.导出名` 访问

```js
namespace Person {
  export interface IStudent {
    name: string;
    score: number
  }

  export interface ITeacher {
    name: string;
    course: string
  }
}

const s: Person.IStudent = {
  name: 'TangBii',
  score: 10
}

const t: Person.ITeacher = {
  name: 'self',
  course: 'front-end'
}
```



