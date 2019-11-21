---
  layout: post
  title: "TypeScript"
  description: "TypeScript"
  category: [TypeScript]
  tags: [TypeScript]
---

## TypeScript
[官方文档](https://www.tslang.cn/docs/handbook/basic-types.html)

#### 基础类型

##### 介绍

ts支持与js相同的数据类型，此外还提供了枚举类型方便使用

##### 布尔值
```typescript
let isDone:boolean = false
```
##### 数字
ts中的数字都为浮点数，类型都为number。除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。
```typescript
let decLiteral:number = 6;
let hexLiteral:number = 0xf00d;
let binaryLiteral:number = 0b1010;
let octLiteral:number = 0o744;
```
##### 字符串
```typescript
let name:string = "bob";
name = "smith"
```
还可以使用模板字符串
```typescript
let name: string = `Gene`;
let age: number = 37;
let sentence:string = `
Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.
`
```
##### 数组
ts有两种方式可以定义数组
```typescript
let arr1: number[] = [0,1,2,3] // 元素类型后面跟[]
let arr2: Array<number> = [4,5,6,7] // 使用数组泛型，Array<元素类型>
```
##### 元组 Tuple
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。比如，你可以定义一对值分别为string和number类型的数组。
```typescript
let x: [string, number];

x = ['hello', 10]
x = [10, 'hello'] // error
// 当访问一个已知索引的元素，可以得到正确的类型
console.log(x[0].substr(1))
// 当访问一个越界元素，会使用联合类型替代
x[3] = 'world'
console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString
x[6] = true; // Error, 布尔不是(string | number)类型
```
==联合类型==

##### 枚举

enum是对js标准数据类型的一个补充
```typescript
enum Color {red, green, Blue}
let c:Color = Color.Green;

enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];
console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```
##### Any
```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

Any类型与Object类型区别：Object类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法
```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```
##### Void
与any类型相反，表示没有任何类型，当一个函数没有返回值时，返回值类型为void
```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```
声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：

```typescript
let unusable: void = undefined;
```

##### Null和Undefined
```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```
默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

##### Never
never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

##### Object
表示非原始类型，除number string boolean null undefined symbol之外的类型


##### 类型断言

```typescript
// 两种形式，一种是尖括号，另一种是as语法
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

##### 联合类型
表示取值可以为多种类型中的一种
```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
