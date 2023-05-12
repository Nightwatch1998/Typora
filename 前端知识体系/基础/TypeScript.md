## 基础类型

- 布尔值 boolean
- 数字 number
- 字符串 string 
- 数组 

```typescript
let list: number[] = [1,2,3];
let list: Array<number> = [1,2,3];
```

- 元组tuple，已知类型的数量的数组

```typescript
let x: [string, number];
```

- 枚举 enum 编号访问属性，属性访问编号

```typescript
enum Color {Red, Green, Blue};
```

- 任意any 不能确定类型时

```typescript
let notSure: any = 4;
notSure = "saf"
notSure.toFixed()

let prettySure:Object = 4;
// prettySure.toFixed()
```

- object表示**非原始类型**

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
```

- 返回值类型Void，只能赋予undefined、null
- never 永远不存在的值，返回nerver的函数存在无法到达的终点

```typescript
function error(msg:string):never{
    throw new Error(msg)
}
```

- 类型断言

```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
let strLength1: number = (someValue as string).length;
```

## 变量声明

- 同级作用域不可重复声明
- 块级作用域声明后，会覆盖父级作用域的声明

## 接口(静态与实例)

对值所具有的的结构进行类型检查，描述对象的**各种外形**

- 一个接口可以继承多个接口

**接口初探**

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
  readonly x: number; // 只读
  [propName: string]: any; // 任意数量其他属性
}

// 只读数组
let list: ReadonlyArray<number> = a;
```

`readonly `VS `const`

- `readonly` 用于属性
- `const` 用于变量

**函数类型**

```typescript
// 普通函数
interface SearchFunc {
  (source: string, subString: string): boolean;
}

// 构造函数
interface ClockConstructor {
    new (hour: number, minute: number);
}
```

**可索引的类型**

```typescript
interface StringArray {
  [index: number]: string;
}
```

ts支持数字和字符串的`索引签名`

**类类型**

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

// 实现接口
class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

**混合类型**

```typescript
// 既可以作为对象，也可以作为函数
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    // 类型断言
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}
```

**接口继承类**

会继承类成员，但不包括类实现

当创建了一个接口继承了一个拥有私有或保护的成员的类时，这个接口类型只能由这个类及其子类所实现

## 类

在TS中，类是一种特殊的对象类型，具有一个构造函数。使用new进行实例化的时候，实际上是调用了这个构造函数

```typescript
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    // 一定要调用super
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}
let sam = new Snake("Sammy the Python");
sam.move()
```

类成员的修饰符：

- public **默认**，声明它的类，实例，派生类都可以访问
- private 只能由声明它的类访问
- protected 派生类可以访问

抽象类：

- abstract 关键字，可以包含成员的实现细节
- 可以创建引用，但不能被实例化
- 继承的子类可以实例化

```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

## 函数

- TS里每个函数参数都是必须的
- 使用?实现参数可选的功能。放在必须参数后面 
- 支持函数重载，把最精确的定义放在最前面 (丑陋)

## 泛型

### 泛型函数

```typescript
function identity<T>(arg: T): T {
    return arg;
}

// 传入参数和类型参数
let output = identity<string>("myString");
// 类型推断
let output = identity("myString")
```

### 泛型接口

```typescript
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

### 泛型类

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

静态属性不能使用泛型类型

### 泛型约束

```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // no more error
    return arg;
}
```

### 泛型中使用类类型

```typescript
// c是一个对象，具有一个无参的构造函数，c就相当于类
function create<T>(c: {new(): T; }): T {
    return new c();
}
// c是无参的构造函数类型
function create<T>(c: new ()=>T): T {
    return new c();
}
```

## 枚举

使用枚举类型定义一些**带名字**的常量：

- 通过枚举的属性访问枚举成员
- 通过枚举的名字访问枚举类型

### 数字枚举

- 默认从0开始递增，也可以指定

```typescript
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```

### 字符串枚举

```typescript
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
```

### 计算枚举

计算枚举会在编译时求值

```typescript
enum FileAccess {
    // constant members
    None,
    Read    = 1 << 1,
    Write   = 1 << 2,
    ReadWrite  = Read | Write,
    // computed member
    G = "123".length
}
```

### const枚举

只能使用常量枚举表达式

## 类型推论

类型是在哪里如何被推断的

- 最佳通用类型：初始化变量和成员的时候
- 上下文类型：表达式的类型与所处位置相关
- 联合类型

## 类型的兼容性

TS的类型兼容性是基于**结构子类型**的，只是用成员描述类型

注：基于**名义类型**的类型系统，数据类型的兼容性是通过明确声明或类型名称决定的

基本规则，如果x要兼容y，那么y至少具有与x相同的属性

```typescript
interface Named {
    name: string;
}

let x: Named;
// y's inferred type is { name: string; location: string; }
let y = { name: 'Alice', location: 'Seattle' };
x = y;
```

检查函数参数时使用相同的规则

```typescript
function greet(n: Named) {
    console.log('Hello, ' + n.name);
}
greet(y); // OK
```

### 比较两个函数

**参数不同**

```typescript
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // OK y兼容了x
x = y; // Error x不能兼容y
```

**返回值不同**

```typescript
let x = () => ({name: 'Alice'});
let y = () => ({name: 'Alice', location: 'Seattle'});

x = y; // OK
y = x; // Error, because x() lacks a location property
```

`系统强制要求源函数的返回值类型必须是目标函数返回值的子类型`

### 函数参数双向协变

协变性是指，当一个函数的参数类型与另一个函数的参数类型一致或者是其子类型时，这两个类型是兼容的。

```typescript
enum EventType { Mouse, Keyboard }

interface Event { timestamp: number; }
interface MouseEvent extends Event { x: number; y: number }
interface KeyEvent extends Event { keyCode: number }

function listenEvent(eventType: EventType, handler: (n: Event) => void) {
    /* ... */
}

// Unsound, but useful and common
listenEvent(EventType.Mouse, (e: MouseEvent) => console.log(e.x + ',' + e.y));

// Undesirable alternatives in presence of soundness
listenEvent(EventType.Mouse, (e: Event) => console.log((<MouseEvent>e).x + ',' + (<MouseEvent>e).y));
listenEvent(EventType.Mouse, <(e: Event) => void>((e: MouseEvent) => console.log(e.x + ',' + e.y)));

// Still disallowed (clear error). Type safety enforced for wholly incompatible types
listenEvent(EventType.Mouse, (e: number) => console.log(e));
```

TS不允许返回值类型的协变性

### 枚举类型

枚举类型与数字类型兼容，不同的枚举类型不兼容

### 类类型

类的兼容性只比较实例部分，private和protected会影响兼容性

```typescript
class Animal {
    feet: number;
    constructor(name: string, numFeet: number) { }
}

class Size {
    feet: number;
    name: string;
    constructor(numFeet: number) { }
}

let a: Animal;
let s: Size;

a = s;  // OK
s = a;  // error
```

### 泛型

不包含成员使用泛型类型的变量时是兼容的

```typescript
interface Empty<T> {
}
let x: Empty<number>;
let y: Empty<string>;

x = y;  // OK, because y matches structure of x
```

当添加了成员变量：

```typescript
interface NotEmpty<T> {
    data: T;
}
let x: NotEmpty<number>;
let y: NotEmpty<string>;

x = y;  // Error, because x and y are not compatible
```

## 高级类型

### 交叉类型

类似于融合怪，将多个类型合并为一个类型。多用在mixin中

**交叉类型可以使用所有类型的成员**

```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{}; // 交叉类型
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}
```

### 联合类型

联合类型只能使用所有类型**共有的成员**

```typescript
function padLeft(value: string, padding: string | number) {
    // ...
}

let indentedString = padLeft("Hello world", true); // errors during compilation
```

### 类型保护与区分类型

- typeof：number、string、boolean、symbol
- instanceof：构造函数
- 可以为null的类型
  - `--strictNullChecks` //声明变量时不会自动包含null和undefined
  - 可选属性会被自动添加|undefined

```typescript
let s = "foo";
s = null; // 错误, 'null'不能赋值给'string'
let sn: string | null = "bar"; //使用联合声明
sn = null; // 可以
```

- 类型断言`!`后缀，去除null和undefined

### 类型别名

给类型起个名字，可以用于原始值、联合类型、元组和自定义类型

**type 操作符**

```typescript
type Name = string; // 字符串
type NameResolver = () => string; // 函数
type NameOrResolver = Name | NameResolver; // 联合类型
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    }
    else {
        return n();
    }
}
type Container<T> = { value: T }; // 泛型
type Tree<T> = {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
} //属性中引用自身

```

### 索引类型

**keyof T索引类型查询操作符**

**T[K]索引访问操作符**

```typescript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

interface Person {
    name: string;
    age: number;
}
let person: Person = {
    name: 'Jarid',
    age: 35
};
let strings: string[] = pluck(person, ['name']); // ok, string[]
```

### 映射类型

从旧类型中创建新类型

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Partial<T> = {
    [P in keyof T]?: T[P];
}
```

## symbol

可以与计算出的属性名声明相结合

```typescript
const getClassNameSymbol = Symbol();

class C {
    [getClassNameSymbol](){
       return "C";
    }
}

let c = new C();
let className = c[getClassNameSymbol](); // "C"
```

内置的symbols:

- `Symbol.hasInstance`被`instanceof`运算符调用
- `Symbol.iterator` 被`for-of`调用
- `Symbol.(match|replace|search|split)` 正则处理字符串

## 模块

模块在自身作用域执行，定义在模块里的变量、函数、类在模块外部是不可见的，除非export导出

为了支持CommonJS和AMD的exports，使用`export =`导出模块，

必须用`import module = require("module")`导入模块

### 导出

```typescript
//ZipCodeValidator.ts
export interface StringValidator {
    isAcceptable(s: string): boolean
}
export const numberRegexp = /^[0-9]+$/;
class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string): boolean {
        return s.length === 5 && numberRegexp.test(s)
    }
}
export {ZipCodeValidator};
export {ZipCodeValidator as mainValidator}



// ParseIntBasedZipCodeValidator.ts
export class ParseIntBasedZipCodeValidator {
    isAcceptable(s: string){
        return s.length === 5 && parseInt(s).toString()===s
    }
}
// 重新导出 扩展其他模块，并不会在当前模块导入那个模块
export {ZipCodeValidator as RegExpBasedZipCodeValidator} from "./ZipCodeValidator"

// 默认导出 JQuert.d.ts
export default $;
```

### 导入

```typescript
// 导入模块的部分导出内容
import {ZipCodeValidator} from "./ZipCodeValidator"
let myValidator = new ZipCodeValidator()

// 使用别名
import {ZipCodeValidator as ZCV} from "./ZipCodeValidator"
let myValidator = new ZCV();

// 将整个模块导入到一个变量
import * as validator from "./ZipCodeValidator"
let myValidator = new validator.ZipCodeValidator()

// 导入默认导出，不需要括号
import $ from "Jquery"
```

### 生成模块代码

CommonJS和AMD模块都有一个`exports`变量，这个变量包含了模块的所有导出内容

- **痛点**：`export default`并不兼容`exports`

- **解决办法**：提供`export=`语法导出模块，并用`import module = require("module")`导入模块

### declare关键字

declare用于告诉编译器某个标识符的类型信息，帮助编译器进行类型检查

通常在一个独立的`.d.ts`文件中定义

```typescript
// 声明全局变量、全局函数
declare const myVar:string;
declare function myFunc(str:string):void;

// 声明模块和命名空间
declare module "my-module"{}
declare namespace MyNamespace {}

// 声明类、接口、类型别名
declare class MyClass {}
declare interface MyInterface {}

declare type MyType = string | number
```

有了模块的类型声明文件

```typescript
/// <reference path="node.d.ts"/>
// import * as URL from "url";
import url = require("url")
let myUrl = URL.parse("http://www.typescriptlang.org");
```

### 创建模块结构指引

- 尽可能在顶层导出
- 导出单个`class`或`function`，使用`export default`
- 导出多个对象，放在顶层导出
- 明确列出导入的名字
- 导出大量内容的时候使用命名空间导出
- 模块里不要使用命名空间

## 命名空间

命名空间是TS中组织代码的方式，将相关代码封装在一个单独的命名空间中。可以定义类、接口、函数等等，避免**命名冲突**和**代码污染**问题。

`namespace`关键字和`module`关键字：

- TS早期使用`module`关键字定义内部模块，TS2.0之后推荐使用`namespace`定义命名空间
- `namespace`适用于不支持ES6模块的环境
- ES6环境使用`import`、`export`定义模块即可

### 分离到多个文件

尽管在不同的文件，仍然是同一个命名空间。使用引用标签告诉编译器文件之间的关联。

**优点是不用在文件之间引入依赖关系**

### 别名

```typescript
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;
let sq = new polygons.Square(); // Same as "new Shapes.Polygons.Square()"
```

## 模块解析

模块解析是指编译器在查找导入模块内容时所遵循的流程

- 相对导入：解析时相对于导入它的文件，**不能是外部的模块声明**，通常用于自己写的模块
- 非相对模块：相对于baseUrl或路径映射进行解析，可以使外部模块声明

模块解析策略：使用`--moduleResolution`标记

- nodejs解析策略
  - 相对模块
    1. 检查目标模块`/root/src/moduleB.js`是否存在
    2. 检查`/root/src/moduleB`是否包含`package.json`，并且指定了`main`模块
    3. 检查`/root/src/moduleB`是否包含`index.js`文件
  - 非相对模块
    - 在`node_modules`目录查找，按照相对模块的**3步**
    - 递归向上查找`node_modules`
- classic策略(ts旧版解析策略)： 相对模块先查找`.ts`在查找`.d.ts`，非相对模块会递归向上查询

### typescript解析模块

模仿NodeJs解析策略，添加了.ts、.tsx、.d.ts：

1. `/root/src/moduleB.ts`
2. `/root/src/moduleB.tsx`
3. `/root/src/moduleB.d.ts`
4. `/root/src/moduleB/package.json` (如果指定了`"types"`属性)
5. `/root/src/moduleB/index.ts`
6. `/root/src/moduleB/index.tsx`
7. `/root/src/moduleB/index.d.ts`

### 路径映射

- 配置paths指定对baseurl的映射，可以为多个
- 利用rootDirs指定虚拟目录

### 跟踪模块解析

使用`--traceResolution`调用编译器。

## 声明合并

编译器针对两个同名的声明，合并为单一的声明

- **接口合并**后，后面的接口具有更高的优先级，单一字符串字面量会提升
- 命名空间的合并
  - 模块导出的同名接口会合并
  - 命名空间的导出成员会被添加到已存在的模块里
- 命名空间与类、函数、枚举合并可以进行扩展
- 模块扩展

```typescript
// observable.ts stays the same
// map.ts
import { Observable } from "./observable";
declare module "./observable" {
    interface Observable<T> {
        map<U>(f: (x: T) => U): Observable<U>;
    }
}
Observable.prototype.map = function (f) {
    // ... another exercise for the reader
}


// consumer.ts
import { Observable } from "./observable";
import "./map";
let o: Observable<number>;
o.map(x => x.toFixed());
```

## JSX(学习React后补充)

## 装饰器(实验性)

通过元编程的方式为类的声明提供标注

**原理**：运行时，装饰器被解析成特殊的函数调用，会被添加到原型链或作为闭包的一部分被保存，当运行时访问到被装饰的代码结构时，这些特殊的函数就会被调用。

tsconfig.json

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true //启用装饰器
    }
}
```

**装饰器组合**：从上至下依次求值，求值的结果作为函数，由下至上依次调用

```typescript
@f @g x //同一行

// 多行
@f
@g
x
```

### 类装饰器

类装饰器会在运行时被当做函数调用，类的**构造函数是其唯一参数**

如果类装饰器返回一个值， 这个值会被用来**替换原来的类声明**

```typescript
@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

function sealed(constructor: Function) {
    Object.seal(constructor); // 密封构造器
    Object.seal(constructor.prototype); // 密封构造器的原型
}
```

### 方法装饰器

三个参数：

- 对于静态对象来说是类的构造函数，实例成员是类的原型对象
- 成员名字
- **成员的属性描述符**

如果返回一个值，这个值会作为方法的属性描述符

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    @enumerable(false)
    greet() {
        return "Hello, " + this.greeting;
    }
}

function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}
```

### 访问器装饰器

和方法的类似

### 属性装饰器

用于监视类中是否声明了某个名字的属性

2个参数：

- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。

- 成员的名字

### 参数装饰器

监视方法的参数是否被传入

- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
- 成员的名字。
- 参数在函数参数列表中的索引。

参数装饰器的返回值会被忽略

## Mixis

将一个类混入另一个类，需要手写混入函数

## 三斜线指令

包含单个XML标签的单行注释，注释的内容会作为**编译器指令**使用

```typescript
/// <reference path="..." />  用于声明文件间的依赖
/// <reference types="..." /> 用于引用一个模块的声明文件，写一个.d.ts时使用
/// <reference no-default-lib="true"/> 标记为默认库
```

