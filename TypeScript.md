# TypeScript

### 目錄
* [快速入門](#快速入門)
* [變數宣告](#變數宣告)
* [分割代入](#分割代入)
* [型別](#型別)
* [命名空間](#命名空間)
* [模組機制](#模組機制)
* [介面](#介面)
* [函式](#函式)
* [類別](#類別)
* [型別兼容性](#型別兼容性)
* [通用型別](#通用型別)
* [後設資料](#後設資料)
* [泛型](#泛型)
* [型別斷言](#型別斷言)
* [象徵](#象徵)
* [冪運算子](#冪運算子)
* [混入](#混入)
* [合併宣告](#合併宣告)
* [承諾](#承諾)
* [產生器](#產生器)
* [非同步與等待](#非同步與等待)
* [修飾器](#修飾器)

***

## 快速入門
```bash
# 安裝 ts-node 與 typescript
$ npm i ts-node typescript@2 -g

# 建立練習檔
$ touch script.ts
```

```ts
// script.ts
var ht = 'Hello TypeScript';
console.log(ht);
```

```bash
# 執行練習檔
$ ts-node script
```

```bash
# 輸出
Hello TypeScript
```

## 變數宣告

### 一般賦值
```ts
let foo = 123;
foo;  // 123
foo = 456;
foo;  // 456
```

```ts
let foo = 123;

if (true) {
  let foo = 456;
}

foo;  // 123
```

### 單一賦值
```ts
const bar = 123;
bar;  // 123
bar = 456;  // Error
```

```ts
const foo = { bar: 123 };
foo = { bar: 456 };  // Error
foo.bar = 456;  // OK
foo;  // { bar: 456 }
```

## 分割代入

### 陣列分割代入
```ts
let [foo, bar] = [123, 456];
foo;  // 123
bar;  // 456
```

```ts
let [foo = 1] = [];
foo;  // 1
```

### 物件分割代入
```ts
const foo = {
  x: 'bar',
  y: 'baz'
};

let { x, y } = foo;
x;  // "bar"
y;  // "baz"
```

### 混合型分割代入
```ts

```

### 函式參數分割代入
```ts

```

## 型別

### 布林值
```ts
let foo: boolean = true;
let bar: boolean = false;
```

### 數值
```ts
let foo: number = 18;  // 十進制
```

### 字串
```ts
let myName: string = 'Hale';

// 模板字串
let sentence: string = `My name is ${myName}.`;  // My name is Hale.
```

### 陣列
```ts
let foo: number[] = [1, 2, 3];
let bar: string[] = ['a', 'b', 'c'];

// 或者
let foo: Array<number> = [3, 2, 1];
let bar: Array<string> = ['x', 'y', 'z'];
```

### 元組
```ts
let foo: [number, string];  // 宣告一個元組型別
foo = [123, 'abc'];  // 將 foo 初始化
```

### 列舉
```ts
enum Thing {
  Foo, Bar, Baz
};

let foo: Thing = Thing.Foo;  // 0
let bar: Thing = Thing.Bar;  // 1
let baz: Thing = Thing.Baz;  // 2

enum Thing2 {
  Up = 4,
  Down,
  Left,
  Right
};

let up: Thing2 = Thing2.Up;  // 4
let down: Thing2 = Thing2.Down;  // 5
let left: Thing2 = Thing2.Left;  // 6
let right: Thing2 = Thing2.Right;  // 7

// 使用 const 宣告
const enum Thing3 (
  // ...
  // 還能使用一些一元或二元運算子
  Foo = 60 >> 2  // 0011-1100 -> 0000-1111, 15
)

enum Thing4 {
  Foo
}
let foo = Thing4.Foo;
let thing4OfFoo = Thing4[Thing4.Foo];  // Foo

// 定義宣告
declare enum Thing5 {
  Foo,
  Bar,
  Baz
}
```

### 任意值
```ts
let notSure: any = 123;
notSure = 'abc';  // OK
notSure = 'true';  // OK
```

### 空值
```ts
let foo: void = undefined;
let bar: void = null;

function baz(thing: number): void {
  this.thing = thing;
}
```
```ts
let foo: string;
foo;  // Error

let bar: string | null;
bar;  // Error

let baz: string | undefined;
baz;  // OK
```

### 型別斷言
```ts
let foo: any = 'abc';
let bar = <string>foo;  // 語法: <>
let baz: number = bar.length;  // 3

// 或者
let bar2 = foo as string;  // 語法: as
let baz2: number = bar2.length;  // 3
```

### 型別併集
```ts
let foo: string | number;

foo = 'abc';  // OK
foo = 123;  // OK

// 使用介面
// 介面用法可以參考介面章節
interface Some {
  foo();
  baz();
}

interface Thing {
  bar();
  baz();
}

function getSomeThing(): Some | Thing {
  // ...
}
let st = getSomeThing();

st.foo();  // Error
st.bar();  // Error
st.baz();  // OK
```

### 型別別名
```ts
// 型別別名
type ThingType = string | number;
let foo: ThingType;

foo = 'abc';  // OK
foo = 123;  // OK
foo = true;  // Error
```

## 命名空間

(1)
```ts
namespace Thing {
  export class Foo { }
  export class Bar { }
}

let foo = new Thing.Foo();
let bar = new Thing.Bar();
```

(2)
```ts
namespace Thing {
  export namespace Foo {
    export class Bar { }
    export class Baz { }
  }
}

let baz = new Thing.Foo.Baz()

// 等同於
import foo = Thing.Foo;
let baz = new foo.Baz();  
```

### 模組機制
```ts
// foo.ts
// 導出介面
export interface Foo {
  // ...
}

// 導出變數
export const numberRegexp = /^[0-9]+$/;
```
```ts
// bar.ts
import { Foo } from './foo';

export class Bar {
  // ...
}
```
```ts
// 預設導出
export default thing;
```

```ts
// foo.ts
export class Angular2 {
  // ...
}
```
```ts
// bar.ts
import { Angular2 as Ng2 } from './foo';  // 對導入的內容重新命名

let ng2 = new Ng2();
```

```ts
// thing.d.ts
declare module 'thing-module' {
  export function foo(): void;
}
```
```ts
// module.ts
/// <reference path="thing.d.ts" />
import * as thing from 'thing-module';
```

透過 Types 安裝模組定義

```bash
$ npm i @types/node -D
```

```ts
// script.ts
import { join } from 'path';  // OK
```

## 介面

(1)
```ts
interface Foo {
  bar: number;
  baz?: string;  // 可選參數
}

function thing(foo: Foo) {
  let bar = foo.bar;
  return bar;
}

thing({ bar: 123 });  // OK
thing({ baz: 'abc' });  // Error
thing({ bar: 123, baz: 'abc' });  // OK
```

(2)
```ts
interface Thing {
  [index: number]: string;
}

let thing: Thing = ['a', 'b', 'c'];

let foo: string = thing[1];  // 'b'
```

(3)
```ts
interface Square {
  color?: string;
  width?: number;
}

function createSquare(square: Square): { color: string; area: number } {
  // ...
}

let cs = createSquare({ width: 100, opacity: 0.5 } as Square);
```

(4)
```ts
interface Foo {
  (bar: number): void;
}

const baz = (foo: Foo) => foo(123);

baz((bar: number) => console.log(bar));  // 123
```

(5)
```ts
interface Foo {
  (x: number, y: number): boolean;
}

const foo: Foo = (x, y) => {
  let result = x + y;
  if (result >= 0) {
    return false;
  } else {
    return true;
  }
};

foo(2, -4);  // true
```

## 函式
```ts
// 設定函式型別
function addition(a: number, b: number): number {
  return a + b;
}

addition(1, 1);  // 2
addition('1', '1');  // Error
```
```ts
// 匿名函式
const addition = function(a: number, b: number): number {
  return a + b;
};

addition(1, 1);  // 2
addition('1', '1');  // Error
```

### 可選參數

(1)
```ts
const ng = function(a: string, b: string, c?: string): string {
  return `${a} ${b} ${c}`;
};

ng('Angular', 'Material', 'Firebase');  // Angular Material Firebase
ng('Angular', 'Material');  // Angular Material undefined
ng('Angular');  // Error
```

(2)
```ts
const ng = function(a: string, b: string, c?: string): string {
  if (c !== undefined) {
    return `${a} ${b} ${c}`;
  }
  return `${a} ${b}`;
};

ng('Angular', 'Material', 'Firebase');  // Angular Material Firebase
ng('Angular', 'Material');  // Angular Material
ng('Angular');  // Error
```

### 預設參數
```ts
const ng = function(a: string, b: string = 'Angular 2'): string {
  return `${a} and ${b}`;
};

ng('Angular');  // Angular and Angular 2
ng('Angular', 'Material');  // Angular and Material
```

## 類別
```ts
class Foo {
  bar: string;
  constructor() {
    // ...
  }
  baz() {
    // ...
  }
}
```
```ts
// 類別的繼承
class Foo {
  constructor() {
    // ...
  }
}

class Bar extends Foo {
  constructor() {
    super();
    // ...
  }
}
```
```ts
// 修飾字元
class Thing {
  public foo: string;
  private bar: string;
  protected baz: string;
  constructor() {
    // ...
  }
}
```
```ts
class Name {
  private name: string;
  constructor(private theName: string) {
    this.name = theName;
  }
}

new Name('Hale').name;  // Error: 'name' is private
```
```ts
class Adder {
  constructor(a: number) {
    console.log(a);
  }
  add(b: number): number {
    return b;
  }
}

let foo: Adder = new Adder(1);

console.log(foo.add(2));
```
```ts
// 將類別當作介面使用
class Point {
  x: number;
  y: number;
}

interface Point3d extends Point {
  z: number;
}

let point3d: Point3d = {
  x: 60,
  y: 120,
  z: 180
};
```
```ts
class Foo {
  constructor(public x = 0) { }

  public getFooX(): number {
    return this.x;
  }
}

class Bar {
  constructor(public x = 0) { }

  public get barX(): number {
    return this.x;
  }
}

const foo = new Foo(123);
foo.getFooX();  // 123

const bar = new Bar(123);
bar.barX;  // 123
```

## 型別兼容性
```ts
interface Foo {
  bar: string;
}

class Thing {
  // ...
}

let thing: Foo;
// 結構性的型別
thing = new Thing();
```

### 通用型別
```ts
const ng: Ng[] = [new Angular(), new Material(), new Firebase()];
```

### 後設資料
```bash
$ npm install core-js -S
```
```js
// tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

## 泛型
```ts
function identity<T>(arg: T): T {
  return arg;
}

identity(123);  // 123
identity('abc');  // 'abc'
identity(true);  // true
```
```ts
class Thing<T> {
  foo: Thing<T>;
  bar: Thing<T>;
}

let nt = new Thing<number>();
let st = new Thing<string>();

nt.foo = new Thing<boolean>();
```
```ts
class Thing<T, U> {
  key: T;
  value: U;
}

let thing = new Thing<string, boolean>();

thing.key = 'foo';  // OK
thing.value = true;  // OK
```

## 型別斷言
```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo = {} as Foo;
foo.bar = 123;
foo.baz = 'abc';

foo;  // { bar: 123, baz: 'abc' }
```
```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo = <Foo>{};
foo.bar = 123;
foo.baz = 'abc';

foo;  // { bar: 123, baz: 'abc' }
```
```ts
interface Foo {
  bar: number;
  baz: string;
  ts: string;
}

let foo = <Foo>{
  bar: 123,
  baz: 'abc'
};
foo.ts = 'TypeScript';

foo;  // { bar: 123, baz: 'abc', ts: 'TypeScript' }
```
```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo: Foo = {
  bar: 123,
  baz: 'abc'
};

foo;  // { bar: 123, baz: 'abc' }
```

### 象徵
```ts
interface Iterable {
  [Symbol.iterator](): Iterator;
}

interface Iterator {
  next(value?: any): IterationResult;
}

interface IterationResult {
  value: any;
  done: boolean;
}
```

## 冪運算子
```ts
let x = 2 ** 5;  // Math.pow(2, 5)
x;  // 32
```

```ts
let x = 2 * 5 ** 2;
x;  // 2 * 25 = 50
```

```ts
let x = -(2 ** 3);
x;  // -8

// 或者
let y = (-2) ** 3;
y;  // -8

// 錯誤
let z = -2 ** 3;
z;  // Error
```

## 混入
```ts
class Disposable {
  isDisposed: boolean;
  dispose() {
    this.isDisposed = true;
  }
}

class Activatable {
  isActive: boolean;
  activate() {
    this.isActive = true;
  }
  deactivate() {
    this.isActive = false;
  }
}

class SmartObject implements Disposable, Activatable { }
```

```ts
interface IFoo {
  add(): number;
}

class Foo implements IFoo {
  add() {
    return // ...
  }
}
```

## 合併宣告
```ts
// 合併介面
interface Box {
  height: number;
  width: number;
}

interface Box {
  scale: number;
}

const box: Box = { height: 5, width: 6, scale: 10 };
```

## 承諾
```ts
const foo = () => {
  console.log(1);

  const bar = new Promise((resolve, reject) => {
    setTimeout(() => resolve(console.log(2)), 0);
  });

  console.log(3);

  return bar;
};

foo().then(() => console.log(4));
// 1
// 3
// 2
// 4
```

```ts
Promise.all([
    p1(), p2()
  ])
  .then((data) => {
    console.log(data[0]);  // p1 結果
    console.log(data[1]);  // p2 結果
  });
```

```ts
Promise.race([
    p1(),  // 假設 p1 為主體
    p2()  // p2 不一定要執行，通常是 p1 的超時處理
  ])
  .then(() => {
    // ...
  });
```

## 產生器
```ts
function *foo(x) {
  let y = x * (yield);
  return y;
};

let bar = foo(2);
bar.next();

let baz = bar.next(3);
baz.value;  // 2 * 3 = 6
```

## 非同步與等待
```ts
async function foo(x) {  // 10
  let bar = await Promise
    .resolve(x)
    .then(x => x + 2)  // 10 + 2 = 12
    .then(x => x - 3)  // 12 - 3 = 9
    .then(x => x * 4)  // 9 * 4 = 36
    .then(x => x / 5);  // 36 / 5 = 0
  return bar;
}

foo(10).then(x => console.log(x));  // 7.2
```

```ts
async function foo(x) {  
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(console.log(2)), x);
  });
}

async function bar() {
  console.log(1);
  await foo(1000);  // 等待了一秒
  setTimeout(() => console.log(3), 1000);  //  等待了一秒，加自己的一秒
}

bar();
// 1
// 2 (一秒後打印)
// 3 (兩秒後打印)
```

## 修飾器

### 修飾類別
```ts
const Foo = (value: any) => {
  return (target: any) => {
    target.Foo = value;
    console.log(value);
  }
}

@Foo(123)
class Thing { }  // 這個類別會打印出 123
```

### 修飾屬性
```ts
const Foo = (value: any) => {
  return (target: any, key: any, descriptor: any) => {
    console.log(value);
    return console.log(target, key, descriptor);
  }
}

class Thing {
  @Foo('bar')
  baz() { }
}
```

### 修飾參數
```ts
const Foo = (target: any, key: any, index: number) => {
  console.log(key, index);
}

class Bar {
  print(@Foo baz): void {
    console.log(baz);
  }
}

new Bar().print(123);
```
