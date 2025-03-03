---
title: '가비지 컬렉션'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/55.png
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
> 변수 선언을 통해  
> 변수에 대한 메모리 공간을 확보해서 변수의 이름과 연결하고  
> 값의 할당을 통해  
> 값에 대한 메모리 공간이 확보한 다음,  
> 값이 원시 타입일 경우 값 자체를 메모리 공간에 저장하고  
> 값이 참조 타입일 경우 값이 저장된 메모리 주소가 메모리 공간에 저장한다.  
> 그리고 해당 메모리 공간은 변수의 이름을 통해 참조되게 된다.

<br><br><br>

## 메모리 누수 (memory-leak)

> 변수를 통해서 참조하고 있는 데이터가 필요없어졌다고 가정하고  
> 재할당을 통해서 새로운 메모리 공간에대한 참조를 하게된다면  
> 재할당 전에 참조하고 있던 메모리 공간은 사라지지 않고 메모리상에 남게되는데  
> 이렇게되면 더 이상 필요하지 않은 메모리가  
> 해제되지 않고 계속 남아있는 `메모리 누수 현상`이 일어난다.  
> 이 메모리 누수 현상을 해결하기 위해 어떻게 해야할까?

<br><br><br>

## 메모리 누수 해결법

> 메모리 누수 현상을 방지하기 위해선  
> 할당된 메모리는 역할을 다하면 해제하면 된다.  
> 어떤 프로그래밍 언어냐에 따라 해결법이 다른데,  
> 우선 C 언어와 같은 `Unmanaged Language`는  
> 메모리 할당과 해제를 개발자가 직접해야하기 때문에  
> 자유도가 높지만 메모리 관리에 복잡도가 증가하게된다.  
> 자바스크립트와 같은 `Managed Language`는  
> `V8엔진`(구글에서 개발한 오픈 소스 자바스크립트 엔진)이 알아서 관리해주기 때문에  
> 개발자가 따로 관리하지 않아도 된다.

<br><br><br>

## 가비지 컬렉터 (Garbage Collector)

> V8 엔진은 `가비지 컬렉터`라는 메모리 관리 시스템을 내장하고 있는데  
> 이 가비지 컬렉터는  
> 메모리를 할당하고 쓸모가 없어졌을때 자동으로 해제하는 가비지 컬렉션 기능을 가지고있다.

<br>
### 메모리의 힙(Heap)과 스택(Stack)
  
  
> 가비지 컬렉션에 대해 이야기하기 전에 메모리의 `힙`과 `스택`에 대해 알아볼 필요성이 있다.  
>
  
  
**스택**  
  
> 스택은 함수호출 시 지역 변수와 매개 변수가 저장되는 임시 메모리 영역으로  
자바스크립트 엔진이 컴파일 시점에 크기를 알 수 있는 정적 데이터인  
원시 값(string, number, boolean 등)과 참조(객체, 함수를 가리키는 메모리 주소)가 저장된다.  
스택은 LIFO(Last-In-First-Out, 후입선출) 방식으로 관리되어  
함수 호출이 끝나면 OS에 의해 메모리가 자동으로 해제된다.  
>
  
  
**힙**  
  
> 힙은 동적으로 할당된 메모리 영역으로  
힙은 메모리의 크기를 사전에 알 수 없는 동적 데이터가 저장되는데  
객체와 배열과 같은 크기가 가변적인 데이터가 저장된다.  
힙은 런타임 시에 메모리가 할당되고 해제되는데  
스택과 달리, OS에 의해 메모리가 해제되는 것이 아니라  
가비지 컬렉션에 의해 메모리를 해제하게 된다.  
>
  
  
<br><br><br>
## 가비지 컬렉션 (Garbage Collection)
  
  
> 힙 영역은 세부적으로  
New space, Old space, Large Object space, 코드 space, 셀 space, 속성 space, 맵 space로 이루어져있는데  
New space와 Old space에서 가비지 컬렉션이 일어난다.  
New space와 Old space각 영역에 따라 진행되는 가비지 컬렉션이 다른데  
New space에서 진행되는 GC는 `마이너 GC`이고  
Old space에서 진행되는 GC는 `메이저 GC`이다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/25e933ed-813e-4d48-b81d-bbb47d6d3fb5' alt=''>
  
<br>
### Minor GC
  
  
> 마이너 GC는 `Copy & Scavenge` 알고리즘을 사용해서 가비지컬렉션을 진행한다.  
New space는 새로 만들어진 객체가 저장되는 곳이고  
New space는 2개의 semi space로 나뉜다.  
객체는 처음에 New space의 첫 번째 semi space에 할당되고  
만약 마이너 GC로부터 한 번 생존한다면, 다른 semi space로 이동하게 된다.  
그리고 생존한 객체들이 또 한 번 마이너 GC로부터 생존하면, Old space로 이동하게 된다.  
>
  
  
> 여기서 2개의 semi space는  
각각 비어있는 영역을 나타내는 To space와 객체들이 머무르는 영역인 From space가 되는데  
상황에 따라서 이 역할이 바뀌게된다.  
Minor GC가 진행되는 순서를 살펴보자  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/b8bfc096-ad2f-4065-8e3e-32e93c108987' alt=''>
  
1. 새로운 객체가 생성되면 먼저 From space에 저장이 된다.  
2. 마이너 GC로 부터 한 번 생존한다면, From space에 있는 객체들을 To space로 이동시킨다.   
그리고 From space에 남아있는 객체들은 버린다.  
3. From space와 To space의 역할을 서로 바꾼다.  
4. From space에 기존에 한 번 생존했던 객체들이 또 한 번 생존한다면, Old space로 이동하게 된다. 이 과정에서 새로운 생성된 객체가 있다면 마찬가지로 생존한다면 To space로 이동하고 남은 객체들은 버린다.  
  
<br>
### Major GC
  
  
> Major GC는  
`Mark-Sweep-Compact` 알고리즘과  
`Tri-color` 알고리즘을 사용해서 가비지 컬렉션을 진행한다.  
>
  
  
> 자세히 말하면,  
Mark-Sweep-Compact 알고리즘은  
마킹, 스위핑, 압축 세 단계를 통해 이루어지는데  
이 중에서 마킹 단계를 Tri-color 알고리즘을 사용해서 최적화해서  
마킹, 스위핑, 압축을 통해 가비지 컬렉션을 진행하는 것이다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/5bc97f7d-8693-44cc-a53b-9182dce07469' alt=''>
  
1. **마킹 (Marking)**: 가비지 컬렉터는 어떤 객체가 사용 중인지, 도달 가능한 객체인지 식별하는 단계이다.  
a. 가비지 컬렉터는 루트(root) 객체(전역 객체, 실행 스택의 변수 등)에서 시작해서 객체 그래프를 순회한다.  
b. 도달 가능한 객체는 "마킹"되어 사용 중임을 표시한다.  
c. 도달 불가능한 객체는 마킹되지 않는다.  
2. **스위핑 (Sweeping)**: 마킹 단계를 통해 식별된 도달 불가능한 객체들을 메모리에서 제거하는 단계이다.  
a. 가비지 컬렉터는 힙 메모리를 순회하면서 마킹되지 않은 객체들을 찾아 메모리에서 해제한다.  
b. 이 과정에서 해제된 메모리 공간은 빈 공간으로 표시된다.  
3. **압축 (Compaction)**: 스위핑 단계에서 생성된 빈 공간들을 정리해서 메모리 단편화를 해소한다.  
      
    a. 가비지 컬렉터는 살아있는 객체들을 메모리의 앞쪽으로 이동시켜 연속된 메모리 공간을 확보한다.  
    b. 이 과정에서 이동된 객체들의 참조는 새로운 메모리 주소를 가리키도록 업데이트된다.  
    c. 압축 단계를 통해 메모리 단편화가 해소되면 새로운 객체를 할당하기 위한 연속적인 메모리 공간이 확보된다.  
      
  
<br><br><br>
## Reference-counting 알고리즘
  
  
> Mark-Sweep-Compact 알고리즘은 `도달 가능성`을 기반으로 동작했다면  
Reference-counting 알고리즘은 `참조 횟수`를 기반을 동작한다.  
각 객체는 자신을 참조하는 다른 객체의 수를 나타내는 참조 카운트를 자기고  
참조 카운트 증감을 통해 참조 카운트가 0이 되면 필요하지 않은 값으로 간주하고 제거하는 방식이다.  
>
  
1. 객체가 새로 생성되면 참조 카운트는 1로 초기화된다.  
2. 다른 객체가 해당 객체를 참조할 때마다 참조 카운트가 1씩 증가한다.  
3. 객체에 대한 참조가 끊길 때마다 참조 카운트가 1씩 감소한다.  
4. 참조 카운트가 0이 되면 해당 객체는 더 이상 사용되지 않는 것으로 판단하고 메모리에서 해제한다.  
  
<br>
### Reference-counting의 단점
  
> 객체들이 서로를 참조하는 순환 참조가 발생하면  
참조 카운트가 0이 되지 않아 메모리가 해제되지 않을 수 있다.  
이로 인해 메모리 누수가 발생할 수 있다.  
이러한 문제는 참조 횟수가 아닌 도달 가능성을 중점으로둔  
Mark-Sweep-Compact 알고리즘을 통해 순환 참조 문제점을 보완할 수 있다.  
>
  
  
```jsx
function couple() {
    const jane = {};
    const sam = {};

    // jane.bf는 sam을 참조한다
    jane.bf = sam;

    // sam.gf는 jane을 참조한다
    sam.gf = jane;

    return 'circular';

}

couple();

````

- Reference-counting 기반이라면
couple()함수가 호출되고 끝나서 더이상 필요한 값이 아닌데도 불구하고 서로에 대한 참조가 걸려있기 때문에 GC는 이 값들에 대한 메모리를 해제하지 않아 계속해서 메모리에 남아 있게 된다.
- 반면 Mark-Sweep-Compact 기반이라면
Couple() 이라는 함수 가 호출된 후 circular값이 return 되고 끝난 후에는
더이상 root에서 jane과 sam에 도달할 수 없기 때문에
해당 값들은 GC에 의해서 메모리가 해제된다.

<br><br><br>
## 성능 개선의 필요성


> 가비지 컬렉션이 실행되는 동안
애플리케이션의 모든 실행 스레드를 일시적으로 중단시키는 `stop-the-world` 현상이 발생할 수 있는데
주로 메이저 GC에서 발생빈도가 높고 이로인해 애플리케이션의 응답성을 저하시킬 수 있다.
>


> 이 때문에 V8 엔진은 메이저 GC의 성능을 개선하기 위해 다양한 최적화 기법을 사용한다.
V8엔진의 가비지 컬렉터는 `Orinoco`, `Parallel`, `Incremental` 등의 기술을 사용한다.
>


<br>
### Orinoco

> Orinoco는
메이저 GC의 성능을 향상시키는 데 초점을 둔
V8 엔진의 최신 가비지 컬렉터로,
Parallel와 Incremental 기술을 통해 성능 개선을 도와준다.
>


<br>
### Parallel

> Parallel은
여러 스레드를 사용해서 가비지 컬렉션 작업을 병렬로 처리하는 기술로
메이저 GC의 마킹, 스위핑, 압축 단계를 병렬로 처리하도록 도와준다.
>


<img class='img-full' src='https://github.com/user-attachments/assets/a7bdbb52-b69d-412e-993e-765657619969' alt=''>

<br>
### Incremental

> Incremental 가비지 컬렉션은
V8 엔진이 가비지 컬렉션을 작은 단위로 나누어 점진적으로 수행하는 기술로
애플리케이션의 실행 중에 가비지 컬렉션을 수행해서
애플리케이션의 일시 중단 시간을 줄이고 응답성을 향상시킬 수 있게 도와준다.
>


<img class='img-full' src='https://github.com/user-attachments/assets/62e6e10d-55aa-4565-9b5f-14876c53abee' alt=''>

<br><br><br>
## 가비지 컬렉션이 안되는 경우


> 자바스크립트에서 객체가 가비지 컬렉터에 수집되는 시점을
프로그래밍 방식으로 감지할 수 있는 `FinalizationRegistry`라는 클래스를 제공한다.
이 클래스를 가지고 실제로 가비지 켈럭션이 안되는 경우는
어떤 경우가 있는지 살펴보자.
>


<br>
### 전역 변수


> 전역 변수는 애플리케이션이 실행되는 동안 계속해서 참조되므로,
가비지 컬렉션의 대상이 되지 않는다.
지역 변수는 함수 호출이 끝나게 될경우 자동으로 가비지 컬렉션 대상이된다.
>


```jsx
const registry = new FinalizationRegistry((message) => console.log(message));

let globalVar = "I am global";
registry.register({ globalVar }, "globalVar has been collected");

function doSomething() {
  // let localVar = "I am local";
  console.log(globalVar);
}

doSomething();
// 결과:
//
````

예방 방법

- 지역 변수 사용: 가능하면 전역 변수 대신 지역 변수를 사용해서 메모리 사용 범위를 제한한다.
- 네임스페이스 활용: 전역 변수가 필요한 경우, 객체 또는 네임스페이스를 사용해서 전역 스코프를 오염시키지 않도록 한다.
- 전역 변수 해제: 전역 변수가 더 이상 필요하지 않을 때 null 또는 빈 값으로 재할당해서 메모리 해제를 돕는다.

<br>
### 참조 유지
  
  
> 객체나 변수가 다른 객체나 변수에 의해 참조되고 있는 경우,  
해당 객체는 가비지 컬렉션의 대상이 되지 않는다.  
>
  
  
```jsx
const registry = new FinalizationRegistry((message) => console.log(message));

let obj = { name: "Alice" };
let reference = obj;
registry.register(obj, "obj has been collected");
registry.register(reference, "reference has been collected");

obj = null;
// reference = null;

// 결과:
// obj has been collected

````

예방 방법

- 순환 참조 방지: 객체 간의 순환 참조를 피하도록 코드 구조를 설계한다.
- 불필요한 참조 제거: 객체가 더 이상 필요하지 않을 때, 해당 객체에 대한 모든 참조를 null로 설정한다.

<br>
### 클로저


> 클로저를 사용해서 외부 함수의 변수를 참조하는 경우,
외부 함수가 종료되더라도 그 변수는 여전히 메모리에 남아 있다.
내부 함수가 외부 함수의 변수를 참조하는 한,
해당 변수는 내부 함수의 렉시컬 환경을 통해 도달 가능하게 되기때문이다.
>


```jsx
const registry = new FinalizationRegistry((message) => console.log(message));

function createCounter() {
  let count = 0;
  registry.register({ count }, "count has been collected"); // count 객체 등록
  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
// counter = null;

// 결과:
//
````

예방 방법

- 클로저 생명 주기 관리: 클로저의 생명 주기를 명확히 파악하고 관리한다.
- 클로저 사용 최소화: 클로저 사용이 꼭 필요한 경우에만 사용하고, 불필요한 클로저 생성을 피한다.
- 클로저 해제: 클로저 사용 후, 더 이상 필요하지 않은 변수는 null로 설정해서 참조를 해제한다.

<br>
### 이벤트 리스너
  
  
> DOM 요소에 이벤트 리스너를 추가한 경우,  
해당 요소가 제거되더라도  
이벤트 리스너가 참조되고 있다면 메모리에서 해제되지 않는다.  
>
  
  
```jsx
const registry = new FinalizationRegistry((message) => console.log(message));

const button = document.createElement("button");
button.textContent = "Click me";

function handleClick() {
console.log("Button clicked");
}

button.addEventListener("click", handleClick);
registry.register(handleClick, "handleClick has been collected");

document.body.appendChild(button);
// button.removeEventListener("click", handleClick);

````

예방 방법

- 이벤트 리스너 제거: DOM 요소가 제거될 때, removeEventListener 함수를 사용해서 이벤트 리스너도 함께 제거한다.
- 이벤트 위임: 이벤트 위임을 사용해서 이벤트 리스너 수를 줄인다.

<br>
### 타이머


> setTimeout이나 setInterval로 설정한 타이머가 여전히 활성화되어 있는 경우,
해당 타이머가 참조하는 변수는 가비지 컬렉션의 대상이 되지 않는다.
>


```jsx
const registry = new FinalizationRegistry((message) => console.log(message));

let timerId;

function startTimer() {
  let count = 0;
  timerId = setInterval(() => {
    count++;
    console.log(count);
  }, 1000);
  registry.register({ timerId }, "timerId has been collected");
}

function stopTimer() {
  clearInterval(timerId);
}

startTimer();
// setTimeout(stopTimer, 5000);

// 결과:
//
````

예방 방법

- 타이머 관리: 타이머의 생명 주기를 명확히 파악하고 관리한다.
- 타이머 제거: 더 이상 필요하지 않은 타이머는 clearInterval 또는 clearTimeout을 사용해서 제거한다.

<br><br><br>
**reference**

- <a href='https://fe-developers.kakaoent.com/2022/220519-garbage-collection/' target='_blank'>https://fe-developers.kakaoent.com/2022/220519-garbage-collection/</a>
- <a href='https://velog.io/@surim014/Experiments-with-the-JavaScript-Garbage-Collector' target='_blank'>https://velog.io/@surim014/Experiments-with-the-JavaScript-Garbage-Collector</a>
