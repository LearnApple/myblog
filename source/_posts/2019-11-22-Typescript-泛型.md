---
title: Typescript-泛型
date: 2019-11-22 16:57:25
tags: [Typescriot]
categories: Typescript
---

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("myString");  // type of output will be 'string'

// 编译器可根据类型推论确定返回值的类型
let output = identity("myString");  // type of output will be 'string'
```
#### 使用泛型变量
```typescript
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // Array has a .length, so no more error
    return arg;
}
```
#### 泛型类型
```typescript
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```
#### 泛型类

```typescript
let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = "";
stringNumeric.add = function(x, y) { return x + y; };

console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```
类有两部分：静态部分和实例部分。 泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。
#### 泛型约束
