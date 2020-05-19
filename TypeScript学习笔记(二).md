本节来学习一下TypeScript的接口

### 什么是接口（interface）

TypeScript的核心原则之一是对值所具有的*结构*进行类型检查。 它有时被称做“鸭式辨型法”或“结构性子类型化”。 在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

### 基本用法

通过一个简单的例子来看看接口是如何工作的

```ts
function getFullName(fullName: {firstName: string, lastName: string}) {
  return `${fullName.firstName} ${fullName.lastName}`
}
let fullName = {
  firstName: 'li',
  lastName: 'hh',
  age: 18
}
getFullName(fullName)
```

类型检查器会查看`getInfo`的调用。 `getInfo`有一个参数，并要求这个对象参数有一个名为`firstName`类型为 `string` 和名为 `lastName` 类型为 `string` 的属性。 需要注意的是，我们传入的对象参数实际上会包含很多属性，但是编译器只会检查那些必需的属性是否存在，并且其类型是否匹配。 然而，有些时候TypeScript却并不会这么宽松，我们下面会稍做讲解。

下面我们重写上面的例子，这次用接口描述：必须包含 `firstName` 和 `lastName` 属性

```ts
interface FullName {
  firstName: string,
  lastName: string
}
function getFullName(fullName: FullName) {
  return `${fullName.firstName} ${fullName.lastName}`
}
let fullName = {
  firstName: 'li',
  lastName: 'hh'
}
getFullName(fullName)
```

`FullName` 接口就好比是一个名字，用来描述例子里的要求，他表示了有 `firstName` 和 `lastName` 并且类型为 `string` 的对象。还有一点值得提的是，类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。

> 接口的名字首字母要大写

### 可选属性

接口里的属性有时候不都是必须的，有时候是可选的，我们用 `?` 用来定义可选属性

```ts
interface Fruit {
  type: string,
  color?: string
}
const getFruit({type, color}: Fruit):string {
  return `A ${color ? (color + ' ') : ''} ${type}`
}
getFruit({
  color: 'red',
  type: 'apple'
}) // ok
getFruit({
  type: 'apple'
}) // ok
```



### 多余属性检查

还是上面那个例子，如果我们在传递的参数中多加一个属性，例如

```ts
getFruit({
  color: 'red',
  type: 'apple'，
  size: 19
}) // err
```

这个时候 TypeScript 会告诉我们传的参数多了一个 `size` 的属性，但是其实这个是不影响我们的结果的，这个时候有两种办法来解决这个问题。

第一种使用`类型断言`

```ts
getFruit({
  color: 'red',
  type: 'apple'，
  size: 19
} as Fruit) // ok
```

第二种使用索引签名

```ts
interface Fruit {
  type: string,
  color?: string,
  [prop: string]: any
}
const getFruit({type, color}: Fruit):string {
  return `A ${color ? (color + ' ') : ''} ${type}`
}
getFruit({
  color: 'red',
  type: 'apple'，
  size: 19
} as Fruit) // ok
```

### 只读属性

接口还可以设置只读属性，表示这个属性不可被修改

```ts
interface Fruit {
  type: string,
  readonly color: string
}
const apple: Fruit = {
  type: 'apple',
  color: 'red'
}
apply.color = 'green' // err
```

### 函数类型

接口不仅可以定义对象的形式，还可以定义函数形式

```ts
interface AddFunc {
  (number1: number, number2: number) => number
}
const addFunc: AddFunc = (n1, n2) => {
  return n1 + n2
}

```

### 索引类型

接口还定义索引类型

```ts
interface ArrInter {
  0: string,
  1: number
}
const arr: ArrInter = ['a', 1]
```

### 继承接口

一个接口可以继承另一个接口， 使用 `extends` 关键字来实现继承

```ts
interface Fruit {
	type: string
}
interface Apple extends Fruit {
  color: string
}
cosnt apple: Apple = {
  color: 'red'
} // 报错， 因为继承 Fruit 接口所以必须有 type 属性
```



### 混合类型接口

接口还可以定义包含任意类型属性的接口

```ts
interface Counter {
  (): void, // 一个函数
  count: number
}
const getCounter = ():Counter => {
  const c = () => c.count++
  c.count = 1
  return c
}
const counter: Counter = getCounter()
counter()
console.log(counter.count) // 2
counter()
console.log(counter.count) // 3
counter()
console.log(counter.count) // 4
```

