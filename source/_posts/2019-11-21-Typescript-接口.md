---
title: Typescript-接口
date: 2019-11-21 11:46:41
tags: [Typescript]
description: "在TypeScript中，使用接口(interfaces)来定义对象的类型"
category: [TypeScript]
---

### 接口

#### 介绍
在面向对象的语言中，接口是一个很重要的概念，是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。

TypeScript的核心原则之一是对值所具有的结构进行类型检查。被称为“鸭式辨型法”或“结构性子类型化”。在ts里，==接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约==。除了可用于对类的一部分行为进行抽象，还可以对对象的形状进行描述。

接口一般首字母大写，定义的变量比接口少一些属性和多一些属性都是不允许的。
```typescript
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```
#### 可选属性

接口里的可选属性不全是必须的，可选属性需要在属性名后加`?`
```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({color: "black"});
```
需要注意的是，==一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：==

#### 只读属性
一些对象属性只能在对象刚创建的时候修改其值。用`readonly`来指定只读属性：
```typescript
interface Point {
    readonly x: number;
    readonly y: string;
}
```
TypeScript具有`ReadonlyArray<T>`类型，它与`Array<T>`相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：
```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

上面代码的最后一行，可以看到就算把整个`ReadonlyArray`赋值到一个普通数组也是不可以的。 但是你可以用类型断言重写：
```typescript
a = ro as number[];
```

##### `readonly` vs `const`
最简单的判断方式：readonly一般是在属性上使用，const是在变量上使用

#### 额外的属性检查
对象字面量会被特殊对待而且会经过 额外属性检查
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}
// 对象字面量会被特殊对待而且会经过 额外属性检查，
// 当将它们赋值给变量或作为参数传递的时候。 
//如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。
// error: 'colour' not expected in type 'SquareConfig'
let mySquare = createSquare({ colour: "red", width: 100 });
```
要绕开这些检查非常简单，最简便的方式就是==类型断言==。然后最佳方式是能够==添加一个字符串索引签名==，前提是你能够确定这个对象可能具有某些做为特殊用途使用的额外属性。 如果 SquareConfig带有上面定义的类型的color和width属性，并且还会带有任意数量的其它属性，那么我们可以这样定义它：
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```
最后一种方式，就是讲字面量对象赋值给另外一个变量。
```typescript
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
```

#### 函数类型

接口能够描述Js中对象拥有的各种各样的外形。除了带有属性的普通对象外，也可以描述函数类型。

为了使用接口表示函数类型，需要给接口定义一个调用签名。
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}

// 对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配
let mySearch: SearchFunc;
mySearch = function(src: string, sub: string): boolean {
  let result = src.search(sub);
  return result > -1;
}

//函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。 如果你不想指定类型，TypeScript的类型系统会推断出参数类型，因为函数直接赋值给了 SearchFunc类型变量。
let mySearch: SearchFunc;
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```
#### 可索引的类型
可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。
```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```
TypeScript支持两种索引签名：字符串和数字。可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 

可以将索引签名设置为只读，这样就防止了给索引赋值：
```typescript
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // error!
```

#### 类类型

##### 实现接口
与c#或Java里接口的基本作用一样，Ts也能够用它来明确地强制一个类去符合某种契约。
```typescript
interface ClockInterface {
    currentTime: Date;
}

class Clock implements ClockInterface {
    currentTime: Date;
    constructor(h: number, m: number) { }
}
// 也可以在接口中描述一个方法，在类里实现它，如下：
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```
接口描述了类的公共部分，而不是公共和私有两部分

##### ==类静态部分与实例部分的区别==
当一个类实现了一个接口时，只对其实例部分进行类型检查。 constructor存在于类的静态部分，所以不在检查的范围内。

#### 继承接口
和类一样，接口也可以相互继承。

```typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
一个接口可以继承多个接口，创建出多个接口的合成接口。
#### ==混合类型==
```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```
#### ==接口继承类==

https://www.tslang.cn/docs/handbook/interfaces.html
