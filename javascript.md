# Contents

1. [변수](#1-변수)  
   1-1. [Data Type](#1-1-data-type)  
   1-2. [const](#1-2-const)

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
