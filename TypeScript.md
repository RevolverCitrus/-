# TypeScript

## 简介

TS是JS的超集，所以[JS基础](https://so.csdn.net/so/search?q=JS基础&spm=1001.2101.3001.7020)的类型都包含在内

***\*起步安装 npm install typescript -g\****

***\*运行tsc 文件名\****

## 编译代码

```tsx
tsc greeter.ts ------//tsc 文件名
```

## tsconfig.json文件配置

```json
/*
    tsconfig.json是编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译
        'include' 用来指定哪个ts文件需要被编译
            路径：** 表示任意目录
                  * 表示任意文件
        'exclude' 不需要被编译的文件目录
            默认值：["node_modules","bower_components","jspm_packages"]
        
        complierOptions 编译器的选项
            target 用来指定ts被编译为的ES的版本
            module 指定要使用的模块化的规范
            lib 用来指定项目中要使用的库
            outDir 用来指定编译后文件所在的目录
            outFile 将代码合并为一个文件，设置之后所有的全局作用域中的代码会合并到同一个文件中
            allowJs 是否对js文件进行编译，默认是false
            checkJs 是否检查js代码是否符合语法规范，默认是false
            removeComments 是否移除注释
            noEmit 不生成编译后的文件
            noEmitOnError 当有错误的时候不生成编译后的文件

            检查语法
            strict 所有严格检查的总开关
            alwaysStrict 用来设置编译后的文件是否使用严格模式，默认是false
            noImplicitAny 不允许隐式的any类型
            noImplicitThis 不允许不明确类型的this
            strictNullChecks 严格的检查空值

*/
```



## 基本数据类型

Boolean、Number、String、`null`、`undefined` 以及 ES6 的 [Symbol](http://es6.ruanyifeng.com/#docs/symbol) 和 ES10 的 [BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。

### 声明变量以及类型

```tsx
// 声明一个变量a，同时指定它的类型为number
let a:number;
// a的类型设置为number，在以后的使用过程中a的值只能是数字
a=10;
// a='hello';//此行代码会报错，因为变量a的类型是number，不能赋值字符串

let b:string;
b='hello';

// 声明变量直接进行赋值
// let c:boolean=true;

// 如果变量的声明和赋值是同时进行的，TS可以自动对变量进行类型检测
let c=false;
c=true;

// JS中的函数是不考虑参数的类型和个数的
//但TS中就可以限制参数的类型,还可以限制函数的返回值的类型sum():number{}
function sum(a:number,b:number):number{
    return a + b;
}
let result=sum (123,456)//579
// sum(123,'hello')//报错

export {}
```

### 数据类型

```tsx
// 也可以直接使用字面量进行类型声明
let a:10;
a=10;
//----------------------------------------------

// 可以使用 | 来连接多个类型(联合类型)
let b:'male' | 'female';
b='male';

let c:boolean | string;
c=true;
c='hello';
//--------------------------------------------------

// 不推荐使用any
// any:任意类型,如果一个变量设置类型为any后相当于该变量关闭了TS的类型检测
let d: any;
// 如果声明变量不指定类型，则TS解析器会自动判断变量类型为any(隐式的any)
// any还可以赋值给任意类型的变量
// let d;
d = 4;
d = "maybe a string instead";
d = false; 
//---------------------------------------------------

// unknown:表示未知变量的值
// unknown实际上就是一个类型安全的any
let e:unknown;
e = 4;
e = false;
e = "maybe a string instead";

let s:string;
// s=e;//报错，因为e的类型是unknown，unknown不能直接赋值给其他变量
if(typeof e==='string'){
    s=e;
}
//---------------------------------------------------------

// 类型断言，可以用来告诉解析器变量的实际类型
/* 语法：
        变量 as 类型
        <类型> 变量
*/
s = e as string;//不报错
s = <string> e;
//--------------------------------------------------------

// void:表示为空，以函数为例，就表示没有返回值的函数
function fn():void{
    // return 123;//报错，此函数不能有返回值
}

// never表示永远不会返回结果
function fn2():never{
    throw new Error('报错了！');
}

export {}
```



### 对象、函数、数组和枚举

```tsx
// object表示一个js对象
let a:object;
a={};
a=function(){};
//---------------------------------------------------------

// {}用来指定对象中可以包含哪些属性
// 语法：{属性名：属性值，属性名：属性值......} 
//属性名?：属性值 问号表示可以有，也可以没有
let b:{name:string,age?:number};
// b={}//报错b要有一个属性而且值为string类型
b={name:'孙悟空'}

// [propName:string]:any 表示任意类型的属性
let c:{name:string,[propName:string]:any};
c={name:'猪八戒',age:18,gender:'male'}
//---------------------------------------------------------

/*
    设置函数结构的类型声明：
        语法：(形参:类型，形参:类型，......)=>返回值
*/

// 指定值为函数，而且有两个number类型的参数且返回值为number类型
let d:(a:number,b:number)=>number
d=function(n1,n2){
    return n1+n2;
}

/*函数重载

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

如果参数类型不同，则参数类型应设置为 any。

参数数量不同你可以将不同的参数设置为可选。*/

function fn(params: number): void
 
function fn(params: string, params2: number): void
 
function fn(params: any, params2?: any): void {
 
    console.log(params)
 
    console.log(params2)
 
}
 
fn(123)
 
fn('123',456)

//---------------------------------------------------------


/*
    数组的类型声明：
        类型[]
        Array<类型>
*/

// string[]表示字符串数组
let e:string[];
e=['a','b'];

let f:number[];
f=[1,2,3];

let g:Array<number>;
g=[1,2,3];

//用接口表示数组
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
//表示：只要索引的类型是数字时，那么值的类型必须是数字。

//多维数组
let data:number[][] = [[1,2], [3,4]];


//any 在数组中的应用
let list: any[] = ['test', 1, [],{a:1}]

//---------------------------------------------------------


/*
    元组：就是固定长度的数组
        语法:[类型，类型，......]
*/
let h:[string,number];
h=['hello',123];
//---------------------------------------------------------


/*
    enum 枚举

*/
enum Gender{
    Male=1,
    Female=0
}

let i:{name:string,gender:Gender};
i={
    name:'孙悟空',
    gender:Gender.Male
}
// console.log(i.gender===Gender.Male);
//---------------------------------------------------------

// &表示同时
let j:{name:string} & {age:number};
j={name:'孙悟空',age:18};
//---------------------------------------------------------

// 类型的别名
type myType=1|2|3|4|5;
let k:myType;
let l:myType;
let m:myType;

// k=6;//报错6不是myType类型的值
k=1;
//---------------------------------------------------------

export {}
```

### 接口和对象类型

定义对象的方式要用关键字**interface**（接口），我的理解是使用**interface**来定义一种约束，让数据的结构满足约束的格式。

```tsx
//这样写是会报错的 因为我们在person定义了a，b但是对象里面缺少b属性
//使用接口约束的时候不能多一个属性也不能少一个属性
//必须与接口保持一致
interface Person {
    b:string,
    a:string
}
 
const person:Person  = {
    a:"213"
}
```

```tsx
//重名interface  可以合并
interface A{name:string}
interface A{age:number}
var x:A={name:'xx',age:20}

//继承
interface A{
    name:string
}
interface B extends A{
    age:number
}
let obj:B = {
    age:18,
    name:"string"
}
```

可选属性 使用?操作符

```tsx
//可选属性的含义是该属性可以不存在
//所以说这样写也是没问题的
interface Person {
    b?:string,
    a:string
}
 
const person:Person  = {
    a:"213"
}
```

任意属性 [propName: string]

需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**：

```tsx
//在这个例子当中我们看到接口中并没有定义C但是并没有报错
//应为我们定义了[propName: string]: any;
//允许添加新的任意属性
interface Person {
    b?:string,
    a:string,
    [propName: string]: any;
}
 
const person:Person  = {
    a:"213",
    c:"123"
}
```

只读属性 readonly

```tsx
//readonly 只读属性是不允许被赋值的只能读取
//这样写是会报错的
//应为a是只读的不允许重新赋值
interface Person {
    b?: string,
    readonly a: string,
    [propName: string]: any;
}
 
const person: Person = {
    a: "213",
    c: "123"
}
 
person.a = 123
```

添加函数

```tsx
interface Person {
    b?: string,
    readonly a: string,
    [propName: string]: any;
    cb:()=>void
}
 
const person: Person = {
    a: "213",
    c: "123",
    cb:()=>{
        console.log(123)
    }
}
```



## 设置自动编译自动监视Ts文件（热更新）

### 第一种方法

tsc+文件名称 -w

### 第二种方法

![image-20230311093625813](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230311093625813.png)

## namespace命名空间

我们在工作中无法避免全局变量造成的污染，[TypeScript](https://so.csdn.net/so/search?q=TypeScript&spm=1001.2101.3001.7020)提供了namespace 避免这个问题出现

- 内部模块，主要用于组织代码，避免命名冲突。
- 命名空间内的类默认私有
- 通过 `export` 暴露
- 通过 `namespace` 关键字定义

如果一个文件不带有顶级的==`import`==或者==`export`==声明，那么它的内容被视为全局可见的（因此对模块也是可见的）

命名空间中通过`export`将想要暴露的部分导出

如果不用export 导出是无法读取其值的

```tsx
namespace a {
    export const Time: number = 1000
    export const fn = <T>(arg: T): T => {
        return arg
    }
    fn(Time)
}
 
 
namespace b {
     export const Time: number = 1000
     export const fn = <T>(arg: T): T => {
        return arg
    }
    fn(Time)
}
 
a.Time
b.Time
```

合并命名空间

重名的命名空间会合并

## 三斜线指令

三斜线指令是包含单个XML标签的单行注释。 注释的内容会做为编译器指令使用。

三斜线指令仅可放在包含它的文件的最顶端。 一个三斜线指令的前面只能出现单行或多行注释，这包括其它的三斜线指令。 如果它们出现在一个语句或声明之后，那么它们会被当做普通的单行注释，并且不具有特殊的涵义。

/// <reference path="..." />指令是三斜线指令中最常见的一种。 它用于声明文件间的 依赖。

三斜线引用告诉编译器在编译过程中要引入的额外的文件。

你也可以把它理解能import，它可以告诉编译器在编译过程中要引入的额外的文件
