---
title: '자바스크립트 동작원리'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/58.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# JavaScript  
  
> 자바스크립트(JavaScript)는 싱글 스레드(Single Thread) 기반의 프로그래밍 언어로  
모든 작업을 단일 호출 스택(Call Stack) 을 이용하여 실행한다.  
>
  
  
> 단일 호출 스택 자체는 동기적이다   
함수 실행 중에는 다음 함수가 실행될 때까지  
해당 함수의 실행이 끝나기를 기다리며, 다른 함수가 호출될 수 없다  
즉 한 번에 하나의 작업만 처리할 수 있기 때문에 여러 작업을 동시에 처리하는 멀티태스킹에 취약하고  
긴 작업이 있는 경우 다른 작업이 대기해야 하는 문제 즉 블로킹(blocking)이 발생할 수 있다  
>
  
  
> 이러한 단점을 극복하고자   
자바스크립트는 자바스크립트 엔진, Web API, 이벤트 루프(Event Loop), Task Queue, Microtask Queue를 활용하여  
하나의 콜 스택으로도 여러 개의 작업을 처리할 수 있다  
>
  
  
<br><br><br>
## **자바스크립트 엔진(JavaScript Engine)**
  
  
> 자바스크립트 엔진은 자바스크립트 코드를 실행하는 프로그램 또는 인터프리터(해석기) 이다.  
대부분 웹 브라우저에서 사용되며, 자바스크립트 코드를 해석하여 실행하는 역할을 한다.  
자바스크립트는 인터프리터 방식을 사용하므로 별도의 컴파일 과정 없이 웹 브라우저에서 즉시 실행(런타임) 된다.  
>
  
  
> 자바스크립트 엔진은 브라우저마다 다르다.  
현재 가장 많이 사용되는 엔진은 Google의 V8 엔진이다.  
>
  
1. **V8 (Chrome, Node.js)**  
    - 구글이 개발한 오픈소스 자바스크립트 엔진.  
    - C++로 작성되었으며 **Chrome 브라우저와 Node.js에서 사용됨**.  
    - 가장 널리 사용되는 엔진이며, 빠른 성능을 제공.  
2. **SpiderMonkey (Firefox)**  
    - **자바스크립트 최초의 엔진**.  
    - 넷스케이프 브라우저를 위해 **브렌던 아이크(자바스크립트 창시자)** 가 개발.  
    - 현재는 Mozilla 재단에서 관리하며 **Firefox 브라우저에서 사용됨**.  
3. **Chakra (Edge - 구버전)**  
    - Microsoft에서 개발한 엔진으로, **Edge 브라우저에서 사용되었음**.  
    - Chakra Core라는 오픈소스 프로젝트를 기반으로 함.  
4. **JavaScriptCore (Safari, React Native)**  
    - Apple에서 개발한 엔진으로, **Safari 브라우저 및 React Native에서 사용됨**.  
  
<br><br><br>
## 자바스크립트 실행 구조와 비동기 처리 원리
  
<img class='img-full' src='https://github.com/user-attachments/assets/9ba1eca3-5daf-4612-8e99-1e4d31589b56' alt=''>
  
<br>
### 자바스크립트 실행 구조
  
  
**`콜 스택(Call Stack)`**  
  
> 콜 스택은 코드 실행 순서를 관리하는 공간이다.  
LIFO(Last In, First Out, 후입선출) 구조로 동작하는데  
함수가 호출되면 콜 스택에 추가(push)되고, 실행이 완료되면 제거(pop)된다.  
재귀 호출이 많거나 깊은 호출 체인이 발생하면 스택 오버플로우가 발생할 수 있다.  
>
  
  
**`메모리 힙(Memory Heap)`**  
  
> 동적 메모리를 저장하는 공간으로,   
크기가 유동적이고 런타임 동안 필요한 메모리를 할당할 수 있다.  
콜 스택에서는 해당 데이터의 주소값만 저장하며, 실제 데이터는 메모리 힙에 저장된다.  
과도한 객체 생성이나 해제가 이루어지지 않으면 메모리 누수가 발생할 수 있는 특징이 있다.  
>
  
  
<br>
### 비동기 처리 원리
  
**`Web API`**  
  
> Web API는 브라우저가 제공하는 비동기 기능을 실행하는 역할로  
자바스크립트 엔진(V8) 자체는 비동기 기능을 제공하지 않으며, Web API가 이를 대신 수행한다.  
>
  
  
> 자바스크립트 엔진이 실행하는 코드 중에서 비동기 작업이 있으면,  
이를 Web API가 처리한 후, 실행이 완료된 콜백을 Task Queue 또는 Microtask Queue에 추가한다.  
>
  
  
> Web API는 다양한 기능을 제공하는데,  
비동기적인 작업을 처리하는 API들이 있다.  
브라우저가 제공하는 API이므로, 브라우저 환경에서만 사용할 수 있는 것도 포함된다.  
>
  
  
**Web API들**  
  
| 기능 | API 예시 | 설명 |  
| --- | --- | --- |  
| **DOM API** | `document.querySelector()`, `getElementById()` | HTML 요소를 선택하고 조작하는 기능 |  
| **Timer API** | `setTimeout()`, `setInterval()` | 일정 시간이 지난 후 콜백을 실행하는 기능 |  
| **Event API** | `addEventListener()` | 사용자 입력(클릭, 키 입력 등)을 감지하고 처리하는 기능 |  
| **XHR & Fetch API** | `XMLHttpRequest`, `fetch()` | 네트워크 요청을 수행하고 응답을 받는 기능 |  
| **requestAnimationFrame** | `requestAnimationFrame()` | 화면을 다시 그리기 전에 실행될 애니메이션을 최적화하는 기능 |  
| **WebSockets API** | `WebSocket()` | 서버와의 실시간 양방향 통신을 제공하는 기능 |  
| **Geolocation API** | `navigator.geolocation.getCurrentPosition()` | 사용자의 위치 정보를 가져오는 기능 |  
| **Web Storage API** | `localStorage`, `sessionStorage` | 브라우저에 데이터를 저장하는 기능 |  
| **Notification API** | `Notification.requestPermission()`, `new Notification()` | 푸시 알림을 전송하는 기능 |  
| **IndexedDB API** | `indexedDB.open()` | 브라우저 내부 데이터베이스에 데이터를 저장하는 기능 |  
  
<br>

**`Task Queue (Callback Queue)`**  
  
> Task Queue는 비동기 작업이 완료된 후 실행될 콜백을 저장하는 대기열(Queue)로  
Web API에서 실행된 setTimeout(), setInterval(), requestAnimationFrame() 같은 작업이 여기에 추가된다.  
이벤트 루프(Event Loop)가 실행되면서   
콜 스택이 비어 있으면 Task Queue에서 실행할 작업을 가져와 실행한다.  
>
  
  
**Task Queue의 실행 조건**  
  
- 콜 스택이 비어 있어야 실행 가능하다.  
- Microtask Queue가 모두 실행된 후에 실행된다.  
- 동일한 실행 루프에서 한 번에 하나의 Task만 실행된다.  
  
**Task Queue에 포함되는 함수**  
  
| 기능 | 포함되는 함수 | 설명 |  
| --- | --- | --- |  
| **Timer API** | `setTimeout()`, `setInterval()` | 일정 시간이 지난 후 콜백 실행 |  
| **Event API** | `addEventListener()` (UI 이벤트) | 클릭, 키 입력, 마우스 이동 등의 UI 이벤트 핸들러 |  
| **XHR & Fetch API** | `setTimeout(() => fetch().then(...))` | `fetch()` 자체는 Microtask Queue에 있지만, `setTimeout()`을 감싸면 Task Queue로 이동 |  
| **setImmediate (Node.js 전용)** | `setImmediate()` | 현재 이벤트 루프가 완료된 직후 실행됨 (Node.js 환경) |  
  
<br>

**`Microtask Queue`**  
  
> Microtask Queue는 비동기 작업 중에서도 우선순위가 높은 작업을 저장하는 큐이다.   
Promise.then(), MutationObserver() 같은 작업이 저장된다.   
Task Queue보다 먼저 실행된다.  
>
  
  
**Microtask Queue의 실행 조건**  
  
- 콜 스택이 비어야 실행 가능하다.  
- Task Queue보다 먼저 실행된다.  
- 동일한 이벤트 루프 사이클에서 Microtask Queue의 모든 작업이 먼저 실행된다.  
  
**Microtask Queue에 포함되는 함수**  
  
| 기능 | 포함되는 함수 | 설명 |  
| --- | --- | --- |  
| **Promise API** | `Promise.then()`, `Promise.catch()`, `Promise.finally()` | 비동기 작업이 완료된 후 실행되는 콜백 |  
| **MutationObserver** | `new MutationObserver(callback).observe()` | DOM 변경을 감지하는 비동기 이벤트 |  
| **queueMicrotask** | `queueMicrotask(callback)` | 개발자가 명시적으로 Microtask Queue에 함수를 추가하는 방법 |  
| **process.nextTick (Node.js 전용)** | `process.nextTick()` | Node.js에서 현재 실행 중인 작업이 끝나기 전에 실행되는 비동기 작업 |  
  
<br>

**`Animation Frames Queue`**  
  
> Animation Frames Queue는 브라우저가 화면을 다시 그리기 전에 실행되는 큐로  
`Microtask Queue` 와 `Task Queue (Callback Queue)` 사이의 우선순위를 가진다.  
애니메이션을 부드럽게 동작하도록 최적화할 수 있다.  
>
  
<br>
  
**Animation Frames Queue에 포함되는 함수**  
  
- **`requestAnimationFrame()`** : 브라우저가 다음 프레임을 렌더링하기 전에 실행할 콜백 함수를 예약하는 함수로 화면을 다시 그리기 전에 실행될 애니메이션 업데이트 로직을 최적화할 수 있다.  
  
<br>

**`이벤트 루프(Event Loop)`**  
  
> 이벤트 루프(Event Loop)는   
Call Stack과 Callback Queue(Task Queue)의 상태를 체크해서  
Call Stack이 빈 상태가 되면, Callback Queue에서 작업을 가져와 실행한다.  
즉, 동기 코드가 실행되는 동안 비동기 작업은 Web API에서 처리되고 이후 큐에서 실행된다.  
>
  
  
**이벤트 루프의 실행 과정**  
  
1. 콜 스택이 비어 있는지 확인한다.  
2. Microtask Queue에 대기 중인 작업이 있으면 즉시 실행한다.  
3. Microtask Queue가 비어 있으면 Task Queue에서 하나의 작업을 실행한다.  
4. 이 과정을 무한 반복하면서 비동기 작업을 실행한다.  
  
<br>
### 실행 과정 예제
  
```jsx
console.log("1. 시작");

setTimeout(() => console.log("2. setTimeout 실행"), 0);

Promise.resolve().then(() => console.log("3. Promise 실행"));

console.log("4. 종료");
```
  
1. **콜 스택(Call Stack)**  
    - 동기 코드 실행을 담당하며, 함수가 실행되면 스택에 추가(push), 실행이 끝나면 제거(pop)된다.  
    - `console.log("1. 시작")`, `console.log("4. 종료")` 등 동기 코드는 즉시 실행되고 실행이 끝나면 콜 스택에서 제거된다.  
2. **Web API**  
    - 비동기 코드(`setTimeout()`, `Promise.then()` 등)를 처리한다.  
    - `setTimeout()`은 Web API에서 처리된 후, 대기열(Task Queue)에 콜백을 추가하고, Web API에서 제거된다.  
3. **Task Queue**  
    - Web API에서 실행된 `setTimeout()`의 콜백이 추가된다.  
    - 이벤트 루프가 Microtask Queue를 모두 처리한 후, Task Queue에서 콜백을 가져와 실행하며, 실행이 끝나면 Task Queue에서 제거된다.  
4. **Microtask Queue**  
    - `Promise.then()` 같은 작업을 저장하며, Task Queue보다 먼저 실행된다.  
    - Microtask Queue의 모든 작업이 완료되면, 큐는 비워진다.  
5. **이벤트 루프(Event Loop)**  
    - 콜 스택이 비어 있으면, Microtask Queue → Task Queue 순서로 실행된다.  
    - Microtask Queue를 모두 처리한 후, Task Queue에서 대기 중인 작업을 실행하며, 실행이 끝나면 큐에서 제거된다.  
    - 모든 작업이 완료되면, 콜 스택과 모든 큐가 비워진 상태가 된다.  
  
<br><br><br>
## **웹 워커(Web Worker)**
  
  
웹 워커(Web Worker)는 메인 스레드와 분리된 별도의 백그라운드 스레드에서 코드 실행을 가능하게 하는 기술이다.  
웹 워커를 사용하면 무거운 연산을 수행하더라도 UI가 멈추지 않고 부드럽게 동작할 수 있다.  
웹 워커의 대표적인 종류에는 전용 워커(Dedicated Worker), 공유 워커(Shared Worker), 서비스 워커(Service Worker)가 있다.  
  
| 항목 | 설명 |  
| --- | --- |  
| **비동기 실행** | 워커는 **별도의 스레드에서 실행되며**, 메인 스레드를 블로킹하지 않음. |  
| **DOM 조작 불가능** | 워커 내부에서는 `document` 객체를 직접 조작할 수 없음. |  
| **메시지 기반 통신** | `postMessage()`와 `onmessage`를 사용하여 데이터를 주고받음. |  
| **네트워크 요청 가능** | `XMLHttpRequest` 또는 `fetch()`를 사용할 수 있음. |  
| **워커 내부에서 추가 워커 생성 가능** | **전용 워커(Dedicated Worker)는 추가적인 워커를 생성할 수 있지만, 서비스 워커(Service Worker)는 불가능함.** |  
| **다른 출처(CORS) 제한** | 워커는 동일한 출처(Same-Origin) 정책을 따라야 하며, 다른 도메인의 스크립트를 로드하려면 `CORS` 설정이 필요함. |  
  
<br>
### **웹 워커의 단점**
  
- **DOM 조작 불가:** HTML을 직접 변경할 수 없다.  
- **메시지 전송 비용:** `postMessage()` 사용 시 데이터가 복사된다. (대량 데이터 전송 시 성능 저하 가능).  
- **추가적인 메모리 사용:** 워커마다 별도의 스레드를 생성하므로 리소스 사용량 증가할 수 있다.  
  
<br>
### **웹 워커 활용**
  
> **과도한 워커 사용은 성능 저하를 유발할 수 있다.  
메모리 소비량을 고려하여 필요한 경우에만 워커를 생성해야 한다.  
UI 관련 작업은 메인 스레드에서, 무거운 연산은 웹 워커에서 실행하는 것이 이상적이다.**  
>
  
- **CPU 연산이 많은 작업 처리**  
    - 이미지 필터링, 데이터 분석, 암호화, 압축 등  
    - 예: **대량의 데이터를 처리할 때 UI가 멈추지 않도록 백그라운드에서 실행**  
- **실시간 데이터 처리**  
    - **WebSocket**과 결합하여 실시간 데이터 스트리밍을 처리  
    - 예: **금융 거래소 데이터, 채팅 애플리케이션 등**  
- **대규모 계산 수행**  
    - **AI 모델 실행, 기계 학습 연산**  
    - 예: **이미지 인식, 음성 분석, 게임 물리 엔진 등**  
  
<br>
### **웹 워커의 동작 원리**
  
> 웹 워커는 별도의 스크립트 파일을 독립된 스레드에서 실행하는 객체이다.   
JavaScript 파일을 지정하여 `Worker()` 생성자로 실행하며,   
워커 스레드는 메인 스레드와 별도의 전역 컨텍스트에서 동작한다.   
웹 워커는 `postMessage()`로 데이터를 주고받으며, `onmessage` 이벤트를 통해 응답을 수신한다.   
DOM 조작은 불가능하며, CPU 집약적인 연산이나 비동기 작업 처리에 적합하다.  
>
  
  
```jsx
// worker.js
self.onmessage = function(event) {
  const result = event.data * 2; // 받은 데이터의 2배 반환
  self.postMessage(result); // 메인 스레드로 결과 전송
};
```
  
```jsx
// main.js (메인 스레드 코드)
const worker = new Worker("worker.js"); // 웹 워커 생성

worker.postMessage(5); // 워커에 데이터 전송

worker.onmessage = function(event) {
  console.log("웹 워커 결과:", event.data); // 워커로부터 데이터 수신
};
```
  
<br>
### **전용 워커 (Dedicated Worker)**
  
- 특정 **메인 스레드에서만 실행되는 워커**.  
- **한 개의 스크립트에서만 사용 가능** (다른 페이지에서는 접근 불가).  
- `Worker()` 생성자로 생성.  
- 주로 CPU 연산이 많은 작업을 메인 스레드와 분리하기 위해 사용.  
  
<br>
### **공유 워커 (Shared Worker)**
  
- 여러 개의 스크립트(창, 탭, IFrame 등)에서 공유할 수 있는 워커.  
- 같은 도메인 내의 여러 브라우징 컨텍스트에서 동일한 워커를 사용 가능.  
- `SharedWorker()` 생성자로 생성.  
- 스크립트 간 메시지 포트를 통해 통신해야 함.  
- 전용 워커와 달리, 공유 워커는 여러 브라우저 컨텍스트에서 동시에 사용 가능하다.  
- 공유 워커와 클라이언트 사이에서 메시지를 주고받기 위해 사용하는 통신 채널인 활성 포트를 사용하여 통신해야 한다.  
  
```jsx
// shared-worker.js (공유 워커)
self.onconnect = function(event) {
  const port = event.ports[0];
  port.onmessage = function(event) {
    port.postMessage("공유 워커에서 받은 메시지: " + event.data);
  };
};
```
  
```jsx
// main.js (메인 스레드)
const worker = new SharedWorker("shared-worker.js"); // 공유 워커 생성
worker.port.start();

worker.port.postMessage("Hello Shared Worker!"); // 메시지 전송

worker.port.onmessage = function(event) {
  console.log(event.data); // 공유 워커의 응답 수신
};
```
  
<br>
### **서비스 워커 (Service Worker)**
  
- 웹 브라우저, 네트워크, 서버 간의 프록시 역할을 하는 백그라운드 스크립트.  
- 오프라인 캐싱, 푸시 알림, 백그라운드 동기화 등을 처리.  
- 브라우저가 종료되더라도 백그라운드에서 실행될 수 있음.  
- `navigator.serviceWorker.register()`로 등록.  
- 서비스 워커는 페이지의 수명 주기와 독립적으로 실행된다.  
- PWA(Progressive Web App) 구현 시 필수적이다.  
  
```jsx
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(() => console.log("서비스 워커 등록 완료"))
    .catch(error => console.log("서비스 워커 등록 실패", error));
}
```
  
<br><br><br>
**reference**
  
  
- <a href='https://velog.io/@surim014/the-v8-javascript-engine' target='_blank'>https://velog.io/@surim014/the-v8-javascript-engine</a>
- <a href='https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API' target='_blank'>https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API</a>