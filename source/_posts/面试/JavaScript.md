---
title: 面试笔记-JavaScript
date: 2021-11-21 15:41:05
tags: 面试笔记
categories: 面试笔记
cover: /img/article/interview/interview.png
top_img: /img/article/interview/interview.png
description:
---

# JavaScript

## 变量类型

JavaScript 变量类型分为**值类型**，**引用类型**；值类型存放在栈中，引用类型存放在堆中。

其中值类型有 7 种：

- `String`
- `Boolean`
- `Number`
- `Null`
- `Undefined`
- `Symbol`
- `BigInt`

引用类型统称为`Object`，又可细分为以下 3 种：

- `Object`
- `Array`
- `Function`

### `typeof`

可以通过`typeof`关键字来判断一个变量的类型：

```js
typeof 'nick' // 'string'
typeof true // 'boolean'
typeof 123 // 'number'
typeof undefined // 'undefined'
typeof Symbol('symbol') // 'symbol'
typeof BigInt('100') // 'bigint'

typeof null // 'object'
typeof {} // 'object'
typeof [] // 'object'
typeof function () {} // 'function'
```

::: warning 注意！
`typeof`对于值类型的`null`会判断为`object`，其他的则是原始类型；对引用类型的`function`会判断为`function`，其他的则都为`object`。
:::

## 类型转换

### 字符串拼接

当 JavaScript 中变量遇到字符串时，会自动转换为字符串：

```js
var str = 123 + '00'
var str1 = 'str' + true
var str2 = 'str' + 'ing'

console.log(str) // '12300'
console.log(str1) // 'strtrue'
console.log(str2) // 'string'
```

### 相等和全等

使用相等符号`==`对变量进行比较时，会进行类型转换，然后确定两个变量是否相等。

在转换变量类型是，会按照以下规则进行转换：

- 如果任意变量为布尔值，则将其转换为数值再比较是否相等。false 转换为 0，true 转换为 1。
- 如果一个变量是字符串，另一个为数值，则尝试将字符串转换为数值，在比较是否相等。
- 如果一个变量为对象，另一个不是，则调用对象的 valueOf()方法获取其原始值，再根据前面的规则比较。
- `null`和`undefined`相等。
- `null`和`undefined`不能转换为其他类型在进行比较。
- NaN 与任何其他类型比较都返回 false，包括自己本身；不相等咋返回 true。
- 对象与对象比较，则会比较两个对象是否指向同一个对象的引用，不等则返回 false。

```js
null == undefined // true
false == 0 // true
true == 1 // true
true == '1' // true
100 == '100' // true
NaN == NaN // false
NaN != NaN // true
```

使用全等符号`===`进行比较时，变量不会进行类型转换；也就是说，数据类型不同，就返回 false。

```js
null === undefined // false
null === null // true
100 === '100' // false
'nick' === 'nick' // true
NaN === NaN // false
```

::: warning 注意
NaN 与任意值进行全等比较依然返回 false，包括自身。
:::

### if 条件，逻辑运算

使用到 if 语句以及逻辑运算时，条件内的值最终都会转换为布尔值。

## 原型和原型链

### `prototype`

在 JavaScript 中，每个函数都会创建一个`prototype`属性，这个属性是一个对象，里面的属性与方法可以在实例中共享。`prototype`属性是通过调用构造函数穿件的对象的原型；也就是说，构造函数的原型就是`prototype`属性。

```js
function Person() {}

Person.prototype.name = 'nick'
Person.prototype.age = 18
Person.prototype.sayHi = function () {
  console.log(this.name, this.age)
}

let p1 = new Person()
p1.sayHi() // 'nick' 18
console.log(p1.name, p1.age) // 'nick'  18

console.log(Person.prototype) // Person { name: 'nick', age: 18, sayHi: [Function (anonymous)] }
```

### `__proto__`

实例对象查找属性时会从自身开始查找，如果有，则直接使用；如果没有，则会通过`__proto__`找到构造对象的原型中查找。

```js
function Person() {}

Person.prototype.name = 'nick'

let p1 = new Person()
p1.age = 18

console.log(p1.name, p1.age) // nick 18
console.log(p1.__proto__ === Person.prototype) // true
```

### `constructor`

构造函数的`prototype`和该构造函数的实例对象的`__proto__`都有一个`constructor`属性，默认都指向该构造函数本身。

```js
function Person() {}

let p1 = new Person()

console.log(Person.prototype.constructor === p1.__proto__.constructor) // true
```

### 构造函数的`prototype`的`__proto__`

构造函数的`prototype`也有`__proto__`属性，普通构造函数原型上的`__proto__`指向特殊构造函数`Object`的原型；而`Object`原型上的`__proto__`指向`null`。

也就是说，实例对象查找属性时会从自身开始查找，如果没有，则会通过`__proto__`一直查找原型中有没有需要的属性，有的话就取出，没有的话，就一直到`null`，返回`undefined`。

```js
function Person() {}
Person.prototype.name = 'nick'

let p1 = new Person()
p1.age = 18

console.log(p1.name, p1.age, p1.hobby) // 'nick' 18 undefined
```

### 构造函数的`__proto__`

所有构造函数都是内置构造函数`Function`的实例，所以构造函数的`__proto__`都指向`Function`的原型，包括`Function`自身的`__proto__`，也是指向自己的原型。

```js
function Person() {}

console.log(Person.prototype.__proto__ === Object.prototype) // true
console.log(Person.__proto__ === Function.prototype) // true
console.log(Function.__proto__ === Function.prototype) // true
```

### `instanceof`基于原型链实现

`instanceof`运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。也可以理解为构造函数的`prototype`属性是否与实例的`__proto__`相等。

::: tip
实例对象的`__proto__`指向创建它的构造函数的原型。
构造函数原型的`__proto__`指向`Object`的原型。
构造函数都是`Function`的实例。
:::

## 作用域与闭包

### 作用域

JavaScript 中有三种作用域：**全局作用域**，**函数作用域**，**块级作用域**。

#### 全局作用域

顾名思义，定义在全局中的变量所处的作用域环境就是全局作用域。全局作用域中的变量可以被任意访问。

```js
let name = 'nick'

function getName() {
  console.log(name)
}

getName() // nick
console.log(name) // nick
```

### 函数作用域

顾名思义，所处环境在函数中。函数作用域只能被当前作用域及内部作用域访问，无法被外部作用域访问。

```js
function foo() {
  var name = 'nick'
  console.log(name)
}
foo() // nick
console.log(name) // ReferenceError: name is not defined
```

### 块级作用域

包含在代码块`{}`中的变量，所处环境就为块级作用域。块级作用域只能被当前作用域内访问，不能被外部访问(var 除外)。

```js
{
  var name = 'nick'
  var age = 18

  let hobby = 'run'
  let eat = 'rice'

  console.log(name, age, bobby, eat) // 'nick' 18 run rice
}

console.log(name, age, hobby, eat) // ReferenceError: hobby is not defined
```

### 闭包

闭包是指能够访问自由变量的函数，而自由变量则是值在当前函数作用域中未定义，但是却被使用的变量。

```js
var name = 'nick'

function getName() {
  console.log(name)
}

getName() // 'nick'
```

对于自由变量的查找，是在函数定义的地方查找，而不是执行时。

```js
var name = 'nick'

function getName() {
  console.log(name)
}

function getOtherName() {
  var name = 'mike'
  getName()
}

getOtherName() // 'nick'
```

一般来说，有两种情况通常形成闭包：

- 函数作为返回值
- 函数作为参数

并且上面两种情况的函数都保留了对外部作用域中变量的引用，这就形成了闭包。

#### 闭包的使用场景

##### setTimeout

原生的`setTimeout`函数的第一个参数不能携带参数，可通过闭包实现。

```js
function getName(name) {
  return function () {
    console.log(name)
  }
}

let func = getName('nick')

setTimeout(func, 1000)
```

##### 封装私有变量

```js
function ownerVarible() {
  let count = 0

  return {
    increment: function () {
      return (count += 1)
    },
  }
}

let result = ownerVarible()

console.log(result.increment()) // 1
console.log(result.increment()) // 2
console.log(result.increment()) // 3
```

## JavaScript 创建对象方式

### 对象字面量

简单粗暴

```js
let person = {
  name: 'nick',
  age: 18,
}
```

### 工厂模式

工厂模式的主要工作原理是用函数来封装创建对象的细节，从而通过调用函数来 达到复用的目的。

```js
function createPerson(name, age) {
  let o = new Object()
  o.name = name
  o.age = age
  o.sayHi = function () {
    console.log(this.name, this.age)
  }

  return o
}

let p1 = createPerson('nick', 18)
let p2 = createPerson('mike', 20)

p1.sayHi() // 'nick' 18
p2.sayHi() // 'mike' 20
```

工厂模式能够解决创建多个类似对象的问题，但是没有解决对象标识问题，无法反应新创建的对象是什么类型。

### 构造函数模式

JavaScript 中每一个函数都可以作为构造函数，只要一个函数是通过 `new` 来调用的， 那么我们就可以把它称为构造函数。执行构造函数首先会创建一个对象，然后将对象的原型指向构造函数 的 `prototype` 属性，然后将执行上下文中的 `this` 指向这个对象，最后再执行整个函数，如果返回值不是 对象，则返回新建的对象。

```js
function Person(name, age) {
  this.name = name
  this.age = age
  this.sayHi = function () {
    console.log(this.name, this.age)
  }
}

let p1 = new Person('nick', 18)
let p2 = new Person('mike', 20)

p1.sayHi() // 'nick' 18
p2.sayHi() // 'mike' 20
```

构造函数模式和工厂模式大致相同，但是有以下几点不同：

::: warning
没有显示地创建对象。
属性和方法直接赋值给了`this`。
没有`return`。
:::

构造函数解决了对象类型识别的问题，但是内部定义的方法会在每个实例上都重新创建一遍，造成浪费。

### `Object.create`

该方法非常有用，因为它允许你为创建的对象选择一个原型对象，而不用定义构造函数，并且使用现有的对象来作为新创建对象的`__proto__`。

```js
const person = {
  name: 'nick',
  age: 18,
  sayHi: function () {
    console.log(this.name, this.age)
  },
}

let p1 = Object.create(person)

p1.name = 'mike'
p1.age = 20

p1.sayHi() // 'mike' 20
person.sayHi() // 'nick' 18

console.log(p1.__proto__ === person) // true
```

### 原型模式

每一个函数都有一个 `prototype` 属性，这个属性是一个对象，它包含了通过构造函数创建的所有实例都能共享的属性和方法。因此我们可以使用原型对象来添加公用属性和方法，从而实现代码的复用。

```js
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.sayHi = function () {
  console.log(this.name, this.age)
}

let p1 = new Person('nick', 18)
let p2 = new Person('mike', 20)

p1.sayHi() // 'nick' 18
p2.sayHi() // 'mike' 20
```

原型模式中，动态属性可以放在构造函数内，通过参数获取；共用的方法可以放在原型上，在实例间共享，重复使用。

但是原型模式的问题也在于所有属性都能够在实例之间共享。这似乎没什么问题，但是原型中若是有引用类型的属性，则会造成混乱。

## 继承

### 原型链继承

主要继承方式。通过原型继承多个引用类型的属性和方法。

```js
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.sayHi = function () {
  console.log(this.name, this.age)
}

function Student(school) {
  this.school = school
}

Student.prototype = new Person('nick', 18)

let s1 = new Student('w3c')
s1.sayHi() // 'nick' 18
console.log(s1.school) // 'w3c'
```

原型链继承的缺点和之前通过原型模式创建对象一样，属性在实例间共享的问题。若是引用类型的属性，对其修改后，会影响到其他实例的属性。

```js
function BaseColors() {
  this.colors = ['red', 'yellow', 'blue']
}

function LinkBaseColors() {}

LinkBaseColors.prototype = new BaseColors()

let color1 = new LinkBaseColors()
let color2 = new LinkBaseColors()

color1.colors.push('green')

console.log(color1.colors) // [ 'red', 'yellow', 'blue', 'green' ]
console.log(color2.colors) // [ 'red', 'yellow', 'blue', 'green' ]
```

还有一个问题是，无法在创建子类实例是给父类传递参数。

### 盗用构造函数

基本思路：在子类构造函数中调用父类构造函数，通过`apply`和`call`改变`this`指向的问题。

```js
function BaseColors() {
  this.colors = ['red', 'yellow', 'blue']
}

function LinkBaseColors() {
  BaseColors.apply(this)
}

let color1 = new LinkBaseColors()
let color2 = new LinkBaseColors()

color1.colors.push('green')

console.log(color1.colors) // [ 'red', 'yellow', 'blue', 'green' ]
console.log(color2.colors) // [ 'red', 'yellow', 'blue' ]
```

利用`call`或者`apply`可以在创建子类实例的时候，将父类`this`的值指向子类的实例；这样，就等于每个子类对象都有属于自己的属性，不会造成修改冲突。

盗用构造函数同样有缺点，也就是使用构造函数模式自定义类型时，必须在构造函数内定义方法，因此函数不能重用。并且，子类也无法访问父类原型上的方法，因此所有类型只能使用构造函数模式。

### 组合继承

组合继承也叫经典继承，结合了原型链与盗用构造函数的有点。基本思路是：使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。

```js
function Person(name) {
  this.name = name
  this.hobby = ['eat', 'run']
}

Person.prototype.sayName = function () {
  console.log(this.name)
}

Person.prototype.sayHobby = function () {
  console.log(this.hobby)
}

function Student(school, name) {
  this.school = school
  Person.call(this, name)
}

Student.prototype = new Person()

let s1 = new Student('w3c', 'nick')
let s2 = new Student('wc', 'mike')

s1.hobby.push('swim')
s1.sayName() // 'nick'
s1.sayHobby() // [ 'eat', 'run', 'swim' ]

s2.sayName() // 'mike'
s2.sayHobby() // [ 'eat', 'run' ]
```

组合继承将子类的原型改为父类的实例，并且在子类构造函数中改变父类构造函数的指向，使其指向子类的实例。这样，子类不仅可以使用父类原型上的方法，也可以拥有自己的属性，并且可以向父类传递参数。

### 寄生式继承

寄生式继承的思想是：接收一个对象，然后克隆这个对象，对其进行增强，然后返回增强后的对象。

```js
function createEnhanceObject(obj) {
  let clone = object(obj)
  clone.sayName = function () {
    console.log('Hi!')
  }

  return clone
}
```

### 寄生式组合继承

组合继承存在效率问题：父类构造函数始终都会被调用两次，一次是在创建子类原型时调用，一次是在子类构造函数中调用。

寄生式组合继承，在就利用到了寄生式继承的特点，通过一个增强函数做到只调用一次父类构造函数。

```js
function enhance(superType, subType) {
  let prototype = object(superType.prototype)
  prototype.constructor = superType
  subType.prototype = prototype
}

function Person(name) {
  this.name = name
  this.hobby = ['eat', 'run']
}

Person.prototype.sayName = function () {
  console.log(this.name)
}

Person.prototype.sayHobby = function () {
  console.log(this.hobby)
}

function Student(name, age) {
  this.age = age
  Person.call(this, name)
}

enhance(Person, student)

Student.prototype.sayAge = function () {
  console.log(this.age)
}
```

通过`enhance`函数，便只调用了一次父类构造函数，效率更高，并且原型链任然保持不变。因此，寄生式组合继承可以算是引用类型继承的最佳模式。

## 事件循环（event loop）

JavaScript 代码由上而下执行，遇到同步代码直接放入主线程，遇到异步代码放进异步队列。在异步队列中，宏任务（如`setTimeout`，`setInterval`）会放进宏任务队列，微任务（如`Promise`）则放进微任务队列。

当主线程中的同步代码执行完毕之后，会进入异步队列中读取异步代码，首先将微任务放进主线程中执行，所有微任务执行完毕之后，就会将宏任务放进主线程执行。

不断重复上面的步骤。
