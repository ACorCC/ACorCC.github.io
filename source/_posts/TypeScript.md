---
title: TypeScript
date: 2023-02-07 15:47:02
tags:
- TypeScript
categories:
- [前端, 语言, TypeScript]
---



# 基本语法

## 基本数据类型

在变量名后加上一个冒号，冒号后加上类型的名称

```typescript
/* 字符串 */
const q: string ='string';
/* 数字 */
const w: number = 1;
/* 布尔值 */
const e: boolean = true;
/* null */
const r: null = null;
/* undefined */
const t: undefined = undefined;
```

## 对象数据类型

一般在开头加上一个大写的I来表示类型，用于和普通的对象或类区分

```typescript
const bytedancer: IBytedancer = {
    jobId: 9303245,
    name:'Lin',
    sex: 'man',
    age: 28,
    hobby:' swimming',
}

interface IBytedancer {
    /* 只读属性: 约束属性不可在对象初始化外赋值 */
    readonly jobId: number;
    name: string;
	sex: 'man' | 'woman' | 'other';
    age: number;
	/* 可选属性: 定义该属性可以不存在 */
    hobby?: string;
    /* 任意属性: 约束所有对象属性都必须是该属性的子类型 */
    [key: string]: any;
}

/* 报错: 无法分配到 “jobId"因为它是只读属性 */
bytedancer.jobId = 12345;
/* 成功: 任意属性标注下可以添加任意属性 */
bytedancer.plateform = 'data';
/* 报错: 缺少属性“name"，hobby可缺省 */
const bytedancer2: IBytedancer = {
    jobId: 89757
    sex: 'woman'
    age: 18,
}
```

## 函数类型

```typescript
function add(x，y) {
    return x + y;
}
const mult = (x，y) => x * y;

/* 定义入参和出参的类型 */
function add(x: number, y: number): number {
    return x + y;
}
const mult: (x: number, y: number) => number = (x，y) => x * y;

/* 也可以通过interface关键字进行定义 */
interface IMult {
    (x: number， y: number): number;
}
const mult: IMult = (x，y) => x * y;


/* 函数重载*/
/* 对getDate函数进行重载，timestamp为可缺省参数 */
function getDate(type:'string',timestamp?: string): string;
interface IGetDate {
    (type: 'string',timestamp?: string): string;
    (type: 'date',timestamp?: string): Date;
    (type: 'string' | 'date', timestamp?: string): Date | string;
}
/* 不能将类型“(type: any，timestamp: any) => string | Date”分配给类型“IGetDate'。
不能将类型“stringDate”分配给类型“string”。
不能将类型“Date”分配给类型“string”。ts(2322) */
const getDate2: IGetDate = (type, timestamp) => {
    const date = new Date(timestamp);
    return type === 'string' ? date.toLocaleString() : date;
}
```

## 数组类型

```typescript
/* [类型 + 方括号] 表示 */
type IArr1 = number[];
/* 泛型表示 */
type IArr2 = Array<string | number | Record<string, number>>;
/* 元祖表示 */
type IArr3 = [number, number, string, string];
/* 接口表示 */
interface IArr4 {
    [key: number]: any;
}

const arr1: IArr1 = [1, 2, 3, 4, 5, 6];
const arr2: IArr2 = [1, 2, '3', '4', { a: 1 }];
const arr3: IArr3 = [1, 2, '3', '4'];
const arr4: IArr4 = ['string', () => null, {}, []];
```

## 补充类型

```typescript
/* 空类型，表示无赋值 */
type IEmptyFunction = () => void;
/* 任意类型，是所有类型的子类型 */
type IAnyType = any;
/* 枚举类型:支持枚举值到枚举名的正、反向映射 */
enum EnumExample (
    add = '+',
    mult = '*',
}
EnumExample['add'] === '+'
EnumExample['+'] === 'add'

enum ECorlor { Mon, Tue, Wed, Thu, Fri,Sat, Sun };
ECorlor['Mon'] === 0;
ECorlor[0] === 'Mon';
/* 泛型 */
type INumArr = Array<number>
```

## 泛型

```typescript
function getRepeatArr(target) {
    return new Array(100).fill(target);
}
type IGetRepeatArr = (target: any) => any[l;

/* 不预先指定具体的类型，而在使用的时候再指定类型的一种特性 */
type IGetRepeatArrR = <T>(target: T) => T[];

/* 泛型接口 & 多泛型 */
interface IX<T，U> {
    key: T;
    val: U;
}
/* 泛型类 */
class IMan<T> {
    instance: T;
}
/* 泛型别名 */
type ITypeArr<T> = Array<T>;

/* 泛型约束: 限制泛型必须符合字符串 */
type IGetRepeatStringArr = <T extends string>(target: T) => Tl];
const getStrArr: IGetRepeatStringArr = target => new Array(100).filltarget);
/* 报错: 类型“number”的参数不能赋给类型“string”的参数 */
getstrArr(123);

/* 泛型参数默认类型 */
type IGetRepeatArr<T = number> = (target: T) => T[];
const getRepeatArr: IGetRepeatArr = target => new Array(100).fill(target);
/* 报错: 类型“string”的参数不能赋给类型“number”的参数 */
getRepeatArr('123')
```

## 类型别名 & 类型断言

```typescript
/* 通过type关键字定义了IObjArr的别名类型 */
type IObjArr = Array<{
    key: string;
    [objKey: string]: any;
}>
function keyBy<T extends IObjArr>(objArr: Array<T>) [
    /* 未指定类型时，result类型为 {} */
    const result = objArr.reduce((res, val, key) => {
        res[key] = val;
		return res;
    }, {});
	/* 通过as关键字，断言result类型为正确类型 */
	return result as Record<string，T>;
}
```

## 字符串/数字 字面量

```typescript
/* 允许指定字符串/数字必须的固定值 */

/* IDomTag必须为html、body、div、span中的其一 */
type IDomTag ='html' | 'body' | 'div' | 'span';
/* IOddNumber必须为1、3、5、7、9中的其一 */
type IOddNumber = 13579;
```

# 高级类型

## 联合/交叉类型

为书籍列表编写类型

```typescript
const bookList = [{
    author: 'xiaoming',
    type: 'history',
    range: '2001-2021',
}, {
    author: 'xiaoli',
    type: 'story',
    theme: 'love',
}]
```

类型声明繁琐，存在较多重复

```typescript
interface IHistoryBook {
    author: string;
    type: string;
    range: string
}
interface IStoryBook {
    author: string;
    type: string;
    theme: string;
}
type IBookList = Array<IHistoryBook | IStoryBook>
```

- 联合类型: IA| IB；联合类型表示一个值可以是几种类型之一
- 交又类型: IA & IB；多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性

```typescript
const bookList = [{
    author: 'xiaoming',
    type: 'history',
    range: '2001-2021',
}, {
    author: 'xiaoli',
    type: 'story',
    theme: 'love',
}]

type IBookList = Array<{
    author: string;
} & ({
	type: 'history';
    range: string;
} | {
    type: 'story';
    theme: string;
})>
```

## 类型保护与类型守卫

```typescript
interface lA { a: 1, a1: 2 }
interface IB { b: 1, b1: 2 }

/* 类型守卫: 定义一个函数，它的返回值是一个类型谓词，生效范围为子作用域 */
function getlslA(arg: IA | IB): arg is IA {
    return !!(arg as IA).a;
}
function log2(arg: IA | IB) {
    if (getlslA(arg)) {
        console.log(arg.a1)
    } else {
		console.log(arg.b1);
    }
}

// 实现函数reverse
// 实现函数logBook类型
// 函数接受书本类型，并logger出相关特征
function logBook(book: IBookltem) {
    // 联合类型 + 类型保护 = 自动类型推断
    if (book.type === "history') {
        console.log(book.range)
	} else {
        console.log(book.theme);
    }
}
```

## 高级类型

```typescript
/**
 * 实现merge函数类型
 * 要求sourceObj必须为targetObj的子集
*/
function merge1(sourceObj, targetObj) {
    const result = { ...sourceObj };
    for(let key in targetObj) {
        const itemVal = sourceObj[key];
        itemVal && ( result[key] = itemVal );
    }
	return result;
}
function merge2(sourceObj, targetObj) {
    return { ...sourceObj, ...targetObj };
}

interface ISourceObj {
	x?: string;
    y?: string;
}
interface ITargetObj {
    x: string;
    y: string;
}
type IMerge = (sourceObj: ISourceObj, targetObj: ITargetObj) =>ITargetObj;
/**
 * 类型实现繁琐: 若obi类型较为复杂，则声明source和target便需要大量重复2遍
 * 容易出错: 若target增加/减少key，则需要source联动去除
*/

interface IMerge{
    <T extends Record<string, any> >(sourceObj: Partial<T>, targetObj: T): T;
}
type IPartial<T extends Record<string, any> > = {
    [P in keyof T]?: T[P];
}
// 索引类型: 关键字【keyof】，其相当于取值对象中的所有kev组成的字符串字面量,如
type lKeys = keyof { a: string; b: number l; // => type IKeys = "a" | "b"
// 关键字【in】，其相当于取值 字符串字面量 中的一种可能，配合泛型P,即表示每个key
// 关键字【?】，通过设定对象可选选项，即可自动推导出子集类型
```

## 函数返回值类型

```typescript
// 实现函数delayCall的类型声明
// delayCall接受一个函数作为入参,其实现延迟1s运行函数
// 其返回promise，结果为入参函数的返回结果
function delayCall(func) {
    return new Promise(resolve = > {
        setTimeout(0 => {
        	const result = func();
        	resolve(result);
		}, 1000);
	});
}

type IDelayCall = <T extends () => any>(func: T) => ReturnType<T>;
type lReturnType<T extends (...args: any) => any> = T extends (..args: any) => infer R ? R : any

// 关键字【extends】跟随泛型出现时，表示类型推断，其表达可类比三元表达式
// 如 T === 判断类型 ? 类型A : 类型B

// 关键字【infer】出现在类型推荐中，表示定义类型变量，可以用于指代类型
// 如 该场景下，将函数的返回值类型作为变量，使用新泛型R表示，使用在类型推荐命中的结果中
```







