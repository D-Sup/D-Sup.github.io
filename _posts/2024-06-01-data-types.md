---
title: '데이터 타입'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/57.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

## 데이터 타입
  
  
> 자바스크립트는 동적 타입(dynamic typing) 을 가지는 언어다.  
즉, 변수를 선언할 때 타입을 명시할 필요가 없으며, 실행 중에 타입이 자동으로 변환될 수 있다.  
>
  
  
```jsx
let value = 42;    // Number
value = "Hello";   // String
value = true;      // Boolean
```
  
> 자바스크립트의 데이터 타입은   
기본(원시) 타입과   
참조(객체) 타입으로 나뉜다.  
>
  
  
<br>
### 기본 타입 (Primitive Type)
  
> 기본 타입은 값 자체가 저장되며, 불변성(immutable) 을 가진다.  
즉, 값을 변경하면 새로운 메모리 공간에 저장되며, 기존 메모리는 수정되지 않는다.  
>
  
  
| 타입 | 설명 | 예시 |  
| --- | --- | --- |  
| **Number** | 정수 및 실수 값 | `let a = 42; let b = 3.14;` |  
| **BigInt** | 2^53보다 큰 정수 표현 | `let big = 9007199254740991n;` |  
| **String** | 문자열 | `"Hello"` , `'World'` |  
| **Boolean** | 논리값 | `true`, `false` |  
| **Undefined** | 값이 할당되지 않은 변수 | `let x; console.log(x); // undefined` |  
| **Null** | 의도적으로 "값이 없음"을 표현 | `let y = null;` |  
| **Symbol** | 고유한 식별자를 생성하는 타입 | `let sym = Symbol("unique");` |  
| **Number**: | `Infinity`, `NaN` 이 포함된다.  |
| **String**: | 변경 불가능한(immutable) 문자 데이터  |
| **BigInt**: | `+`, `/`, `*` 등 연산 가능하지만, `Number`와 혼합이 불가능하다.  |
| **Symbol**: | 객체의 속성 키로 사용 가능하고 고유한 값을 보장 한다.  |
  
<br>
### 참조 타입 (Reference Type)
  
> 참조 타입은 값이 아니라 메모리 주소(참조값)가 저장되며, 가변성(mutable) 을 가진다.  
즉, 객체의 속성을 변경해도 메모리 주소는 변하지 않는다.  
>
  
  
| 타입 | 설명 | 예시 |  
| --- | --- | --- |  
| **Object** | 키-값 구조의 데이터 저장 | `{ key: "value" }` |  
| **Array** | 순서가 있는 리스트 | `[1, 2, 3]` |  
| **Function** | 실행 가능한 객체 | `function foo() {}` |  
| **Set** | 중복 없는 값 저장 | `new Set([1, 2, 3])` |  
| **Map** | 키-값 저장 가능 (키로 객체도 가능) | `new Map([["name", "Alice"]])` |  
  
<br>
### 객체 래퍼 클래스
  
> 자바스크립트의 원시 타입(`String`, `Number`, `Boolean`)은 자동으로 객체처럼 동작할 수 있다.  
이러한 동작을 가능하게 하는 것이 객체 래퍼(wrapper) 클래스이다.  
>
  
  
```jsx
let str = "Hello";
console.log(str.length); // 5
console.log(str.toUpperCase()); // "HELLO"
```
  
- `"Hello"`는 원시 타입이지만, `.length`, `.toUpperCase()`와 같은 메서드를 사용할 수 있다.  
- 이는 자바스크립트가 자동으로 `String` 객체로 변환해주기 때문이다.  
- 하지만 `new String()`, `new Number()` 같은 래퍼 객체를 직접 생성하는 것은 권장되지 않는다.  
    - `new String()`, `new Number()` 같은 래퍼 객체를 생성하면, 원시 값과 다르게 동작하여 혼란을 초래할 수 있다.  
    - 메모리 낭비가 발생하며, `typeof` 검사 시 일관성이 깨질 수 있다.  
    - 자바스크립트는 자동으로 원시 값을 객체처럼 다룰 수 있으므로, `new`를 사용할 필요가 없다.  
    - 따라서 문자열, 숫자, 불리언 값은 `new String()`, `new Number()` 대신 `String`, `Number` 그대로 사용하는 것이 권장된다.  
  
<br>
### **동적 타이핑 (Dynamic Typing)**
  
> 자바스크립트는 **동적 타입 언어**이므로 변수의 타입이 실행 중에 변할 수 있다.  
따라서, 유연하게 코딩이 가능하지만  
예기치 못한 타입 변환 오류 발생 가능성이 있다.  
>
  
  
```jsx
let value = 42;    // Number
value = "Hello";   // String
value = true;      // Boolean
```
  
<br>
### **타입 검사**
  
> 자바스크립트에서는 `typeof` 연산자로 변수의 데이터 타입을 확인할 수 있다.  
>
  
  
```jsx
console.log(typeof 42);        // "number"
console.log(typeof "Hello");   // "string"
console.log(typeof true);      // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null);      // "object"  (자바스크립트의 설계 오류)
console.log(typeof {});        // "object"
console.log(typeof function() {}); // "function"
```
  
`typeof null`이 `"object"`로 나오는 것은 자바스크립트의 설계 오류이다.  
  
<br><br><br>
## 동등연산자와 일치연산자
  
<br>
### 동등연산자
  
> 타입이 다르면 암시적 형변환을 수행 후 비교하는 방식이다.  
암시적으로 형변환이 이루어지기 때문에 예상치 못한 결과가 발생할 수 있다.  
>
  
  
```jsx
console.log(0 == false);   // true  (false → 0)
console.log("123" == 123); // true  ("123" → 숫자 변환)
console.log(null == undefined); // true (둘 다 값 없음)
```
  
<br>
### 일치연산자
  
> 타입 변환 없이 값과 타입이 모두 동일한지 비교하는 방식이다.  
`===`을 사용하면 예상치 못한 자동 형변환을 방지할 수 있어, 일반적으로 `===`을 사용하는 것이 권장된다.  
>
  
  
```jsx
console.log(0 === false);   // false  (타입이 다름)
console.log("123" === 123); // false  (타입이 다름)
console.log(null === undefined); // false (타입이 다름)
```
  
<br><br><br>
## Falsy 값
  
  
> 자바스크립트에서 Falsy 값 이란,   
조건문에서 `false`로 평가되는 값을 의미한다.  
즉, `if` 문에서 `false`처럼 동작하는 값들이다.  
>
  
  
> 자바스크립트에서 Falsy 값에 해당하는 값은 총 7가지이다.  
>
  
  
| 값 | 설명 |  
| --- | --- |  
| `false` | `false` 자체 |  
| `0`, `-0` | 숫자 0과 음수 0 |  
| `""`, `''`, `` | 빈 문자열 |  
| `null` | 값이 없음을 명시적으로 표현 |  
| `undefined` | 변수가 선언되었지만 값이 할당되지 않음 |  
| `NaN` | 숫자가 아님을 의미하는 값 (연산 결과가 숫자로 표현될 수 없는 경우) |  
| `document.all` | HTML DOM에서 특별한 객체로 `false`처럼 동작 (예외적인 경우) |  
  
<br>
### **`undeclared`**
  
> `undeclared`는 변수가 아예 선언되지 않은 상태를 의미한다.  
`undefined`와 다르게, 아예 변수가 존재하지 않기 때문에 `ReferenceError`가 발생한다.  
`undefined`는 변수가 선언되었지만 값이 없음을 나타내는 것이고  
`undeclared` 변수가 아예 존재하지 않는 것을 나타낸다.  
>
  
  
```jsx
console.log(notDeclaredVar); // ReferenceError: notDeclaredVar is not defined
```
  
<br>
### **`NaN`**
  
> `NaN`은 수학적으로 숫자로 변환할 수 없는 경우 발생한다.  
하지만 `typeof NaN === "number"` 이므로, 숫자 타입으로 취급된다.  
`NaN`은 자기 자신과 일치하지 않는 유일한 값이다.  
>
  
  
```jsx
console.log("abc" * 3); // NaN
console.log(0 / 0); // NaN
console.log(parseInt("xyz")); // NaN
console.log(typeof NaN); // "number"

console.log(NaN === NaN); // false 
console.log(isNaN(NaN)); // true 
```
  
<br>
### **`undefined` vs `null` 비교**
  
> `undefined`와 `null`은 서로 다른 값이지만,   
**`==` 연산자를 사용하면 같은 값으로 간주된다.**  
>
  
  
```jsx
console.log(undefined == null);  // true  (자동 형변환 발생)
console.log(undefined === null); // false (타입이 다름)
```
  
<br>
### **Falsy 값과 Truthy 값 비교**
  
> Truthy 값은 Falsy 값이 아닌 모든 값으로  
`{}, [], function(){}` 같은 빈 객체나 빈 배열도 Truthy 값으로 간주된다.  
>
  
  
| 값 | **Falsy 값 (false로 평가됨)** | **Truthy 값 (true로 평가됨)** |  
| --- | --- | --- |  
| 숫자 | `0`, `-0`, `NaN` | 나머지 모든 숫자 (`1`, `-1`, `Infinity`) |  
| 문자열 | `""` (빈 문자열) | `"Hello"`, `" "` (공백 포함) |  
| 논리값 | `false` | `true` |  
| 객체 | 없음 | 모든 객체 (`{}`, `[]`, `new Boolean(false)`) |  
| 특수값 | `null`, `undefined` | 모든 함수, 객체, 배열 |  
  
<br><br><br>
## **데이터 형변환**
  
<br>
### **암시적 형변환 (Implicit Coercion)**
  
> 앞서 봤듯, 자바스크립트는 연산자 사용 시 자동으로 타입을 변환하는 특성이 있다.  
주로 `+`, `-`, `*`, `/`, `==` 등의 연산에서 발생한다.  
>
  
  
```jsx
console.log("5" + 3);   // "53" (문자열로 변환)
console.log("5" - 3);   // 2 (숫자로 변환)
console.log(true + 1);  // 2 (true → 1)
console.log(false + "test"); // "falsetest" (false → 문자열)
```
  
<br>
### **명시적 형변환 (Explicit Conversion)**
  
> 개발자가 의도를 가지고 데이터 타입을 변환시키는 것으로  
타입을 변경하는 기본적인 방법은   
`Object()`, `Number()`, `toString()`, `Boolean()` 와 같은 함수를 이용하는데  
>
  
  
> new 연산자가 없다면 사용한 함수는 타입을 변환하는 함수로써 사용된다.  
new 연산자가 있으면 → 객체를 생성하는 생성자 함수로 동작한다.  
>
  
  
| 변환 대상 | 변환 함수 | 변환 결과 예시 |  
| --- | --- | --- |  
| **숫자로 변환** | `Number()`, `parseInt()`, `parseFloat()` | `Number("123.45")` → `123.45` |  
| **문자열로 변환** | `String()`, `toString()`, `toFixed()` | `String(123)` → `"123"` |  
| **불리언으로 변환** | `Boolean()`, `!!` | `Boolean("")` → `false`, `!!"abc"` → `true` |  
| **객체 변환 (비추천)** | `new Number()`, `new String()`, `new Boolean()` | `new Number(123)` → `Number {123}` |  
  
<br><br><br>
## 배열과 객체
  
  
> 자바스크립트에서 객체(Object)와 배열(Array) 은 데이터를 저장하는 두 가지 주요 구조이다.   
하지만 이 둘은 내부 구조, 데이터 접근 방식, 사용 목적에서 차이가 있다.  
>
  
  
<br>
### 객체(Object)
  
> 객체(Object) 는 키(key)-값(value) 쌍으로 이루어진 데이터 구조이다.  
>
  
- 키(Key)는 문자열(String) 또는 심볼(Symbol)만 가능하다.  
- 값(Value)에는 모든 데이터 타입이 가능하다.  
- 속성(Property)은 `객체명.속성` 또는 `객체명["속성"]` 형태로 접근 가능하다.  
- 순서가 중요하지 않아서 요소의 정렬이 보장되지 않는다.  
  
<br>
### 배열 (Array)
  
> 배열(Array) 은 순서(index)가 있는 데이터 리스트로  
값을 순차적으로 저장할 때 사용된다.  
>
  
- 요소(Item)는 순서를 가짐 → `0`, `1`, `2`처럼 숫자 인덱스로 접근 가능하다.  
- 값(Value)에는 모든 데이터 타입이 가능하다.  
- 배열 길이(`length`)가 자동으로 관리된다.  
- 배열 메서드(`push()`, `pop()`, `map()`, `filter()` 등) 사용이 가능하다.  
  
<br>
### **메모리 구조**
  
> 일반적인 배열은 메모리 공간이 연속적으로 배치되며, 모든 요소가 동일한 크기의 메모리 블록을 차지한다.  
하지만 자바스크립트의 배열은 이러한 구조를 따르지 않고, 내부적으로 객체처럼 동작한다.  
즉, 요소들이 연속된 메모리에 저장되지 않을 수도 있으며, 객체의 속성과 유사한 방식으로 관리된다.  
>
  
  
> 객체와 배열은 모두 `object` 타입이다.  
>
  
  
```jsx
const arr = ['a', 'b', 'c', 'd'];
console.log(arr[1]); // "b"
console.log(arr.length); // 4
console.log(typeof arr); // "object"
```
  
<br>
### 내부구조
  
> 배열과 객체는 모두 object이지만   
내부적으로 상속받는 프로토타입이 다르다.  
이를 확인하기 위해, `__proto__` 프로퍼티를 사용하면   
각 데이터 타입이 어떤 프로토타입을 상속받았는지 알 수 있다.  
>
  
  
```jsx
const arr = ["a", "b", "c"];
console.dir(arr.__proto__); // Array.prototype

const obj = { 0: "a", 1: "b", 2: "c" };
console.dir(obj.__proto__); // Object.prototype
```
  
- 배열(`arr`) 은 `Array.prototype`을 상속받아 배열 메서드(`push()`, `map()`, `filter()` 등)를 사용할 수 있다.  
- 객체(`obj`) 는 `Object.prototype`을 상속받아 배열 전용 메서드를 사용할 수 없다.  
- 배열의 `length` 속성은 자동으로 관리되지만, 객체에서는 수동으로 설정해야 한다.  
  
<br>
### **성능**
  
> 배열은 **순차적인 데이터를 다룰 때** 효율적  
객체는 **키-값 구조로 빠르게 데이터 검색 및 접근할 때** 효율적이다.  
>
  
  
| 비교 항목 | 배열 (Array) | 객체 (Object) |  
| --- | --- | --- |  
| **인덱스로 요소 접근** | 빠름 (O(1)) | 상대적으로 느림 (O(1)~O(n)) |  
| **요소 추가/삭제 (맨 끝)** | 빠름 (`push()`, `pop()`) | 느림 (`delete obj.key`) |  
| **요소 추가/삭제 (중간)** | 느림 (`splice()`) | 빠름 (`delete obj.key`) |  
| **검색 속도 (`includes()`, `find()`)** | 느림 (O(n)) | 빠름 (O(1) - 해시 테이블) |  
  
<br>
### 차이점 정리
  
| 비교 항목 | 배열 (Array) | 객체 (Object) |  
| --- | --- | --- |  
| **데이터 구조** | 순서(index)가 있는 리스트 | 키(key)-값(value) 쌍 |  
| **length 프로퍼티** | 자동 관리됨 | 없음 (수동 설정 가능) |  
| **순서 보장** | 보장됨 | 보장되지 않음 |  
| **접근 속도** | 빠름 (O(1)) | 빠름 (해시 테이블) |  
| **요소 추가/삭제** | 끝에서 추가/삭제 빠름 (`push()`, `pop()`) | 속성이 많아질수록 느려질 수 있음 |  
| **검색 (`includes()`, `find()`)** | 느림 (O(n)) | 빠름 (O(1)) |  
| **반복문** | `for`, `forEach`, `map`, `filter` | `for...in`, `Object.keys()`, `Object.values()` |  
| **사용 예시** | 리스트, 스택, 큐, 컬렉션 데이터 | JSON 데이터, 설정값 저장 |  
  
<br><br><br>
## **Set과 Map**
  
  
> 자바스크립트에서 객체와 배열만으로   
해결하기 어려운 문제를 보완하기 위해 등장한 자료구조가 `Map`과 `Set` 이다.   
기존의 객체와 배열보다 데이터 저장, 검색, 중복 방지, 키-값 저장 등에서   
성능과 유연성이 더 뛰어난 구조를 제공한다.  
>
  
  
<br>
### **Map**
  
> `Map`은 키-값 쌍으로 이루어진 데이터를 저장하는 자료구조이다.   
기존의 객체(Object)와 달리, 모든 타입의 키(key)를 저장할 수 있으며, 키의 순서를 유지한다.  
>
  
- **키-값(Key-Value) 구조**: 기존 객체는 문자열 키만 허용하지만, `Map`은 모든 데이터 타입(객체, 배열, 원시값 등) 을 키로 사용할 수 있다.  
- **키의 삽입 순서 유지**: `Map`은 데이터가 삽입된 순서를 기억하며, 반복문(`for...of`)을 사용하면 순서대로 값을 얻을 수 있다.  
- **키의 중복 허용 X**: 같은 키로 여러 번 `set()`을 호출하면 기존 값이 덮어쓰기 된다.  
  
```jsx
const myMap = new Map();
myMap.set("name", "Alice");
myMap.set(123, "Number Key");
console.log(myMap.get("name"));  // "Alice"
console.log(myMap.get(123));  // "Number Key"
```
  
**주요 메서드**  
  
| 메서드 | 설명 |  
| --- | --- |  
| `map.set(key, value)` | `key`를 이용해 `value` 저장 |  
| `map.get(key)` | `key`에 해당하는 값 반환 (없으면 `undefined`) |  
| `map.has(key)` | `key`가 존재하면 `true`, 아니면 `false` 반환 |  
| `map.delete(key)` | `key`에 해당하는 값을 삭제 |  
| `map.clear()` | 모든 요소 제거 |  
| `map.size` | `Map`의 요소 개수 반환 |  
| `map.keys()` | 모든 키를 모아둔 `Iterator` 반환 |  
| `map.values()` | 모든 값을 모아둔 `Iterator` 반환 |  
| `map.entries()` | `[key, value]` 한 쌍의 `Iterator` 반환 (for...of 루프 사용 가능) |  
  
<br>
### **Set**
  
> `Set`은 중복되지 않는 값을 저장하는 자료구조이다.   
배열과 다르게 동일한 값이 여러 번 추가되지 않으며, 값의 유일성을 보장한다.  
>
  
- **값의 중복을 허용X:** 같은 값이 여러 번 추가되지 않으며, **유일한 값만 저장**된다.  
- **값 자체가 키처럼 동작:** 기존 객체(Object)나 `Map`은 **키-값 구조**지만, `Set`은 **키 없이 값만 저장**된다.  
- **삽입된 순서대로 순회 가능:** `for...of` 반복문을 사용하면 삽입 순서대로 값을 순회할 수 있다.  
  
```jsx
const mySet = new Set();
mySet.add(1);
mySet.add(2);
mySet.add(2); 
console.log(mySet); // Set {1, 2}
```
  
**주요 메서드**  
  
| 메서드 | 설명 |  
| --- | --- |  
| `set.add(value)` | `value`를 추가 (중복 불가) |  
| `set.has(value)` | `value`가 존재하면 `true`, 아니면 `false` 반환 |  
| `set.delete(value)` | `value` 제거 |  
| `set.clear()` | 모든 요소 삭제 |  
| `set.size` | `Set`의 요소 개수 반환 |  
| `set.keys()` | `Set` 내 모든 값을 포함하는 `Iterator` 반환 |  
| `set.values()` | `set.keys()`와 동일 (Map과의 호환성을 위해 존재) |  
| `set.entries()` | `[value, value]` 형태의 `Iterator` 반환 (Map과의 호환성을 위해 존재) |  
  
<br>
### 차이점 정리
  
| 비교 항목 | `Map` | `Set` |  
| --- | --- | --- |  
| **데이터 구조** | `키-값` 쌍 저장 | `값`만 저장 |  
| **중복 허용 여부** | `key` 중복 불가, `value` 중복 가능 | 중복 값 허용 X |  
| **순서 유지** | 삽입 순서 유지 | 삽입 순서 유지 |  
| **데이터 접근 방식** | `map.get(key)` | `set.has(value)` |  
| **반복 방식** | `map.entries()`, `for...of` | `set.values()`, `for...of` |  
| **검색 속도** | 빠름 (`O(1)`) | 빠름 (`O(1)`) |  
| **사용 예시** | 객체 대신 사용 (빠른 검색 필요) | 중복 없는 값 저장 |  
  
<br><br><br>
## WeakMap과 WeakSet
  
> 가비지 컬렉션(Garbage Collection) 은 사용되지 않는 객체를 자동으로 제거하여 메모리를 효율적으로 관리한다.   
하지만 기본적인 Map과 Set은 객체가 참조를 잃어도 메모리에서 제거되지 않는 문제가 있다.   
이를 해결하기 위해 `WeakMap`과 `WeakSet`이 등장했다.  
>
  
  
**`WeakMap`과 `WeakSet` 공통점**  
  
- **객체만 저장 가능하다.** (원시값 사용 불가능)  
- **객체가 참조되지 않으면 자동으로 삭제된다.** (가비지 컬렉션 발생)  
- **반복 작업(iteration)이 불가능하다.**  
- **모든 요소를 조회할 수 있는 방법이 없다.**  
  
<br>
### WeakMap
  
> `WeakMap`은 `Map`과 비슷하지만,   
키(key)로 객체(Object)만 사용할 수 있으며, 객체가 더 이상 참조되지 않으면 자동으로 삭제된다.  
>
  
>
  
> 즉, 가비지 컬렉션이 키를 기준으로 작동하는 특별한 `Map`이다.  
>
  
  
```jsx
let weakMap = new WeakMap();
let obj = { name: "Alice" };

weakMap.set(obj, "Secret Data"); // 객체를 키로 설정

obj = null; // 참조 제거

// `obj`가 더 이상 접근되지 않으므로 `WeakMap`에서도 자동으로 삭제된다.
```
  
**주요 메서드**  
  
| 메서드 | 설명 |  
| --- | --- |  
| `weakMap.set(key, value)` | `key`(객체)에 `value`를 저장 |  
| `weakMap.get(key)` | `key`에 저장된 값 반환 (없으면 `undefined`) |  
| `weakMap.has(key)` | `key`가 존재하면 `true`, 아니면 `false` |  
| `weakMap.delete(key)` | `key`에 해당하는 값 삭제 |  
  
<br>
### WeakSet
  
> `WeakSet`은 `Set`과 비슷하지만, 객체만 저장할 수 있으며, 객체가 참조를 잃으면 자동으로 삭제된다.  
>
  
  
```jsx
let weakSet = new WeakSet();
let user = { name: "Bob" };

weakSet.add(user); // 객체 저장
console.log(weakSet.has(user)); // true

user = null; // 참조 제거
// 가비지 컬렉션 발생 → weakSet에서도 자동 삭제된다.
```
  
**주요 메서드**  
  
| 메서드 | 설명 |  
| --- | --- |  
| `weakSet.add(value)` | `value`(객체)를 추가 |  
| `weakSet.has(value)` | `value`가 존재하면 `true`, 아니면 `false` |  
| `weakSet.delete(value)` | `value` 삭제 |  
  
<br><br><br>
## **차이점 정리**
  
| 비교 항목 | `WeakMap` | `WeakSet` |  
| --- | --- | --- |  
| **저장 방식** | `키-값(Key-Value)` 쌍 저장 | `값(Value)`만 저장 |  
| **키 유형** | **객체만 사용 가능** (원시값 X) | **객체만 사용 가능** (원시값 X) |  
| **값 유형** | 모든 데이터 타입 저장 가능 | 객체만 저장 가능 |  
| **가비지 컬렉션 영향** | **키가 참조되지 않으면 자동 삭제** | **값이 참조되지 않으면 자동 삭제** |  
| **반복(iteration) 가능 여부** | 불가능 (`keys()`, `values()` 없음) | 불가능 (`values()` 없음) |  
| **사용 예시** | 캐싱, 사용자 데이터 저장 | 방문 기록, 일회성 데이터 |  
  
<br><br><br>
**reference**
  
  
- <a href='https://ko.javascript.info/map-set' target='_blank'>https://ko.javascript.info/map-set</a>
- <a href='https://pozafly.github.io/javascript/array-is-object/' target='_blank'>https://pozafly.github.io/javascript/array-is-object/</a>