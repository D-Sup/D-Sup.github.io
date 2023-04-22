---
title: '[코어자바스크립트] Execution Context'
excerpt_separator: '<!--more-->'
categories:
  - Core JavaScript
tags:
  - Stack
  - Queue
header:
  teaser: ./assets/image/24.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>

# 📚 What is TIL?

## 실행 컨텍스트란?
_Execution Context_

> 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로,  
> 자바스크립트 코드가 실행되는 환경을 말한다  
> 즉, 모든 자바스크립트 코드는 실행 컨텍스트 내부에서 실행된다  
> 그렇다면 실행 컨텍스크는 언제 생성될까?

🔎 실행 컨텍스트가 생성되는 3가지 경우  
<br>
&emsp;1) **전역 컨텍스트(Global Execution Context)** - 스크립트 파일이 실행될 때 생성  
&emsp;2) **함수 컨텍스트(Function Execution Context)** - 함수가 호출될 때 생성  
&emsp;3) **Eval() 컨텍스트(Eval Execution Context)** - eval() 내장함수가 실행될 때 생성  
<br>
&emsp;위 3가지 경우는 자바스크립트 코드가 실행될 때 발생하는 상황(실행환경)을 의미하는 용어를 말한다
{:.notice--info}

<br>
<br>

## 변수 객체

<img class='img' src='https://user-images.githubusercontent.com/96939334/233699650-4a7d14be-f086-414b-95b9-12febc5a2ce2.png' alt=''>

> 실행 컨텍스트가 **생성될 때**는,  
> 자바스크립트 엔진이 해당 컨텍스트에 필요한 정보들  
> (객체에서 사용할 매개변수, 사용자가 정의한 변수, 객체)을 저장할 **변수 객체**를 만든다.  
>
> 즉, 변수 객체는 코드들이 실행될 때 필요한 각종 정보들을 담아 두는 곳으로  
> 예를 들어 아래코드와 같은경우 함수 실행 컨텍스트 내의 변수 객체는 이렇게 생성될 것이다

```js
function foo(param1, param2){
	var a;
  var b;
  function bar(){
    console.log(this,a, b)
  }
  bar()
}
foo();
```

🔎 foo() 실행 컨텍스트의 변수 객체에는 **arguments, 스코프, 변수, 내부 함수, this 바인딩** 과 같은 정보들이 들어가게 된다  
🔎 함수가 호출되면 해당 함수의 **변수 객체가 생성되고** 하나의 묶음으로 만들어져 **Call Stack**에 차곡차곡 쌓는다  
&emsp;여기서 하나의 묶음이 실행 컨텍스트들인 것이다
{:.notice--info}

<br>
<br>

## 스택과 큐

💡 그렇다면 스택(Stack)은 무엇일까?  
<br>
&emsp;스택은 자료구조의 일부이다  
&emsp;자바스크립트에서 스택(Stack)과 큐(Queue)는 가장 기본적인 자료구조 중 두 가지로  
&emsp;스택(Stack)은 **후입선출(LIFO, Last-In-First-Out)** 원리에 따라 데이터를 저장하고 꺼내는 자료구조이며,   
&emsp;큐(Queue)는 **선입선출(FIFO, First-In-First-Out)** 원리에 따라 데이터를 저장하고 꺼내는 자료구조이다  
&emsp;스택과 큐는 데이터를 저장하고 처리하는 데 유용하고, 알고리즘 및 데이터 구조를 구현하는 데 사용된다
{:.notice--primary}

<details>
  <summary>스택과 큐의 기본동작</summary><br>
  <h4>스택</h4>
  <br>
   <code>push(item)</code><br>
  &nbsp;스택의 맨 위에 item을 추가한다<br>
  <br>
   <code>pop()</code><br>
  &nbsp;스택에서 맨 위의 항목을 삭제하고 반환한다<br>
  <br>
   <code>peek()</code><br>
  &nbsp;스택 맨 위의 항목을 반환, 삭제하지 않는다<br>
  <br>
   <code>is_empty()</code><br>
  &nbsp;스택이 빈 여부를 반환<br>
  <br>
   <code>size()</code><br>
  &nbsp;스택에 저장된 항목 개수를 반환 <br>
  <br>
  <h4>큐</h4>
  <br>
   <code>enqueue()</code><br>
  &nbsp; 큐에 item을 추가한다<br>
  <br>
   <code>dequeue()</code><br>
  &nbsp;큐 맨 앞의 item을 삭제하고 반환한다<br>
  <br>
   <code>front()</code><br>
  &nbsp;큐의 가장 앞에 있는 항목을 반환한다<br>
  <br>
   <code>back()</code><br>
  &nbsp;큐의 가장 뒤에 있는 항목을 반환한다<br>
  <br>
   <code>isEmpty()</code><br>
  &nbsp;큐가 비어있는지 아닌지 여부를 반환한다<br>
  <br>
   <code>size()</code><br>
  &nbsp;큐에 저장되어 있는 자료의 개수를 반환한다<br>
</details>

<br>
<br>

### 스택과 큐의 추상적인 개념을 자바스크립트로 구현해보기

#### 스택(Stack)
---
```js
class Stack {
  constructor() { // ------ [1]
    this.items = [];
  }
  
  push(item) { // ------ [2]
    this.items.push(item);
  }

  pop() { // ------ [3]
    if (!this.is_empty()) {
      return this.items.pop();
    }
  }

  peek() { // ------ [4]
    if (!this.is_empty()) {
      return this.items[this.items.length - 1];
    }
  }

  is_empty() { // ------ [5]
    return this.items.length === 0;
  }

  size() { // ------ [6]
    return this.items.length;
  }
}
```

<br>
&emsp;**[1]** stack 빈 배열로 초기화  
<br>
&emsp;**[2]** `push(item)` 스택의 맨 위에 item 추가   
<br>
&emsp;**[3]** `pop()` 스택의 맨 위 항목을 삭제하고 반환, 비어있다면 undefined 반환  
<br>
&emsp;**[4]** `peek()` 스택 맨 위의 항목을 반환 삭제하지 않고, 배열의 인덱싱을 이용 맨 위를 참조  
<br>
&emsp;**[5]** `is_empty()` 스택의 빈 여부를 반환, 배열 길이를 확인 하여 true와 false 반환  
<br>
&emsp;**[6]** `size()` 스택에 저장된 항목의 개수를 반환, 배열의 길이를 반환  
<br>
{:.notice--success}

```js
let stack = new Stack(); // ------ [1]

stack.push(1); // ------ [2]
stack.push(2); //
stack.push(3); //

console.log(stack.peek()); // ------ [3]
console.log(stack.pop()); // ------ [4]
console.log(stack.peek());

console.log(stack.is_empty()); // ------ [5]
console.log(stack.size()); // ------ [6]
```

<br>
&emsp;**[1]** stack 객체 생성  
<br> 
&emsp;**[2]** `push(1,2,3)` 순서대로 stack에 추가  
<br>
&emsp;**[3]** `peek()` stack 맨 위 항목을 확인 ➡️ **3**  
<br>
&emsp;**[4]** `pop()` stack맨 위 항목 제거 및 반환 ➡️ **3**  
<br>
&emsp;**[5]** `is_empty()` stack의 빈 여부 확인 ➡️ **false**  
<br>
&emsp;**[6]** `size()` 빈 여부 확인 및 항목 갯수 확인 ➡️ **2**  
<br>
{:.notice--success}

<br>

#### 큐(Queue)
---
```js
class Queue { 
  constructor() { // ------ [1]
    this.queue = [];
  }

  enqueue(item) { // ------ [2]
    this.queue.push(item);
  }

  dequeue() { // ------ [3]
    if (this.isEmpty()) {
      return "Queue is empty";
    } else {
      return this.queue.shift();
    }
  }

  front() { // ------ [4]
    if (this.isEmpty()) {
      return "Queue is empty";
    } else {
      return this.queue[0];
    }
  }

  back() { // ------ [5]
    if (this.isEmpty()) {
      return "Queue is empty";
    } else {
      return this.queue[this.queue.length - 1];
    }
  }

  isEmpty() { // ------ [6]
    return this.queue.length === 0;
  }

  size() { // ------ [7]
    return this.queue.length;
  }
}
```

<br>
&emsp;**[1]** queue  빈 배열로 초기화  
<br>
&emsp;**[2]** `enqueue(item)` 새로운 item을 큐의 뒤쪽에 추가, push 메서드 사용  
<br>
&emsp;**[3]** `dequeue()` 큐의 첫 번째 item을 제거, shift 메서드 사용, 만약 비어있다면 “Queue is empty” 반환  
<br>
&emsp;**[4]** `front()` 큐의 첫 번째 item을 반환, 만약 비어있다면 “Queue is empty” 반환  
<br>
&emsp;**[5]** `back()` 규의 마지막 item을 반환, 만약 비어있다면 “Queue is empty” 반환  
<br>
&emsp;**[6]** `isEmpty()` 큐의 빈 여부를 반환 배열 길이를 확인 하여 true와 false 반환   
<br>
&emsp;**[7]** `size()` 큐에 저장된 항목의 개수를 반환, 배열의 길이를 반환  
<br>
{:.notice--success}

```js
const queue = new Queue(); // ------ [1]

queue.enqueue("작업 1"); // ------ [2]
queue.enqueue("작업 2"); //
queue.enqueue("작업 3"); //

console.log("front of queue:", queue.front()); // ------ [3]

console.log("back of queue:", queue.back()); // ------ [4]

console.log("queue size:", queue.size()); // ------ [5]

while (!queue.isEmpty()) { // ------ [6]
  const job = queue.dequeue();
  console.log(`${job} 처리 완료`);
}

console.log("queue size:", queue.size()); // ------ [7]
```

<br>
&emsp;**[1]** queue 객체 생성  
<br>
&emsp;**[2]** `enqueue(작업 1, 작업 2, 작업 3)` 순서대로 queue에 추가  
<br>
&emsp;**[3]** `front()` queue의 첫 번째 item 반환 ➡️ **작업 1**  
<br>
&emsp;**[4]** `back()` queue의 마지막 item 반환 ➡️ **작업 3**  
<br>
&emsp;**[5]** `size()` queue의 길이 반환 ➡️ **3**  
<br>
&emsp;**[6]** `dequeue()` queue가 모든 item을 비어있을 때까지 루프를 반복  
<br>
&emsp;**[7]** `size()` queue의 길이 반환 ➡️ **0**  
<br>
{:.notice--success}

<br>
<br>

## 실행컨텍스트의 구조

> 함수가 호출되면 실행 컨텍스트가 생성될 때 함수 내부의 코드를 분석하고  
> 해당 코드에서 변수, 함수 등을 생성하는 과정 즉, **함수 코드 평가**가 이루어진다.  
> 전역 공간에 있던 코드의 **제어권**이 함수가 호출되면서 함수 내부로 이동하게 되는데   
> 때문에 함수 내부에서 실행되는 코드는 함수의 지역 변수, 매개 변수 등에 접근할 수 있으며,  
> 함수 외부에서 정의된 변수나 함수에도 접근할 수 있다  
> 함수 호출이 끝나면, 실행 컨텍스트가 제거되고 제어권이 이전 위치로 돌아간다

🔎 함수 코드 평가는 아래와 같이 이루어진다  
<br>
**함수 LexicalEnvironment 생성**  
실행 컨텍스트 안에서 함수의 LexicalEnvironment가 생성된다  
LexicalEnvironment는 함수의 변수와 함수 스코프를 담고 있는 객체이다  
함수 실행 도중 변수에 접근하거나 함수를 호출할 때 사용된다  
<br>
**함수 environmentRecord 생성**  
함수 안의 코드가 실행되기 전에 현재 컨텍스트와 관련된 코드의 식별자 정보가 저장된다  
(매개변수의 이름, 함수 선언, 변수명 등)  
즉, 코드가 실행되기 전에 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수명 등을 모두 알고 있게 된다 (호이스팅)  
<br>
**variableEnvironment 생성**  
현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보를 담는다  
선언 시점의 LexicalEnvironment의 스냅샵으로 변경 사항은 반영되지 않는다  
실행 컨텍스트를 생성할 때 variableEnvironment에 정보를 먼저 담은 다음,  
이를 복사해서 LexicalEnvironment를 만든다  
<br>
**ThisBinding 결정**  
식별자가 바라봐야 할 대상으로 함수가 실행될 때 어떤 객체를 가리키는지 결정한다  
실행 컨텍스트가 활성될 때 this가 지정되지 않은 경우 this에는 전역 객체가 저장된다  
함수 호출 방법에 따라 ThisBinding이 달라진다  
<br>
**outerEnvironmentReference 결정**  
outerEnvironmentReference는 현재 함수가 속한 스코프를 가리키는 참조로 상위 스코프를 가리킨다  
즉, 현재 environment record보다 바깥에 있는 environment record를 참고한다는 뜻이며   
해당 실행 컨텍스트를 생성한 함수의 바깥 환경을 가리킨다  
(outerEnvironmentReference는 외부 함수의 LexicalEnvironment를 가리킨다)  
따라서 자바스크립트 엔진이 현재 렉시컬 환경에서 변수를 찾을 수 없다면 외부 환경에서 찾는다는 것을 뜻한다  
만약 상위 스코프에서도 해당 식별자를 찾을 수 없다면 참조 에러(uncaught reference error)를 발생시킨다  
outerEnvironmentReference는 스코프 체인을 구성하는 데 사용된다  
{:.notice--info}

<br>

### 실행컨텍스트의 동작과정
---

<img class='img' src='https://user-images.githubusercontent.com/96939334/233785538-9ddf0ec6-7b51-48e7-9b5b-0bc4a72aa1a0.png' alt=''>

```js
var a = 1
function outer() {
  // --------------------------- [2]
  function inner() {
    // ------------------------- [4]
    console.log(a) // ---------- [5]
    var a = 3 // --------------- [6]
  }
  inner() // ------------------- [3]
  console.log(a) // ------------ [7]
}
outer() // -------------------- [1]
console.log(a) // --------------[8]
```

✔️ 함수가 호출되고 컨텍스트가 생성되고 제거되는 시점은 이렇다  
<br>
**1)** (1) 자바스크립트 코드를 실행하는 순간 전역 컨텍스트가 콜 스택에 담긴다.  
<br>
**2)** (3) outer 함수를 호출하면 자바스크립트 엔진은 outer에 대한 환경 정보를 수집해서 outer 실행 컨텍스트를 생성한 후 콜 스택에 담는다. (콜 스택 맨 위에 outer 실행 컨텍스트가 놓인 상태가 됐으므로, 전역 컨텍스트와 관련된 코드의 실행을 일시 중단하고 대신 outer 실행 컨텍스트와 관련된 코드, 즉 outer 함수 내부의 코드들을 순차적으로 실행한다.)  
<br>
**3)** (2) inner 함수의 실행 컨텍스트가 콜 스택의 가장 위에 담기면 outer 컨텍스트와 관련된 코드의 실행을 중단하고 inner 함수 내부의 코드를 순서대로 진행한다.  
<br>
**4)** inner 함수 실행 종료되면 콜 스택에서 제거됨  
<br>
**5)** 아래에 있던 outer 컨텍스트가 콜 스택 맨 위에 존재하게 되므로, (2)의 다음줄부터 이어서 실행  
<br>
**6)** a 변수 출력하면 outer 실행 컨텍스트가 콜 스택에서 제거되고, 전역 컨텍스트만 남게됨  
<br>
**7)** 실행을 중단했던 (3) 다음줄 이어서 실행  
<br>
**8)** a 변수 값 출력하면 전역 공간에 실행할 코드가 없어 전역 컨텍스트도 제거. 콜 스택 비어진 채로 종료 
<br>
{:.notice--success}

```js
// 전역 실행 컨텍스트 생성 [1]
// LexicalEnvironment 생성 [1-1]
// environmentRecord 생성 [1-1]
// outerEnvironmentReference 결정 (null) [1-2]
// ThisBinding 결정 [1-3]
// VariableEnvironment 결정 [1-3]
var a = 1;
function outer() {
  // outer 실행 컨텍스트 생성 [2]
  // LexicalEnvironment 생성 [2-1]
  // environmentRecord 생성 [2-1]
  // outerEnvironmentReference 결정 (전역 렉시컬 환경) [2-2] 
  // ThisBinding 결정 (전역 객체) [2-3]
  // VariableEnvironment 결정 [2-4]
  function inner() {
    // inner 실행 컨텍스트 생성 [3]
    // LexicalEnvironment 생성 [3-1]
    // environmentRecord 생성 [3-1]
    // outerEnvironmentReference 결정 (outer 함수 렉시컬 환경) [3-2]
    // ThisBinding 결정 (전역 객체) [3-3]
    // VariableEnvironment 결정 [3-4]
    console.log(a); // undefined [3-5]
    var a = 3;
  }
  inner(); // inner 함수 호출 [2-5]
  console.log(a); // 1 [2-6]
}
outer(); // outer 함수 호출 [1-4]
console.log(a); // 1 [1-5]
```

💡 더 세부적으로보면?  
<br>
**1)** 전역 실행 컨텍스트에서 변수 a가 선언되고 1이라는 값이 할당된다  
&emsp;이때, 전역 실행 컨텍스트의 LexicalEnvironment에는 environmentRecord 와  
&emsp;outerEnvironmentReference가 생성되며, ThisBinding과 VariableEnvironment는 전역 객체를 참조한다  
<br>
**2)** outer 함수가 호출되면, outer 실행 컨텍스트가 생성된다 
&emsp;outer 함수 실행 컨텍스트의 LexicalEnvironment에는  
&emsp;environmentRecord와 outerEnvironmentReference가 생성된다  
&emsp;이때, outerEnvironmentReference는 전역 실행 컨텍스트의 LexicalEnvironment를 참조한다  
&emsp;또한, ThisBinding과 VariableEnvironment는 전역 객체를 참조한다  
<br>
**3)** inner 함수가 호출되면, inner 실행 컨텍스트가 생성된다  
&emsp;inner 함수 실행 컨텍스트의 LexicalEnvironment에는 environmentRecord와 outerEnvironmentReference가 생성된다  
&emsp;이때, outerEnvironmentReference는 outer 함수 실행 컨텍스트의 LexicalEnvironment를 참조한다  
&emsp;또한, ThisBinding과 VariableEnvironment는 전역 객체를 참조한다  
<br>
**4)** console.log(a) 구문이 실행되면, inner 함수 내부에서 선언된 변수 a가 아직 초기화되지 않았으므로 undefined가 출력된다  
<br>
**5)** var a = 3 구문이 실행되면, inner 함수 실행 컨텍스트의 environmentRecord에 변수 a가 추가되고 3이라는 값이 할당된다  
<br>
**6)** inner 함수 실행 컨텍스트가 종료되면, inner 실행 컨텍스트는 스택에서 제거된다  
<br>
**7)** console.log(a) 구문이 실행되면, outer 함수 내부에서 선언된 변수 a의 값인 1이 출력된다  
<br>
**8)** outer 함수 실행 컨텍스트가 종료되면, outer 실행 컨텍스트는 스택에서 제거된다  
<br>
**9)** console.log(a) 구문이 실행되면, 전역 실행 컨텍스트에서 선언된 변수 a의 값인 1이 출력된다  
{:.notice--primary}


<br>
<br>

## 자바스크립트의 특징

> 자바스크립트는 **싱글 스레드 언어**이고 하나의 콜스택 **단일 호출스택**으로 코드를 실행한다

<br>

### 싱글 스레드란?

> 우선 프로세스와 스레드의 개념을 알고 가야 한다  
> 프로세스는 **운영 체제로부터 할당받은 작업의 단위**이다  
> 프로세스 내부에는 Code, Data, Stack, Heap이라는  네 가지 리소스가 존재한다  
> 프로그램을 실행했을 때 보조 기억장치(하드 디스크)에서 프로그램의 코드를 컴파일하여   
> 주 기억장치인 메모리에 옮겨가면, 그것을 프로세스라고 한다  

💡 스레드는 프로세스가 할당받은 자원을 사용하는 **작업의 흐름 단위**이다  
💡 스레드는 Stack을 가지며 나머지 Code, Data, Heap 리소스를 사용하며 작업을 처리한다  
💡 이때 프로세스 내부에 스레드가 하나라면 싱글 스레드, 여러 개라면 멀티 스레드이다  
💡 따라서 자바스크립트는 싱글 스레드 언어이기때문에 하나의 단일 호출스택으로 밖에 코드를 실행하지 못하는것이다
{:.notice--primary}

<br>

### 자바스크립트는 왜 싱글 스레드일까?

> 자바스크립트가 싱글 스레드를 선택한 이유는 자바스크립트가 탄생한 역사를 보면 알 수 있다  
> 웹 사이트를 구현하던 개발자들에게 자바라는 언어는 다소 무겁고 어려운 언어였다  
> 자바는 멀티 스레드 모델을 지원하는 언어이다  
> 멀티 스레드로 구현되는 서비스에서는 동시성 문제를 불가피하게 신경 써야만 한다  
> 이러한 역사 그리고 동시성으로 인한 복잡도 증가와 같은 이유들로 인해 자바스크립트는 싱글 스레드로 탄생한 것이다

🔎 자바스크립트는 웹 페이지의 보조적인 기능을 수행하기 위해  
&emsp;브라우저에서 동작하는 경량 프로그래밍 언어를 도입하기로 결정하고 만들어진 언어이다  
<br>
🔎 따라서 자바스크립트는 싱글 스레드 언어이기때문에 하나의 단일 호출스택으로 밖에 코드를 실행하지 못하는것이다
{:.notice--info}

<br>

### 그렇다면 단일 호출 스택의 장단점은?

> 단일 호출 스택 자체는 **동기적**이다 함수 실행 중에는 다음 함수가 실행될 때까지  
> 해당 함수의 실행이 끝나기를 기다리며, 다른 함수가 호출될 수 없다  

🔎 이렇게 함수 실행 순서가 예측 가능하고 일관적이다  
🔎 하지만 한 번에 하나의 작업만 처리할 수 있기 때문에 여러 작업을 동시에 처리하는 멀티태스킹에 취약하다  
🔎 긴 작업이 있는 경우 다른 작업이 대기해야 하는 문제 즉 **블로킹(blocking)**이 발생할 수 있다   
🔎 무한 루프나 재귀 호출 같은 무한 반복 코드가 있을 경우, 호출 스택이 계속해서 쌓이다가 스택 오버플로우(메모리 초과)가 발생할 수 있다
{:.notice--info}


✔️ 이러한 단점을 극복하고자 **Web api, Eventloop, Taskque**와 같은 자바스크립트가 아닌  
&emsp;브라우저에 내장되어 있는 기능들을 사용함으로써, **하나의 콜 스택으로도 여러 개의 작업**을 처리할 수 있다
{:.notice--success}

<br>

<span class='explain'>Reference</span> 

- [lifeandit.tistory.com](https://lifeandit.tistory.com/145)
- [hayeondev.gatsbyjs.io](https://hayeondev.gatsbyjs.io/220219-core-javascript-2/)
- [joontae-kim.github.io](https://joontae-kim.github.io/2020/10/12/excution-context/)