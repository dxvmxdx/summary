#### 들어가기 전 checklist

```shell
node -v
npm -v
npm i -g typescript # typescript install
tsc -v
```

<br>
<br>
<br>

# Contents

1. [Types](#1-types)  
   1-1. [기본 타입](#1-1-기본-타입)  
   1-2. [배열](#1-2-배열과-튜플)  
   1-3. [Type Alias](#1-3-type-alias)

<br>
<br>
<br>
<br>
<br>

# 1. Types

## 1-1. 기본 타입

```ts
// number
const num: number = 10;

// string
const str: string = 'hello, world!';

// boolean
const isBoolean: boolean = true;

// undefined
let und: undefined; // 💩
let age: number | undefined; // 👍🏻

// null
let nul: null; // 💩
let title: string | null;
```

```ts
// void
// 함수에서 값을 리턴하지 않을 때 사용
function print(): void {
  console.log('hello');
  return;
}

// never
// 함수에서 절대 리턴되지 않을 때 사용
function throwError(message: string): never {
  // message -> server(log)
  throw new Error(message);
}

// unknown 💩
// 알 수 없는 타입.
let notSure: unknown = 123;
notSure = 'hello';
notSure = 123;

// any 💩
// 어떤 타입의 값도 할당할 수 있다.
let anything: any = 0;
anything = 'world!';

// object 💩
// 원시타입을 제외한 모든 타입을 할당할 수 있다.
let obj: object; //
function acceptSomeObject(obj: object) {}
```

<br>
<br>
<br>

## 1-2. 배열과 튜플

```ts
// Array
const fruit: string[] = ['🍎', '🍓', '🍌'];
const score: Array<number> = [1, 3, 4];

// Tuple
// 서로 다른 타입을 담을 수 있다. 💩
let student: [string, number];
student = ['name', 123];
student[0]; // name
student[1]; // 123
```

<br>
<br>
<br>

## 1-3. Type Alias

```ts
type Student = {
  name: string;
  age: number;
};

const student: Student = {
  name: 'steve',
  age: 20,
};
```
