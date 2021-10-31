---
title: TypeScript学习
date: 2021-06-27 20:15:15
tags:
cover: /img/article/typescript/typescript.png
top_img: /img/article/typescript/typescript.png
description: 说实话没学明白...
---

# TypeScript 学习

## 安装`TypeScript`

全局安装`TypeScript`：

```
yarn global add typescript
```

全局安装`ts-node`，用于快速编译 ts 文件：

```
yarn global add ts-node
```

## 静态类型的理解

定义变量时，首先定义其类型，在后续的使用中，也只能用这种类型的数据赋值给变量。

## 基础类型

基础类型也可以叫做原始类型，与 JavaScript 中的原始类型类似。typescript 中的原始类型有：

- 布尔值
- 数字
- 字符串
- Null
- Undefined
- Symbol
- BigInt

其中，`Symbol`和`BigInt`为 ES6 中新增的基础类型。

```ts
const isTure: Boolean = false
const count: number = 123
const animal: string = 'Cat'
const notExist: undefined = undefined
const empty: null = null
const aSymbol: Symbol = Symbol('id')
const bigBumber: BigInt = BigInt(10)
```

除了上述类型，typescript 中还有一个空值`void`，在函数中，可以表示没有任何返回值：

```ts
function func(): void {
  console.log('empty!')
}
```

`undefined`与`null`类型是所有类型的子类型，也就是说，定义变量时可以将初始值赋值为`undefined`与`null`，而`void`就不行。

```ts
let count: number = undefined

let un: undefined
let num: number = un

let v: void
let number: number = v // error
```

## 对象类型

在 typescript 中，使用接口`interface`来定义对象的类型。

```ts
interface People {
  name: string
  age: number
}

const nick: People = {
  name: 'nick',
  age: 18,
}
```

接口定义的值，在使用中是不可以出现缺少或者多余的字段名：

```ts
interface People {
  name: string
  age: number
}

const nick: People = {
  name: 'nick', // error
}

const tom: People = {
  name: 'tom',
  age: 18,
  gender: 'man',
}
```

若是需要一个可选的属性，只需在字段名后面加一个问号`?`，但是使用时，同样不可以出现多余属性：

```ts
interface People {
  name: string
  age: number
  gender?: string
}

const nick: People = {
  name: 'nick',
  age: 18,
}
```

## 类型注解和类型推断
