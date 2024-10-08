---
title: TypeScript快速入门语法
img: https://cdn.pixabay.com/photo/2015/04/14/10/30/bird-721951_960_720.jpg
excerpt: 作为JavaScript的超集, 大大扩展了JavaScript的数据类型, 在大型项目中有效的避免出现低级的bug
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
categories: 计算机
tags: [前端,TypeScript,编程语法]
mathjax: true
date: 2022-05-05 21:30:11
---


# TypeScript快速入门语法

## 一、语法

### 1.前言

JS绝大部分都是类型错误，TS为JS添加类型支持。

JS属于动态类型的编程语言，在执行期做类型检查。
TS属于静态类型的编程语言，可以提前在编写代码的同时发现代码的错误。

Node.js和浏览器都只认识JS，不认识TS，需要先将TS代码转化为JS代码。

```bash
npm i -g typescript # 安装TS工具包，把TS文件解析成JS文件
tsc hello.ts
node hello.js 
```

所有合法的JS代码都是TS代码

>新建ts文件

```typescript
let a:number = 18;
console.log(a);
```

```bash
#安装ts-node包，直接运行ts代码
npm i -g ts-node
#运行TS文件
ts-node hello.ts  
```
　　
### 2.基本数据类型

#### 2.1 类型注解

给变量一个限制类型，如果更换其他类型会报错

```ts
let a:number = 18;
```

#### 2.2 数据类型

1. 原始类型：`number`/`string`/`boolean`/`null`/`undefined`/`symbol`
为JS原来就有的类型

2. 对象类型: object
在TS中更加细化

* 数组类型：
  * `let a: number[]=[1,2,3]`
  * `let a: boolen[]=[true,false]`
* 联合类型，由多个类型组成
  * `let a: (number | string)[]=[1,'a',3]`
* 类型别名，为复杂类型起别名
  * `type CustomArray = (number | string)[]`
* 函数类型，指定参数类型和返回值类型
  * `function add(num1: number,num2: number): number{ return num1+num2 }`
  * `const add:(num1: number,num2: number)=>number=(num1,num2)=>{return num1+num2}`
  * `function say(): void{ console.log('Hello') }`
  * `function mySile(start?:number):void{}` 可选参数，往后站
* 对象类型

  ```ts
  let person:{
      name:string;
      age:number;
      hobbies:string[];
      say():void;
  } = {
      name: 'Max',
      age: 27,
      hobbies: ['Sports', 'Cooking'],
      say() {console.log("Hello!");}
  }
  ```

* 接口
  * 当对象类型被多次使用，一般用接口来描述

  ```ts
  interface IPerson{
      name: string;
      age: number;
      hobbies: string[];
      say(): void;
  }
  let person:IPerson = {
      name: 'Max',
      age: 27,
      hobbies: ['Sports', 'Cooking'],
      say() {console.log("Hello!");}
  }
  ```

  * 接口`interface`与类型别名`type`的区别
    * 接口只能为对象指定类型，类型别名可以指定任何类型
  * 接口继承

    ```ts
    interface IAnimal{
        age: number;
        say(): void;
    }
    interface IPerson extends IAnimal{
        name: string;
        hobbies: string[];
    }
    let person:IPerson = {
        name: 'Max',
        age: 27,
        hobbies: ['Sports', 'Cooking'],
        say() {console.log("Hello!");}
    }
    ```

* 元组（Tuple）
  * 特定类型的数组：确切的知道要包含的元素以及特定索引对应的类型。
  * `let position:[number,number] = [39,40]`
* 类型推论
  * TS自带类型推论，在声明变量和初始化值时，会自动对变量进行类型推断
* 类型断言
  * 利用`as`指定一个更加具体的类型
* 字面量类型
  * `const a = 'up'`，a的类型就是`up`
* 枚举类型

  ```ts
  enum Dirction {
    Up,
    Down,
    Left,
    Right
  }
  Dirction.up  //使用枚举类型,枚举数值默认从0开始自增
  ```

* any类型
  * `typeof`获取变量类型，也可以对类型进行查询

### 3.高级数据类型

#### 3.1 class类型

* 初始化

```ts
class Person {
  age: number;
  sex = 'man';
}
```

* 构造函数

```ts
class Person {
  age: number;
  sex: string;

  constructor(age:number,sex:string){
      this.age=age;
      this.sex=sex;
  }
}
```

* 继承

```ts
class Animal{
  move(){
    console.log('roaming the earch...');
  }
}
class Dog extends Animal{
  say(){
    console.log("wang wang");
  }   
}
let dog = new Dog();
dog.say();
```

* 实现

```ts
interface Hello {
  say(): void;
}
class Person implements Hello{
  say(): void {
    console.log("Hello");
  }
}
let p = new Person();
p.say();
```

* 可见性
  * public 公有的（默认）
  * protected 受保护的：类与子类中（非实例对象）可见
  * private 私有的：只在当前类可见
* readonly 修饰符
  * 防止构造函数外对属性赋值，但必须手动进行类型注解，否则就为字面量类型
* 类型兼容
  * 结构化的类型系统：当两个类的结构相同，可以互相实现
  * 接口之间也有兼容性

  ```ts
  class Point {
      x: number;
      y: number;
      z: number;
  }
  class Point2 {
      x: number;
      y: number;
  }
  let p:Point2 = new Point(); // 不能反过来，Point > Point2
  ```

  * 函数之间的兼容性：1. 参数个数 2. 参数类型 3. 返回值类型

#### 3.2 交叉类型

功能类似继承

```ts
interface Person {
    name: string;
    age: number;
}
interface Book {
    title: string;
}
type PersonAndBook = Person & Book;
let personAndBook: PersonAndBook = {
    name: 'John',
    age: 30,
    title: 'Hello World'
}
```

与继承额区别：

1. 当出现同名属性和方法时，继承会报错

#### 3.3 泛型

在保证类型安全的情况下，让函数与多种类型一起工作，从而实现复用

* 创建泛型函数

```ts
function id<Type>(value: Type):Type{   //Type可以为任意字符串
    return value;
}
let num = id<number>(10);  //调用函数
let num = id(10);          //TS类型参数推断，简化写法
```

* 泛型约束

```ts
function id<Type>(value: Type[]):Type[]{  //约束为数组
    return value;
}
```

```ts
//利用接口类型兼容性来作泛型约束
interface ILength {
    length: number;
}
function id<Type extends ILength>(value: Type[]):Type[]{
    console.log(value.length);
    return value;
}
```

```ts
//泛型可以有多个，而且泛型之间还可以有类型约束
function grep<Type, Key extends keyof Type>(obj: Type, key: Key) {

}
grep({ name: 'John', age: 30 }, 'name');
```

* 泛型接口

```ts
interface IAnimal<T> {
  name: T;
  age: number;
}

let animal: IAnimal<string> = {
  name: "cat",
  age: 3
}
```

* 泛型类
  * 类似泛型接口

* 泛型工具
  * `Partial<Type>` 用来创建一个类型，把传入的对象所有属性设置为可选
  * `Readonly<Type>`用来创建一个类型，把传入的对象所有属性设置为只读
  * `Pick<Type,Keys>`用来创建一个类型，从传入对象中选择其中几个属性
  * `Record<Keys,Type>`用来创建一个类型，把属性设置为Key，类型设置为Type

#### 3.4 索引签名类型

当无法确定对象的属性时，需要用到索引签名类型

```ts
interface Object {
  [key: string]: number;
}

let obj: Object = {
  name: 1,
  age: 2,
  height: 3,
}
```

#### 3.5 映射类型

基于旧类型创建新类型

```ts
type PropKey = "x" | "y" | "z";
type Type1 = { x: number; y: number; z: string };
type Type2 = { [P in PropKey]: number };
type Type3 = { [p in keyof Type1]: string };
type Type4 = Type1['x'];            //索引查询类型，获取特定的键对应值的类型
type Type5 = Type1['x'|'y'|'z'];    //获取三个值的联合类型
type Type6 = Type1[keyof Type1];    //获取Type1所有属性的联合类型
```

#### 3.6 类型声明文件

* `.ts`文件
  1. 既包含类型信息又包含可执行文件代码
  2. 可以编译为js文件
* `.d.ts`文件
  1. 只包含类型信息的类型声明文件
  2. 不会生成js文件
* 内置类型声明文件
* 第三方库的类型声明文件
* 自己实现类型声明文件
  * 给JS文件声明类型，方便给TS文件使用
    * 使用declare关键字，声明js与ts相同的数据类型
    * 声明文件的名字需要与js的名字相同
    * 用export导出声明后的数据类型
  * 给TS文件声明类型，方便给其他TS文件使用
