---
title: Typescript-函数
date: 2019-11-21 17:42:51
tags: [Typescript, js]
categories: Typescript,
description: 函数是JavaScript应用程序的基础。 它帮助你实现抽象层，模拟类，信息隐藏和模块。 在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 行为的地方。 TypeScript为JavaScript函数添加了额外的功能，让我们可以更容易地使用。
---

### 函数

#### 函数
ts可创建有名字的函数和匿名函数，同js。

```javascript
// Named function
function add(x, y) {
    return x + y;
}

// Anonymous function
let myAdd = function(x, y) { return x + y; };

```
#### 函数类型
我们可以给每个参数添加类型之后再为函数本身添加返回值类型。 TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };
```
完整的函数类型：
函数类型包含两部分：参数类型和返回值类型。以参数列表的形式写出参数类型，为每个参数指定一个名字和类型。函数和返回值类型之间用=>符号。如果函数没有任何返回值，则必须指定为void，而不能留空。
```typescript
let myAdd: (baseValue: number, increment: number) => number =
    function(x: number, y: number): number { return x + y; };
```
函数的类型只是由参数类型和返回值组成的。 函数中使用的捕获变量不会体现在类型里。 实际上，这些变量是函数的隐藏状态并不是组成API的一部分。

##### 推断类型

，你会发现如果你在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型：
```typescript
// myAdd has the full function type
let myAdd = function(x: number, y: number): number { return x + y; };

// The parameters `x` and `y` have the type number
let myAdd: (baseValue: number, increment: number) => number =
    function(x, y) { return x + y; };
```
这叫做 按上下文归类，是类型推论的一种

#### 可选参数和默认参数
传递给一个函数的参数个数必须与函数期望的参数个数一致。JavaScript里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是undefined。 在TypeScript里我们可以在参数名旁使用 ?实现可选参数的功能。可选参数必须跟在必须参数后面。
有默认初始化值的参数，在所有必须参数后面的带默认初始化值的参数都是可选的。可以放在必传参数前面，如果要获得默认值，则必须传undefined。
```typescript
function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // okay and returns "Bob Adams"
let result4 = buildName(undefined, "Adams");     // okay and returns "Will Adams"
```

#### 剩余参数
js中可以用arguments访问所有传入的参数
剩余参数被当做个数不限的可选参数。
```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```
#### this

[this是如何工作的-Yehuda Katz](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/)
call方法
```javascript
function hello(thing) {
  console.log(this + " says hello " + thing);
}

hello.call("Yehuda", "world") //=> Yehuda says hello world
```
##### this和箭头函数
js里this的值在被调用的时候才会被指定。

##### this参数

##### this参数在回调函数里

#### 重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
同一个函数提供多个函数类型定义来进行函数重载。

注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。