---
title: '[코어자바스크립트] undefined 과 null'
excerpt_separator: '<!--more-->'
categories:
  - Core JavaScript
tags:
  -
header:
  teaser: ./assets/image/20.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 📚 What is TIL?

## undefined란? 

> 정의 되지 않는 값으로 값이 할당되지 않은 상태를 나타낼 때 사용하는데  
> 사용자가 명시적으로 지정하는 경우와 값이 존재하지 않을 때   
> 자바스크립트 엔진이 자동으로 부여하는 경우가 있다

<br>

### 사용자가 명시적으로 지정하는 경우

> 변수를 선언하고 초기값으로 undefined를 직접 할당하는 방법으로    
> 변수의 값을 undefined로 지정할 수 있다

```js
let num = undefined;
```

<br>

### 자바스크립트 엔진이 자동으로 부여하는 경우

> 사용자가 알맞은 어떤 값을 지정할 것이라고 예상되는 상황임에도  
> 실제로 그렇게 하지 않았을 때 undefined를 반환한다

💡 어떤 경우가 있을까?  
<br>
&emsp;값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때  
&emsp;객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때  
&emsp;return 문이 없거나 호출되지 않는 함수를 실행할 때
{:.notice--primary}

🔎 undefined가 없다면?  
🔎 값이 할당되지 않은 변수나 객체의 존재하지 않는 프로퍼티에 접근할 때 예기치 않은 동작이 발생할 수 있다
{:.notice--info}

<br>

#### <span class='explain'>값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때</span>
---
```js
let a
console.log(a) // undefined
```
🔎 변수나 프로퍼티의 초기값으로 사용할 수 있어서, 초기화되지 않은 변수에 대한 안정성을 높인다
{:.notice--info}

<br>

#### <span class='explain'>객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때</span>
---
```js
const person = {name: 'DongSup', age: 20};
console.log(person.address); // undefined
```
🔎 undefined 값을 반환하기 때문에, 객체의 프로퍼티가 존재하는지 여부를 확인할 때 유용하다 
{:.notice--info}

<br>

#### <span class='explain'>return 문이 없거나 호출되지 않는 함수를 실행할 때</span>
---
```js
function foo() {
  // return 문이 없는 경우
}
console.log(foo()); // undefined

function bar() {
  return;
}
console.log(bar()); // undefined
```

🔎 암묵적으로 undefined 값을 반환하기 때문에 반환값이 필요하지 않은 함수를 작성할 때 유용하다
{:.notice--info}

✔️ 이러한 예기치 않은 접근으로 발생하는 문제를 해결하기 위해 undefined 가 존재한다
{:.notice--success}

<br>

### undefined가 존재하지 않는다면?

> undefined가 존재하지 않는다는 가정하에 
> 가장 크게 작용할 수 있는 문제는 변수에 값을 할당하지 않았을 때 이다

```js
let num
console.log(num); // ?
```

🔎 변수 num이 undefined로 초기화되지 않았기 때문에 변수 num 값은 무엇인지 알 수 없다  
🔎 이것은, 코드 상에 버그가 존재하는 것이자 디버깅의 난이도가 높아지는 것을 야기시킬 수 있다
{:.notice--info}

<br>

```js
let num;
if (typeof num === "undefined") {
  console.log('변수 num이 정의되지 않았습니다.');
} else {
  console.log(num);
}
```

🔎 따라서 변수를 사용하기 전에 값이 할당되어 있는지 체크하거나,  
&emsp;명시적으로 값을 할당해주는 식의 예외처리를해야하는 번거러움이 생길 수 있다
{:.notice--info}

<br>

#### 배열과 undefined

> 값을 할당했을때와 할당하지 않았을 때 어떠한 차이점이 있을까?  
> 변수에 undefined를 할당하는 것은 하나의 값을 동작한다  
> 하지만 사용자가 아닌 자바스크립트 엔진이 undefined를 부여하는 경우 문자 그대로 값이 없음을 나타낸다

```js
let arr1 = [undefined, undefined, undefined];
console.log(arr1); // [undefined, undefined, undefined]

let arr2 = [];
arr2.length = 3;
console.log(arr2); // [empty x 3]

let arr3 = new Array(3);
console.log(arr3); // [empty x 3]
```

🔎 즉, arr.length 나 new 연산자와 Array 생성자 함수를 호출해서 배열 인스턴스를 생성하는 경우  
배열의 길이를 설정하는 것으로 배열 요소의 개수를 지정하는 것이지, 요소에 값을 할당하는 것과는 별개라는 것을 인지해야 한다
{:.notice--info}

<br>

#### 배열의 순회

> 마찬가지로 사용자가 명시적으로 할당하는 경우와 
> 비어있는 요소에 접근할 때 반환되는 경우에따라
> undefined의 의미가 다르다

```js
let arr1 = [undefined, 1];
let arr2 = [];
arr2[1] = 1;

arr1.forEach((v, i) => {console.log(v, i)});  // undefined 0 / 1 1
arr2.forEach((v, i) => {console.log(v, i)});  // 1 1

arr1.map((v, i) => { return v + i });  // [NaN, 2]
arr2.map((v, i) => { return v + i });  // [empty, 2]

arr1.filter((v) => { return !v });  // [undefined]
arr2.filter((v) => { return !v });  // []

arr1.reduce((p, c, i) => { return p + c + i }, '');  // undefined011
arr2.reduce((p, c, i) => { return p + c + i }, '');  // 11
```

✔️ map 메서드의 경우 arr1에 할당된 요소를 처리하면서, 새로운 요소를 반환한다  
&emsp;그 과정에서 undefined + i 로 인해 number형으로 형변환이 이루어져 NaN이 반환된다  
<br>
✔️ filter 메서드의 경우 "undefined" 값이 할당된 요소를 건너뛰고, 조건에 맞는 요소만 반환한다  
&emsp;논리 부정연산자로 인해서 arr1은 1이 아닌 undefined가 반환되고  
&emsp;arr2 는 !1 가 false를 반환하므로 빈배열이 반환된다  
<br>
✔️ arr2의 첫 번째 요소는 비어있는데, 비어있는 요소에 대해서 어떠한 처리도 하지 않고 건너뛰는 것을 볼 수 있다
{:.notice--success}


🔎 리터럴 방식으로 배열을 생성한경우, 배열의 요소에 고유의 키값이 실존하게 되므로 순회의 대상이 될 수 있지만,  
🔎 배열의 키값 자체가 존재하지 않는 비어있는 요소는 순회와 관련된 많은 배열 메서드들의 순회 대상에서 제외된다
{:.notice--info}

<br>

### 변수 선언 방식에 따른 차이점

> 아래는 ECMAScript 명세서의 내용이다

<span class='explain'>
let 및 const 선언은 실행 중인 실행 컨텍스트의 LexicalEnvironment로 범위가 지정된 변수를 정의합니다.   
변수는 포함된 환경 레코드가 인스턴스화될 때 생성되지만,   
변수의 LexicalBinding이 평가될 때까지는 어떤 방식으로도 액세스할 수 없습니다.   
이니셜라이저가 있는 LexicalBinding에 의해 정의된 변수는 변수가 생성될 때가 아니라   
LexicalBinding이 평가될 때 해당 이니셜라이저의 AssignmentExpression 값이 할당됩니다.   
let 선언의 LexicalBinding에 이니셜라이저가 없는 경우 LexicalBinding이 평가될 때   
변수에 정의되지 않은 값이 할당됩니다.  
<br>
var 문은 실행 중인 실행 컨텍스트의 VariableEnvironment로 범위가 지정된 변수를 선언합니다.   
변수는 포함된 환경 레코드가 인스턴스화될 때 생성되며, 생성 시 정의되지 않은 상태로 초기화됩니다.   
어떤 VariableEnvironment의 범위 내에서 공통 BindingIdentifier는   
둘 이상의 VariableDeclaration에 나타날 수 있지만 이러한 선언은 집합적으로 하나의 변수만 정의합니다.   
이니셜라이저가 있는 변수선언에 의해 정의된 변수는 변수가 생성될 때가 아니라   
변수선언이 실행될 때 그 이니셜라이저의 AssignmentExpression 값이 할당됩니다.
</span>


🔎 무슨 얘기 일까?? 요약해보자  
&emsp;let과 const로 선언된 변수는 현재 실행되는 코드 블록에서만   
&emsp;유효한 범위를 가지며, 이를 LexicalEnvironment라고 한다  
&emsp;이 변수들은 선언된 시점 이후에 사용할 수 있으며, 초기값이 없다면 undefined로 초기화한다  
&emsp;반면 var로 선언된 변수는 함수 또는 전역 범위에서   
&emsp;유효한 범위를 가지며, 이를 VariableEnvironment라고 한다  
&emsp;var로 선언된 변수는 코드의 맨 처음으로 이동되어 호이스팅되며,  
&emsp;이때 초기값이 없다면 undefined로 초기화한다  
<br> 
🔎 호이스팅은 변수를 선언하기 전에도 변수를 사용할 수 있다는 것으로  
&emsp;즉, 변수를 선언하기 전에 해당 변수를 참조할 수 있다는 것을 의미한다
{:.notice--info}

<br>

```js
console.log(a); // ReferenceError
let a = 10;
```

🔎 let과 const 변수는 EnvironmentRecord가 인스턴스화될 때 생성되어 메모리에 할당된다  
🔎 하지만 이 변수들은 초기화되지 않은 상태로 생성되기 때문에, 변수를 선언하기 전에는 접근할 수 없다  
🔎 이러한 상황에서 해당 변수를 참조하려고 하면, ReferenceError가 발생한다
{:.notice--info}

<br>

```js
console.log(a); // undefined
var a = 10;
```
🔎 var로 선언된 변수는 EnvironmentRecord가 인스턴스화될 때 생성되면서 undefined로 초기화된다  
🔎 즉, 변수가 선언되기 전에도 접근할 수 있고 이때는 undefined를 반환한다
{:.notice--info}

<br>
<br>

## null

> 값이 없음을 나타내는 특별한 값으로  
> 비어있음을 명시적으로 나타내고 싶을 때는 undefined가 아닌 null을 쓰면 된다

### null의 이점

```js
let person = {
  name: 'dongsup',
  age: undefined,
};
console.log(person.name); // 'dongsup'
console.log(person.age); // undefined

if (person.age === undefined){
  person.age = null;
}
console.log(person.name); // 'dongsup'
console.log(person.age); // null
```

🔎 person 객체 내의 age라는 키의 값을 undefined로 지정해놓은것을 확인할 수 있다  
🔎 하지만 만약, 사용자가 객체 내부에 대한 인지를 하지 못했다면  
&emsp;undefined를 자바스크립트 엔진이 부여한것인지 사용자가 직접 지정한 것인지 알 수 없다  
🔎 따라서, null로 값이 없음을 명시적으로 나타낸다면 더 높은 가독성, 더 명확한 타입, 더불어 에러 가능성을 낮출 수 있다
{:.notice--info}

⚠️ null은 왜 object 라고 뜨는가?
<br>
&emsp;자바스크립트 초기 버전의 버그이다  
&emsp;JavaScript 설계자가 설계의 오류를 인정하는 부분에서 알 수 있듯, 원시타입으로 보는 것이 맞다
{:.notice--warning}

<br>

### null 의 큰 단점

```js
let person = {
  name: 'dongsup'
};
let myObj = null;
console.log(typeof person); // object
console.log(typeof myObj); // object
console.log(Object.keys(person)); // ['name']
console.log(Object.keys(myObj)); // TypeError: Cannot convert undefined or null to object
```

🔎 null은 객체타입으로 인식되는 버그가 있어서 null을 객체로 잘못 오인하고,  
&emsp;객체에 대한 메서드나 속성을 사용해서 예기치 않은 동작이 발생할 수 있다
{:.notice--info}

<br>

```js
let num = null;

if(num === null || num === undefined) {
  console.log('num is null or undefined');
} else if (num === 0) {
  console.log('num is zero');
} else {
  console.log('num is not null, undefined, or zero');
}
```

🔎 null의 사용은 falsy한 값이 하나 더 추가되는 것이다  
🔎 예를 들어, number 타입의 경우 falsy 한 값을 확인하기 위해  
&emsp;숫자 0과 null, undefined를 모두 명시적으로 확인해야한다
{:.notice--info}


💡 따라서, null대신 undefined만을 사용하는 것이 여러방면에서 좋겠지만 오랜 기간 마치 표준인 것처럼 사용해왔고  
&emsp;이것을 변경했을 때, 객체가 존재하지 않는 상황을 처리하지 못하는 등  
&emsp;기존 코드에 끼치는 영향이 크기 때문에 회사에서 정한 컨벤션에 맞게, null 사용에 대한 고려를 할 필요가 있다
{:.notice--primary}

<br>

## undefined ==(=) null

```js
console.log(null == undefined);
console.log(null == 0);
console.log(undefined == 0);
console.log(NaN == NaN);
// true  
// false   
// false   
// false
```

🔎 동등 연산자를 사용해 null과 undefined를 비교하면  
&emsp;두 값 모두 값이 없다는 것을 나타내는 특별한 규칙이 적용돼 true가 반환된다  
🔎 하지만 다른 값들과는 false가 반환된다  
🔎 undefined나 null일 때 형 변환을 하지 않는다   
🔎 **undefined**가 **NaN**으로 변환되는데, **NaN** 이 형변환이 이루어졌을 때는 **false**를 반환하겠지만   
&emsp;**undefined** 는 **NaN** 을 반환하기 때문에 비교가 불가능하다
<br>
🔎 NaN은 자기 자신과도 비교할 수 없다 따라서 NaN은 모든 숫자와 다른 값으로 간주된다
{:.notice--info}

⚠️ 일치 연산자 ===를 제외한 비교 연산자의 피연산자에 undefined나 null이 오지 않도록 특별히 주의하자
{:.notice--warning}