---
title: '값 복사와 변경'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/53.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

## 변수 선언과 값의 할당
  
  
> 자바스크립트는  
변수 선언을 통해  
변수에 대한 메모리 공간을 확보해서 변수의 이름과 연결하고  
값의 할당을 통해  
값에 대한 메모리 공간이 확보한 다음,  
값이 원시 타입일 경우 값 자체를 메모리 공간에 저장하고  
값이 참조 타입일 경우 값이 저장된 메모리 주소가 메모리 공간에 저장한다.  
그리고 해당 메모리 공간은 변수의 이름을 통해 참조되게 된다.  
>
  
  
<br><br><br>
## const 키워드로 선언된 변수
  
  
> const 키워드로 선언된 변수는 재할당이 금지된다.  
즉, 한 번 할당된 변수를 다시 다른 값으로 변경할 수 없다.  
>
  
  
> 원시 타입의 경우  
값 자체가 메모리 공간에 저장되기 때문에 const로 선언된 변수의 값을 변경하려면  
재할당이 필요하지만, 재할당이 금지되어 있기 때문에 변경할 수 없다.  
>
  
  
> 반면, 배열과 객체 같은 참조 타입의 경우  
변수에는 값 자체가 아닌 참조값(메모리 주소)가 저장된다.  
따라서, const 변수에 저장된 참조값은 변경할 수 없지만,  
그 참조값이 가리키는 객체나 배열 내부의 요소를 추가, 수정, 삭제하는 것은 가능하다.  
즉, const 변수에 저장된 배열이 가리키는 메모리 주소 자체는 변하지 않지만  
배열 내부의 요소는 다른 값을 가리키거나 변경될 수 있기 때문이다.  
>
  
  
```jsx
// 원시 타입의 const 변수
const str = "Hello";
str = "World";  // TypeError: Assignment to constant variable.

// 참조 타입(const로 선언된 배열)
const arr = [1, 2, 3];
arr[0] = 100;  // 내부 값 변경 가능
arr.push(4);   // 요소 추가 가능
arr.pop();     // 요소 삭제 가능
console.log(arr); // [100, 2, 3]

// 새로운 배열을 할당하려고 하면 오류 발생
arr = [4, 5, 6];  // TypeError: Assignment to constant variable.
```
  
<br><br><br>
## 변수에 데이터 전달
  
  
> 자바스크립트에서 변수에 데이터를 전달할 때,  
원시 타입의 데이터 전달 방식과 참조 타입의 데이터 전달 방식이 다르다.  
>
  
  
<br>
### 값에 의한 전달
  
> 원시 타입의 데이터는 값 자체가 복사되어 전달된다.  
>
  
  
```jsx
let a = 10;
let b = a;
b = 20;

console.log(a); // 10
console.log(b); // 20
```
  
<br>
### 참조에 의한 전달
  
> 참조 타입의 데이터는 참조값(메모리 주소)이 전달된다.  
>
  
  
```jsx
const obj1 = { name: "Dongsup" };
const obj2 = obj1;

obj2.name = "Kim";
console.log(obj1.name); // "Kim"
```
  
값에 의한 전달은 값 자체가 복사되어 전달되기 때문에  
원본 데이터와 독립적으로 동작하지만  
참조에 의한 전달은 참조값이 복사되어 전달되기 때문에  
여러 변수가 같은 객체를 가리키게 된다.  
따라서 객체 내부 값을 변경하면 모든 변수가 영향을 받게 되어, 불변성이 보장되지 않는다.  
  
<br><br><br>
## 불변성
  
  
> 불변성은 데이터가 한 번 생성되면 변경될 수 없는 성질로  
불변성을 유지하면 데이터 관리가 쉬워지고, 코드의 안정성과 성능이 향상된다.  
리액트 같은 프레임워크에서 상태 관리를 할 때 이 불변성이 중요하게 작용한다.  
>
  
<br>
  
**불변성을 유지해야 하는 이유**  
  
- **예측 가능한 코드**: 데이터가 변경되지 않으면 코드의 동작도 예측 가능해진다.  
값이 변하지 않으므로 코드 실행 도중 의도치 않은 변경 사항이 발생할 위험이 줄어든다.  
- **성능 향상**: 불변한 데이터는 변할 필요가 없으므로, 변경 시 발생하는 연산 비용을 줄일 수 있다. 이로써 코드 실행 속도가 향상될 수 있다.  
- **디버깅의 간편함**: 불변성을 유지하면 이전 데이터를 보존하면서 새로운 데이터를 만들기 때문에, 디버깅과 히스토리 관리가 쉬워진다.  
  
<br>

**불변성을 유지하는 방법**  
  
- 원시 데이터 타입 사용: 숫자나 문자열과 같은 원시 데이터 타입은 불변하기 때문에 변수 값을 갱신하려면 새로운 값을 할당해야 한다.  
- 복제: 객체를 다룰 때는 원본 객체를 직접 수정하지 않고, 새로운 객체를 생성하고 수정해야한다.  
  
<br><br><br>
## 깊은 복사와 얕은 복사
  
  
> 깊은 복사와 얕은 복사  
자바스크립트에서 객체(참조형 데이터)를 복사하는 방식에는 깊은 복사와 얕은 복사가 있다.  
>
  
  
<br>
### 얕은 복사 (Shallow Comparison)
  
> 얕은 복사는 객체의 최상위 프로퍼티까지만 새로운 객체에 복사하고,  
내부의 참조형 데이터(객체, 배열 등)는 기존 참조값을 공유하는 방식이다.  
객체의 바로 아래 단계(depth 1)의 프로퍼티들만 복사되고,  
내부의 참조형 데이터는 원본과 같은 메모리 주소를 가리킨다.  
>
  
  
```jsx
const original = { a: 1, b: { c: 2 } };
const shallowCopy = { ...original }; // 얕은 복사

shallowCopy.b.c = 99;
console.log(original.b.c); // 99 (원본도 변경됨)
console.log(shallowCopy.b.c); // 99 (같은 객체를 참조)
```
  
{:.notice--primary}
💡 **`전개구문`**<br><br>
전개구문은 얕은 복사가 이루어지지만,  
실질적으로  depth가 1일때는 원본 객체의 직접적인 프로퍼티들의 참조가 끊어지고 새로운 객체에 복사되는 것이므로 깊은 복사나 다름이 없다.  
depth가 2부터는 얕은 복사가 일어난다고 할 수 있다.  
  
  
{:.notice--primary}
💡 **`얕은 복사의 문제점`**<br><br>
객체 내부에 다른 객체가 포함되어 있다면 (depth 2 이상), 내부 객체는 원본과 동일한 참조값을 가지므로 불변성이 깨질 수 있다.  
원본 객체의 내부 값이 예상치 못하게 변경될 수 있어 버그를 유발할 가능성이 크다.  
  
  
<br>
### 깊은 복사 (Deep Comparison)
  
> 깊은 복사는 원본 객체와 완전히 독립적인 새로운 객체를 생성하는 것을 의미한다.  
즉, 복사된 객체는 원본 객체와 참조를 공유하지 않으며,  
내부의 모든 참조형 데이터(객체, 배열 등)도 새로운 메모리 공간에 생성되는 것을 의미한다.  
깊은 복사를 하면 내부 객체도 새로운 메모리에 저장되므로, 원본이 변경되지 않는다.  
>
  
<br>
  
**재귀함수**  
  
  
> for...in 문을 사용하여 객체의 모든 프로퍼티를 순회하고  
만약 값이 객체 또는 배열이면 재귀 호출하여 깊은 복사 수행하는 방식이다.  
>
  
  
```jsx
function deepClone(obj) {
  // 원시 값(Primitive Type) 또는 null이면 그대로 반환
  if (obj === null || typeof obj !== "object") {
    return obj;
  }

  // 배열(Array)인 경우, 빈 배열을 만들어 재귀 복사
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }

  // 객체(Object)인 경우, 빈 객체를 만들어 재귀 복사
  const clonedObj = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) { // 프로토타입 상속된 프로퍼티 제외
      clonedObj[key] = deepClone(obj[key]);
    }
  }

  return clonedObj;
}
```

<br>
  
**JSON.parse(JSON.stringify(obj))**  
  
  
> 객체를 JSON 문자열로 변환(JSON.stringify)후  
다시 객체로 변환(JSON.parse)하여 새로운 객체를 생성하는 방식으로  
내부 객체까지 새로운 객체로 만들어지므로 깊은 복사(Deep Copy)가 이루어진다.  
>
  
  
> 하지만 JSON.parse(JSON.stringify(obj)) 방식은  
함수(function), undefined, Symbol 같은 값은 복사되지 않고 제거된다.  
또한, Date 객체는 문자열로 변환되며, Map, Set, RegExp 같은 특수 객체도 올바르게 복사되지 않는다.  
이 방식은 순수한 JSON 데이터를 다룰 때만 적합하다.  
>
  
  
```jsx
const obj = {
  name: "Alice",
  sayHello: function() { return "Hello!"; },
  nested: { age: 25 },
  date: new Date(),
  undef: undefined
};

const copied = JSON.parse(JSON.stringify(obj));

console.log(copied.sayHello); // undefined (함수가 제거됨)
console.log(copied.date); // 날짜가 문자열로 변환됨
console.log(copied.undef); // undefined도 제거됨
```
  
<br>

**Immer 라이브러리 사용**  
  
  
> 불변성을 유지할 수 있도록 도와주는 라이브러리이다.  
produce 함수 내부에서 draft 객체를 변경하면 새로운 객체가 생성되는데  
이를 통해서 깊은 복사를 할 수 있다.  
>
  
  
```jsx
import produce from "immer";

const original = {
  name: "Dongsup",
  nested: { hobby: "Reading" }
};

const updated = produce(original, draft => {
  draft.nested.hobby = "Gaming";
});

console.log(original.nested.hobby); // "Reading" (원본 유지)
console.log(updated.nested.hobby); // "Gaming" (불변성을 유지하며 변경)
```
  
<br><br><br>
## 리액트에서 state 변경 감지 방식
  
  
> 리액트는 Call Stack이 아니라 Heap 메모리에서 객체의 참조값(메모리 주소)이 변경되었는지를 비교하여 상태 변화를 감지한다.  
즉, 상태 변화 감지는 얕은 비교를 통해 이루어진다는 것과 같은 이야기다.  
>
  
  
{:.notice--primary}
💡 **`왜 리액트는 깊은 비교 대신 얕은 비교를 사용할까?`**<br><br>
바로 성능 최적화 때문이다.  
만약 객체의 깊이가 1000이라면, 999번째 깊이의 값이 변경되었는지를 확인하려면 전체 객체를 깊은 비교 해야 한다.  
하지만 얕은 비교는 객체의 참조값(주소)만 비교하면 되므로 훨씬 빠르다.  
  