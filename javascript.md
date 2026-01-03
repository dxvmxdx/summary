# Contents

1. [변수](#1-변수)  
   1-1. [Data Type](#1-1-data-type)  
   1-2. [const](#1-2-const)
2. [연산자](#2-연산자)  
   2-1. [논리연산자](#2-1-논리연산자)  
   2-2. [옵셔널 체이닝 ?.](#2-2-옵셔널-체이닝)
3. [제어문](#3-제어문)  
   3-1. [switch](#3-1-switch)

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
