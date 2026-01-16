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
