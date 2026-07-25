# Contents

1. [변수](#1-변수)  
   1-1. [Data Type](#1-1-data-type)  
   1-2. [const](#1-2-const)
2. [연산자](#2-연산자)  
   2-1. [논리연산자](#2-1-논리연산자)  
   2-2. [옵셔널 체이닝 ?.](#2-2-옵셔널-체이닝)
3. [제어문](#3-제어문)  
   3-1. [switch](#3-1-switch)
4. [함수](#4-함수)  
   4-1. [parameter](#4-1-parameter)  
   4-2. [callback](#4-2-callback-function)
5. [클래스](#5-클래스)  
   5-1. [static](#5-1-static)  
   5-2. [field](#5-2-field)  
   5-3. [getter & setter](#5-3-getter--setter-접근자-프로퍼티)  
   5-4. [상속](#5-4-상속)
6. [내장 객체](#6-내장-객체)  
   6-1. [Date](#6-1-date)
7. [Iterable](#7-iterable)  
   7-1. [spread](#7-1-spread)
8. [Destructuring](#8-destructuring)
9. [모듈](#9-모듈)
10. [비동기](#10-비동기)  
    10-1. [Promise](#10-1-promise)  
    10-2. [async](#10-2-async)
11. [Scope](#11-scope)  
    11-1. [렉시컬 환경](#11-1-렉시컬-환경)  
    11-2. [Closure](#11-2-closure)  
    11-3. [Hoisting](#11-3-hoisting)
12. [this 바인딩](#12-this-바인딩)
13. [자바스크립트 동작 원리](#13-자바스크립트-동작-원리)  
    13-1. [자바스크립트 엔진](#13-1-자바스크립트-엔진)  
    13-2. [런타임](#13-2-런타임)  
    13-3. [이벤트 루프 동작 방식](#13-3-이벤트-루프-동작-방식)

<br>
<br>
<br>
<br>
<br>

# 1. 변수

변수에 값을 할당하면 그 값이 메모리 셀에 저장되고, 변수 이름은 해당 메모리 주소를 가리킨다.

<br>

## 1-1. Data Type

- primitive  
  Number, BigInt, String, Boolean, Undefined, Null, Symbol
- object  
  Object, Function, Array, Map, Set, Date, RegExp, ...

<br>

> ⭐️ 값과 참조의 차이  
> primitive 타입은 메모리 셀(stack)에 값 자체가 저장되지만, object 타입은 실제 객체가 들어있는 메모리(heap) 주소가 스택에 저장된다.

<br>
<br>
<br>

## 1-2. const

```js
// 상수
const MY_FAV = 7;

// 재할당 불가능한 상수변수 또는 변수
const bob = {
  name: 'bob',
  age: 19,
};
```

<br>
<br>
<br>
<br>
<br>

# 2. 연산자

## 2-1. 논리연산자

`&&`: 모든 피연산자가 `true`일 경우 `true`, 그렇지 않으면 `false` 반환  
`||`: 하나 이상이 `true`면 `true`, 그렇지 않으면 `false` 반환  
`!`: truthy를 `false`로, falsy를 `true`로 반환  
`x ?? y`: x가 `null` 또는 `undefined`일 때 y 반환, 그렇지 않으면 x 반환

<br>

### 단락 평가

표현식을 평가하는 도중에 2번째 피연산자까지 평가하지 않아도 결과가 확정된 경우, 나머지 평가 과정을 생략하는 것.

```js
// x && y
// x 값이 falsy면 x 반환, 그렇지 않으면 y 값 반환
// 💡 조건이 truthy일 때 && 실행할 코드
const userName = user && user.name;

// x || y
// x 값이 truthy면 x 반환, 그렇지 않으면 y 반환
// 💡 조건이 falsy일 때 || 실행할 코드
let name = '';
const user = name || '방문자';
```

<br>
<br>
<br>

## 2-2. 옵셔널 체이닝

`x?.prop`: x 값이 nullish(`null` 또는 `undefined`)면 `undefined`를 반환하고, 그렇지 않으면 `obj.prop` 값 반환.

> ⚠️ obj 값이 선언되어 있지 않으면 에러가 발생하므로, obj 값이 반드시 존재하고 prop가 필수값이 아닌 경우에 사용해야 한다.

```js
const customer = {
  name: 'Carl',
  details: { age: 82 },
};
const customerCity = customer?.city ?? 'Unknown city';
```

<br>
<br>
<br>
<br>
<br>

# 3. 제어문

## 3-1. switch

#### 실행순서

① 표현식 평가  
② 결과값과 일치하는 case문으로 이동  
③ break문을 만나면 switch문 종료, 만나지 못하면 다음 case문 실행

```js
const expr = 'Papayas';

switch (expr) {
  case 'Oranges':
    console.log('Oranges are $0.59 a pound.');
    break;
  case 'Mangoes':
  case 'Papayas':
    console.log('Mangoes and papayas are $2.79 a pound.');
  // break;
  default:
    console.log(`Sorry, we are out of ${expr}.`);
}

// Expected output:
// "Mangoes and papayas are $2.79 a pound."
// "Sorry, we are out of Papayas."
```

<br>
<br>
<br>
<br>
<br>

# 4. 함수

## 4-1. parameter

> 매개변수(parameter): 전달된 데이터를 할당받아 함수 내부에서 사용할 수 있는 <u>변수</u>.  
> 인수(argument): 함수 호출 시 매개변수에 전달된 <u>값</u>.

<br>

### default parameters

함수의 매개변수는 `undefined`가 기본으로 설정된다. 매개변수에 아무 값도 주지 않으면 `undefined`가 되므로 기본값을 설정해주면 된다.

```js
function multiply(a, b = 1) {
  return a * b;
}

multiply(5); // 5
```

<br>

### rest parameters

매개변수 이름 앞에 `...`을 붙여 정의한 매개변수로, 먼저 선언된 파라미터에 할당된 인수를 제외한 나머지 인수들의 목록을 배열로 받는다.

> ⚠️ 마지막에 위치해야 한다.

```js
function multiply(multiplier, ...theArgs) {
  return theArgs.map((x) => multiplier * x);
}

const arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

<br>
<br>
<br>

## 4-2. callback function

다른 함수의 argument로 전달된 함수.

```js
function doSomething(callback) {
  setTimeout(callback, 0);
}
```

<br>

> 고차함수: 함수를 반환하거나 다른 함수를 argument로 받는 함수.

<br>
<br>
<br>
<br>
<br>

# 5. 클래스

객체를 생성하기 위한 템플릿.

```js
class Fruit {
  // 생성자 함수: new 키워드로 인스턴스를 생성할 때 (자동으로) 호출되는 함수
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }

  display() {
    console.log(`${this.name}: ${this.emoji}`);
  }
}

const apple = new Fruit('apple', '🍎');
```

<br>
<br>
<br>

## 5-1. static

프로퍼티와 메서드 앞에 `static` 키워드를 붙이면, 클래스에 정의하고 재사용할 수 있다.  
클래스의 인스턴스에서는 접근•호출할 수 없다.

> 💡 클래스에 공통적으로 사용하거나 인스턴스 데이터에 참조할 필요가 없을때, 사용할 수 있다.

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  // 정적 프로퍼티
  static displayName = 'Point';
  // 정적 메소드
  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.hypot(dx, dy);
  }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
Point.displayName;
Point.distance(p1, p2);
```

<br>
<br>
<br>

## 5-2. field

클래스가 생성할 인스턴스 프로퍼티

<br>

### public field

```js
class Fruit {
  // constructor에서 초기화하면 생략 가능
  // name;
  // emoji;
  type = '과일';

  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }
}

const apple = new Fruit('apple', '🍎');
console.log(apple);
// Expected output:
// Fruit {
//   type: '과일',
//   display: [Function: display],
//   name: 'apple',
//   emoji: '🍎'
// }
```

<br>

### private field

`#` 접두사를 사용하여, 클래스 내부에서만 접근 가능하고 외부에서 접근하지 못하게 막는다.

```js
class Person {
  #name;

  constructor(name) {
    this.#name = name;
  }

  #display() {
    console.log(`이름: ${this.#name}`);
  }

  show() {
    this.#display();
  }
}

const person = new Person('Alice');
person.show();
```

<br>
<br>
<br>

## 5-3. getter & setter (접근자 프로퍼티)

> ⚠️ 데이터 프로퍼티명과 접근자 프로퍼티명을 다르게 설정해야 한다.

```js
class User {
  #name;
  #age;

  constructor(name, age) {
    this.#name = name;
    this.#age = age;
  }

  get name() {
    console.log(`사용자의 이름은 ${this.#name}입니다.`);
  }

  get age() {
    console.log(`사용자의 나이는 ${this.#age}입니다.`);
  }

  set age(newAge) {
    if (newAge < 0) {
      console.log('입력하신 숫자가 0 미만입니다. 다시 설정해주세요.');
      return;
    }
    this.#age = newAge;
  }
}

const snow = new User('설윤', 20);
snow.name;
snow.age = -2;
snow.age;
```

<br>
<br>
<br>

## 5-4. 상속

`extends` 키워드로 다른 클래스를 상속받을 수 있다.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // super(...): 부모 클래스의 생성자 함수 호출
  }

  speak() {
    super.speak(); // super.method(); 부모 클래스에서 정의한 메서드 호출
    console.log(`${this.name} barks.`);
  }
}

const mitzie = new Dog('Mitzie');
mitzie.speak();
// Expected output:
// "Mitzie makes a noise."
// "Mitzie barks."
```

<br>
<br>
<br>
<br>
<br>

# 6. 내장 객체

## 6-1. Date

```js
new Date();
new Date(1997, 0, 1, 21, 30, 0);

// 문자열
new Date().toLocaleString('ko-KR'); // "2026. 1. 12. 오후 2:41:59"
```

<br>

```js
// 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환
Date.now();

// 밀리초 -> Date
new Date(Date.now());
// Date -> 밀리초
Date.parse(new Date());
```

<br>
<br>
<br>
<br>
<br>

# 7. Iterable

순회가 가능한 객체로, `for...of`, `spread` 연산자를 사용할 수 있다.

<br>

## 7-1. spread

spread 문법(`...`)은 iterable를 각 개별 item으로 분리한다.

```js
// function argument
function sum(x, y, z) {
  return x + y + z;
}
const numbers = [1, 2, 3];
sum(...numbers);

// array
const arr = [1, 2, 3];
const copy = [...arr];

// 객체 리터럴도 적용되도록 추가되었다. (ECMAScript 2018)
{ ...{ x: 1, y: 2 }, y: 100 }; // { x: 1, y: 100 }
```

<br>
<br>
<br>
<br>
<br>

# 8. Destructuring

배열이나 객체를 destructuring해 개별 변수에 할당하는 것

```js
// 배열
const [a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

// 객체
const john = { name: 'john', age: 20, job: 's/w' };
const { name, age, job: occupation, pet = '강아지' } = john;
console.log(occupation); // 's/w'

// 중첩된 객체 구조분해할당
const person = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul',
  },
};
const {
  address: { city },
} = person;
console.log(city); // 'Seoul'
```

<br>
<br>
<br>
<br>
<br>

# 9. 모듈

## 9-1. export

변수나 함수, 클래스를 선언할 때 앞에 `export` 키워드를 붙이면 내보낼 수 있다.

```js
export const name = 'square';

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, length, length);

  return { length, x, y, color };
}
```

또는 모듈 파일의 끝에 한번에 내보낼 수도 있다.

```js
export { name, draw };
```

`as`를 이용해 이름을 바꿔서 내보낼 수도 있다.

```js
export { name as squareName };
```

<br>
<br>
<br>

## 9-2. import

`export` 된 이름을 `import`로 가져올 수 있다.

```js
import { name, draw } from './square.js';
```

한꺼번에 다 가져올 수도 있다.

```js
import * as square from './square.js';

console.log(square.name);
```

`as`를 이용해 이름을 바꿔서 가져올 수도 있다.

```js
import { name as squareName } from './square.js';
```

<br>
<br>
<br>

## 9-3. default export

대표적으로 export 할 것이 있거나 하나만 export 할 때 `export default`를 사용한다.

```js
export default function (user) {
  // 이름이 없어도 됨.
  console.log(`Hello, ${user}!`);
}
```

```js
import greeting from './myModule.js'; // 중괄호 생략 가능

console.log(greeting('john'));
```

default export와 named exports를 동시에 가져올 수 있다.

```js
import React, { Component, Fragment } from 'react';
```

<br>
<br>
<br>
<br>
<br>

# 10. 비동기

자바스크립트는 싱글 스레드 기반의 언어로, 한번에 하나의 작업만 수행한다. "동기"  
그렇지만 host 환경에서 제공하는 API를 통해서 비동기적으로 코드를 수행할 수 있다.

```js
function execute() {
  console.log(1);
  setTimeout(() => {
    console.log(2);
  }, 3000);
  console.log(3);
}

execute();
// Expected output:
// 1
// 3
// 2 (3초 후 실행)
```

<br>
<br>
<br>

## 10-1. Promise

콜백 방식의 비동기 처리가 갖는 문제점(콜백헬, 에러처리한계)을 극복하기 위해 Promise가 제안됨.

The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.  
Promise 객체는 비동기 작업의 완료(또는 실패)와 그 결과 값을 나타냅니다.

<br>

### 프로미스 객체 생성

프로미스 생성자 함수는 콜백함수(executor)를 인수로 받는다. 이 콜백함수는 인수로 `resolve` 함수와 `reject` 함수를 받는다.

executor는 내부에서 비동기 처리 작업을 수행한다. 이때 비동기 작업이 끝나면 `resolve` 함수를 호출하고, 에러가 발생하면 `reject` 함수를 호출한다.

- `resolve(value)`  
  비동기 처리 결과를 인수로 전달
- `reject(error)`  
  에러 객체를 인수로 전달

프로미스는 3가지 상태 정보를 갖는데, 처음엔 '**pending**' 상태였다가  
비동기 처리가 성공하면(resolve 함수 호출) '**fulfilled**' 상태가 되고,  
실패하면(reject 함수 호출) '**rejected**' 상태가 된다.

```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('hello');
  }, 3000);
});

p.then(console.log); // 3초 뒤 hello 출력
console.log(p); // pending 상태의 Promise 객체 출력
```

<br>

### 후속 처리 메소드

프로미스 객체의 결과값을 사용해 추가작업을 하려면 `then`, `catch`, `finally` 메소드를 호출하면 된다.

#### then()

두 개의 콜백함수를 인수로 받는다.  
첫번째 콜백함수는 성공 시 호출되며 인수로 성공 결과를 받고, 두번째 콜백함수는 실패 시 호출되며 인수로 에러객체를 받는다. (그렇지만 가독성을 위해 `catch()` 메소드 사용 권장)

`then()` 메소드는 새로운 프로미스 객체를 반환한다. 이때 콜백함수에서 반환한 값이 프로미스 객체의 결과값이 된다. (콜백함수에서 반환한 값이 프로미스라면 그 프로미스를 그대로 반환한다.)

```js
const p = new Promise((resolve, reject) => {
  resolve('Success!');
});

p.then(console.log); // 'Success!'
```

#### catch()

실패 시 호출되며, 새로운 프로미스 객체를 반환한다. (콜백함수에서 반환한 값이 프로미스라면 그 프로미스를 그대로 반환한다.)

```js
const p = new Promise((resolve, reject) => {
  throw new Error('Uh-oh!');
});

const result = p.catch((error) => console.log(error.message)); // Uh-oh!
setTimeout(() => console.log(result), 3000); // Promise { undefined } ‼️
```

#### finally()

프로미스의 결과에 관계없이 실행될 콜백을 등록하며, 이전 프로미스 결과값이 전달되지 않는다.  
새로운 프로미스 객체를 반환하며, 원래 프로미스의 최종 상태를 변경하지 않는다.  
(⚠️ `finally` 콜백 내 예외가 발생하거나 rejected 프로미스를 반환하는 경우에는, 그 값을 가진 rejected 프로미스 객체를 반환한다.)

```js
function checkMail() {
  return new Promise((resolve, reject) => {
    resolve('Mail has arrived');
  });
}

checkMail()
  .then((response) => response)
  .catch(console.error)
  .finally(() => {
    console.log('Experiment completed');
  })
  .then((result) => console.log(result)); // Mail has arrived
```

<br>
<br>
<br>

## 10-2. async

함수 앞에 `async` 키워드를 붙이면 비동기 함수가 되고, 프로미스를 반환한다.  
async 함수 내에서 `await` 키워드를 쓸 수 있는데, **`await` 뒤에 오는 프로미스를 반환하는 비동기 작업이 완료될**까지 async 함수의 실행을 중단시킨다.  
⚠️ `await` 연산자는 결과값을 반환한다. 만약 비동기 작업이 에러를 발생시킨다면 코드 실행이 멈추기 때문에 에러 처리를 해줘야 한다.

```js
function resolveAfter2Seconds() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  // Expected output: "resolved"
}

asyncCall() //
  .then(console.log); // undefined
```

<br>

### 에러 핸들링

`try ... catch` 를 이용해 처리한다.

```js
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.reject(new Error('치킨을 가지고 올 수 없음!'));
  // return Promise.resolve(`🪴 => 🐓`);
}

async function makeFriedEgg() {
  let chicken;
  try {
    chicken = await getChicken();
  } catch {
    chicken = '🐔';
  }
  const egg = await fetchEgg(chicken);
  return fryEgg(egg);
}

makeFriedEgg() //
  .then(console.log);
```

<br>
<br>
<br>
<br>
<br>

# 11. Scope

식별자(변수, 함수, 클래스 이름)가 유효한 범위.

```js
let y = 0;
// 코드 블록 안의 변수는 그 안에서만 유효
{
  let y = 1;
  console.log(y); // 1
}
console.log(y); // 0
```

<br>
<br>
<br>

## 11-1. 렉시컬 환경

모든 함수, 코드 블록 `{...}`, 전체 스크립트는 '렉시컬 환경' 이라는 숨겨진 내부 오브젝트를 갖는다.

환경 레코드와 외부 렉시컬 환경에 대한 참조로 구성됨.

- 환경 레코드: 현재 스코프에 정의된 모든 변수와 함수 선언을 저장하는 객체.
- 외부 렉시컬 환경에 대한 참조: 상위 렉시컬 환경의 참조값 (💡외부 변수에 접근할 수 있게 된다.)

<br>
<br>
<br>

## 11-2. Closure

클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경과의 조합이다.

클로저는 함수가 선언될 당시의 렉시컬 환경을 기억하고 있어, 자신이 선언됐을 때의 환경 밖에서 호출되어도 그 환경에 접근할 수 있다.

```js
function counter() {
  let count = 0; // private
  function increse() {
    count++;
    console.log(count);
  }
  return increse;
}

const increse1 = counter();
const increse2 = counter();
console.log(increse1 === increse2); // false
```

<br>
<br>
<br>

## 11-3. Hoisting

인터프리터가 코드를 실행하기 전에 변수, 함수, 클래스 또는 `import`의 선언문을 해당 scope의 맨 위로 끌어올리는 것처럼 보이는 현상.

🔍  
코드 실행 전 렉시컬환경이 생성되고 렉시컬환경의 환경레코드에 식별자 정보가 저장되는데, 이때문에 실행 전 식별자 정보를 알게 된다.

함수 선언문으로 선언한 함수는 선언과 동시에 생성되며, `var` 로 선언된 변수는 `undefined`으로 할당되나,  
`let`과 `const`로 할당된 변수, 그리고 class는 uninitialized 상태다. (메모리 공간만 잡아둠)

때문에 함수 선언문으로 선언된 함수와 `var`로 선언된 변수는 선언 이전에 호출해도 에러가 발생하지 않지만, 함수표현식으로 선언된 함수와 class, `let`, `const`로 선언된 변수는 참조할 메모리 값이 없으므로 호출 시 에러가 발생한다.

<br>
<br>
<br>
<br>
<br>

# 12. this 바인딩

함수 호출 방식에 따라 this에 바인딩 되는 객체가 **동적**으로 결정된다.

1. global scope에서 `this`는 전역 객체(`window`)를 가리킨다.

2. 함수 내부에서의 `this`는 호출 방식에 따라 달라진다. (`this`는 함수를 호출한 객체이다.)
   - 전역 범위에서 호출한 함수 내부의 `this`는 전역 객체를 가리킨다.
     단, strict mode인 경우에는 `undefined`로 바인딩된다.

     ```js
     function func() {
       console.log(this);
     }
     func(); // = window.func(); --> this: window 객체
     ```

   - 객체의 메소드로 호출한 메소드 내부의 `this`는 해당 메소드를 호출한 객체를 가리킨다.

     ```js
     function bar() {
       console.log(this);
     }
     const foo = {
       a: 20,
       bar,
     };

     foo.bar(); // foo 객체
     const bar2 = foo.bar;
     bar2(); // window 객체 ‼️
     ```

<br>

### 화살표함수 내부의 this

함수 선언 시점의 상위 컨텍스트의 `this`를 가리킨다.

```js
const foo = {
  a: 20,
  bar() {
    setTimeout(() => {
      console.log(this);
    }, 1);
  },
};
foo.bar(); // foo 객체
```

```js
const obj = {
  getThisGetter() {
    const getter = () => this;
    return getter;
  },
};

const fn = obj.getThisGetter();
console.log(fn() === obj); // true
```

<br>
<br>
<br>
<br>
<br>

# 13. 자바스크립트 동작 원리

## 13-1. 자바스크립트 엔진

자바스크립트 엔진은 자바스크립트 코드를 실행하는 프로그램 또는 인터프리터이다.  
엔진의 주요 구성 요소로는 memory heap과 call stack이 있다.

- memory heap  
  참조 타입의 데이터가 할당되는 메모리 영역
- call stack  
  코드가 실행되며 생성되는 실행 컨텍스트를 저장하는 자료구조. (LIFO)

#### 실행 컨텍스트

실행할 코드에 제공할 환경 정보들을 모아놓은 객체.

- variable environment
- lexical environment
- this binding

<br>

> 자바스크립트 엔진이 처음 코드를 실행하면 전역 실행 컨텍스트가 생성되고 이를 콜스택에 push한다. 이후 함수가 호출될 때마다 함수 실행 컨텍스트가 생기고 콜스택에 push한다. 함수가 종료되면 해당 컨텍스트는 콜스택에서 pop된다.

<br>
<br>
<br>

## 13-2. 런타임

자바스크립트 엔진은 싱글 스레드 기반으로 한 번에 하나의 작업만 처리한다. (동기)  
그러나 자바스크립트가 구동되는 환경 즉, 자바스크립트 런타임 환경(브라우저, Node.js)에서 지원하는 APIs로 비동기 처리를 할 수 있다.

<img width="600" height="400" alt="js_browser_runtime" src="https://github.com/user-attachments/assets/c64fff80-6bd1-41e3-86ea-e57bc07e4b9e" />

<br>

#### Callback Queue

비동기 작업이 완료된 후 실행될 콜백 함수가 대기하는 큐 (FIFO)

- Microtask Queue  
  `promise.then`, `process.nextTick`, `MutationObserver` 와 같이 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐
- Animation Frames  
  `requestAnimationFrame()` 에 등록한 콜백 함수가 들어가는 큐
- Task Queue  
  `setTimeout`, `setInterval`, `fetch`, `addEventListener` 와 같이 비동기로 처리되는 함수들의 콜백 함수가 들어가는 큐

<br>

### 비동기 코드 실행 순서

비동기 함수가 호출되면,  
① 콜스택에 쌓이고 web api에 실행 요청을 보냄  
② 비동기 함수는 콜스택에서 제거됨  
③ 콜스택의 다음 함수 실행  
④ 백그라운드에서 web api 작업이 완료되면 이벤트 루프가 실행될 콜백 함수를 콜백 큐에 전달  
⑤ 콜스택이 비게 되면, 이벤트 루프가 콜백 큐에서 대기 중인 콜백 함수를 콜스택에 push  
⑥ 콜스택에 넘겨진 콜백 함수가 실행

<br>
<br>
<br>

## 13-3. 이벤트 루프 동작 방식

콜스택이 비게 되면, 🔄 순회하다가 (한바퀴 도는데 1ms도 안 걸림)

- 16.7ms 이내에서 렌더링 작업을 업데이트 한다. (60fps;1프레임 = 16.7ms)  
  animation frames의 콜백 함수가 다 없어질 때까지 머물고, 렌더링 작업을 마무리한다.  
  다시 순회한다.

- 마이크로태스크 큐에 콜백 함수가 다 없어질 때까지 마이크로태스크 큐에 머물러 있는다.  
  만약 콜스택에 넘겨진 콜백 함수가 끝나기 전에 마이크로태스크 큐에 또 다른 콜백 함수가 추가된다면, 바로 추가된 함수가 실행이 된다.  
  다시 순회한다.

- 태스크 큐에 있는 콜백 함수 하나를 콜스택에 push한다.  
  다시 순회한다.

#### 실행 순서 예제

```js
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve() //
  .then(() => {
    console.log('Promise1');
  })
  .then(() => {
    console.log('Promise2');
  });

requestAnimationFrame(() => {
  console.log('animation frames');
});

console.log('End');

// Expected output:
// Start
// End
// Promise1
// Promise2
// animation frames <- 호출되는 순서 다를 수 있음
// Timeout
```

```js
async function fetchData() {
  console.log(1);
  await Promise.resolve();
  console.log(2);
}

fetchData();
console.log(3);

// Expected output:
// 1
// 3
// 2
```

<br>

> ⚠️ 콜백 함수 내 DOM 요소의 스타일 또는 위치를 변경시키는 코드는 브라우저에 바로 적용되지 않는다.  
> (<u>콜백 함수가 다 끝난 후 렌더링 작업이 일어나므로</u>)

```js
const button = document.querySelector('button');
const box = document.querySelector('box');
button.addEventListener('click', () => {
  box.style.backgroundColor = 'pink'; // 브라우저에서 pink 색깔로 바뀌는게 보이지 않음
  box.style.backgroundColor = 'orange';
});
```
