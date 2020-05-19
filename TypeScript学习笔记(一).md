### 什么是TypeScript

[TypeScript](http://www.typescriptlang.org/) 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**

官方定义

> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open source.
>
> TypeScript 是 JavaScript 的类型的超集，它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。TypeScript 是开源的。

### 为什么要学习 TypeScript

* Typescript 是对 JavaScript 的增强，它大大增强了代码的可读性和维护性，让我们编写出更加健壮的代码

* 未来前端的发展趋势
  * 最新发布的 **Vue3** 使用了TypeScript。这一点其实就够了，尤大的认可就是最大的理由了
  * **Angular** 在 2.0 版本就内置了 TypeScript
  * **React** 对TypeScript 的支持也很丝滑
* 涨工资啊，要高薪啊

### 开始学习TypeScript

#### 类型注解

学习TypeScript之前我们先来了解类型注解这个概念TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。

```ts
// js
let num = 5 
num = 'hello' // 没毛病

// ts
let num: number = 5 
num = 'hello' // 报错，因为定义了num为number类型的变量所以赋值为string类型时会报错
```



**然后我们来看看TypeScript中的基本类型**

#### 布尔值

最基本的数据类型就是简单的`true`/`false`值，在JavaScript和TypeScript里叫做`boolean`（其它语言中也一样）。

```ts
let bool: boolean = true
```

#### 数字

和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是`number`。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。

```ts
let num: number = 123
num = 0b1101 // 二进制
num = 0o164 // 八进制
num = 0x8b // 十六进制
```

#### 字符串类型

和JavaScript一样，可以使用双引号（`"`）或单引号（`'`）表示字符串。

```ts
let name: string = 'hui'
```

你还可以使用*模版字符串*，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（ **`** ），并且以`${ expr }`这种形式嵌入表达式

```ts
let name: string = `hui`
let hello: string = `hello, my name is ${name}`
```

#### 数组

TypeScript像JavaScript一样可以操作数组元素。

有两种方式可以定义数组。 第一种，可以在元素类型后面接上`[]`，表示由此类型元素组成的一个数组：

```ts
let arr1: number[] = [1, 2, 3]
let arr2: string[] = ['a', 'b', 'c']
let arr3: (number | string)[] = [1, 'b', 3] // 数组元素既可以是number类型也可以是string类型
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```ts
let arr1: Array<number> = [1, 2, 3]
let arr2: Array<string> = ['a', 'b', 'c']
let arr3: Array<number | string> = [1, 'b', 2]
```

#### 元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为`string`和`number`类型的元组。

```ts
let x: [string, number]
x = ['hui', 2] // OK
x = [2, 'hui'] // Error
```

#### 枚举类型

`enum`类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```ts
enum Roles {
  SUPER_ROLE,
  ADMIN,
  USER
}
console.log(Roles.SUPER_ROLE) // 0
console.log(Roles.ADMIN) // 1
console.log(Roles.USER) // 2
```

默认情况下，从`0`开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从`1`开始编号：

```ts
enum Roles {
  SUPER_ROLE = 1,
  ADMIN,
  USER
}
console.log(Roles.SUPER_ROLE) // 1
console.log(Roles.ADMIN) // 2
console.log(Roles.USER) // 3
```

#### 任意值 any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用`any`类型来标记这些变量：

```ts
let name:any = 'hui'
name = 123 // OK
name = true // OK
```

当你只知道一部分数据的类型时，`any`类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```ts
let list: any[] = [1, true, 'hui']
```

> 尽量少的使用 `any`， 否则你可能在用 **AnyScript** 写代码

#### 空值

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是`void`：

```ts
function getName():void {
  alert('my name is hui')
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```ts
let v: void
v = undefined // OK
```

#### Null 和 Undefined

TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。 和`void`相似，它们的本身的类型用处不是很大：

```ts
let u: undefined = undefined
let n: null = null
```

#### Never

`never`类型表示的是那些永不存在的值的类型。`never` 类型用于返回 `error` 死循环

```ts
function getError(message: string): never {
    throw new Error(message);
}
function infiniteFunc(): never {
    while (true) {}
}
```

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使`any`也不可以赋值给`never`。

```ts
let neverVarible: never = (() => {
  while (true) {}
})()
let num: number = 123
let name: string = 'hui'
num = neverVarible // OK
neverVarible = name // error
```

#### 类型断言

类型断言就是手动指定一个类型的值

类型断言有两种形式。 其一是“尖括号”语法：

```ts
let str: any = "this is a string";

let strLength: number = (<string>str).length;
```

另一个为`as`语法：

```ts
let str: any = "this is a string";

let strLength: number = (str as string).length;
```



**下一篇我们来讲讲Typescript的`接口`**

