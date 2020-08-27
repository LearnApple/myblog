---
title: Typescript-声明文件
date: 2019-11-22 10:31:23
tags: [Typescript]
description: 当使用第三方库时，我们需要引用它的声明文件，才能获得相应的代码补全、接口提示等功能。
---

#### 声明文件

##### 新语法索引

* `declare var` 声明全局变量
* `declare function` 声明全局方法
* `declare class` 声明全局类
* `declare enum` 声明全局枚举类型
* `declare namespace` 声明（含有子属性的）全局对象
* `interface` 和 `type` 声明全局类型
* `export` 导出变量
* `export namespace` 导出（含有子属性的）对象
* `export default` ES6 默认导出
* `export = commonjs` 导出模块
* `export as namespace` UMD 库声明全局变量
* `declare global` 扩展全局变量
* `declare module` 扩展模块
* /// `<reference />` 三斜线指令

#### 什么是声明语句
假如我们想使用第三方库jQuery，一种常见的方式是在html中通过script标签引入jQuery，然后就可以使用全局变量$或jQuery了。

```typescript
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```
#### 什么是声明文件

通常将声明语句单独放到一个单独文件中，这就是声明文件，必须与`.d.ts`为后缀。
##### 第三方声明文件

社区已有jQuery的声明文件了，可直接下载使用：[jQuery in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/jquery/index.d.ts)
更推荐的是 使用@types统一管理第三方库的声明文件。
@types的使用方式很简单，如下所示：
```typescript
npm install @types/jquery --save-dev
```

#### 书写声明文件

