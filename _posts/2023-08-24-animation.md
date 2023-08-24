---
title: 'transition과 animation 속성의 차이'
excerpt_separator: '<!--more-->'
categories:
  - HTML CSS
tags:
  -
header:
  teaser: ./assets/image/35.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# animation과 transition

> transition과 animation 속성은  
> *플래시의 기술이나 자바스크립트의 도움 없이*  
> 웹 요소에 직접 **애니메이션 효과를 적용하는 CSS 기술**이다  
> 이러한 속성을 사용해서 웹 요소의 스타일 속성을 변경하고 애니메이션 효과를 부여할 수 있다

💡 <u>플래시의 기술이나 자바스크립트의 도움없이?</u>  
<br>
플래시(**Flash**)와 자바스크립트는(**JavaScript**)는  
transition과 animation 속성이 도입되기 이전에  
웹 페이지에서 **동적인 효과와 애니메이션을 구현하기 위해 주로 사용되었던 방식**이다
{:.notice--primary}

<br>

- 자바스크립트는  
  - 웹 페이지에 동적인 행동과 상호작용을 구현하기 위한 프로그래밍 언어로 널리 사용되었다  
  - 애니메이션을 만들기 위해 **DOM 요소의 스타일과 속성을 변경**하거나 **타이머와 함께 사용하여 움직임을 만들 수 있다**  
  - 하지만 자바스크립트만으로 **애니메이션을 구현하면 코드가 복잡**해질 수 있고   
    무거운 **동적 콘텐츠를 만들 경우 성능 문제**가 있을 수 있었다  
    또한 브라우저 호환성과 웹 표준을 준수하기 어려운 경우도 있었다  

- 플래시는  
  - 웹 페이지 내에서 다양한 멀티미디어 콘텐츠와  
    인터랙티브한 애니메이션을 생성하고 재생하기 위한 플랫폼이다  
  - 하지만 자바스크립트와 마찬가지로 **브라우저 호환성의 문제**가 존재했고  
    **보안 문제** 또한 있었다

💡 즉 브라우저 호환성을 강화하고,  
&emsp;성능을 최적화하고,   
&emsp;웹표준을 준수하고,   
&emsp;간편하게 사용할 수 있는   
&emsp;**플래시와 자바스크립트를 대체하기 위한 기술이 필요**했고  
&emsp;그렇게 개발된 것이 **CSS 애니메이션 기술**이다
{:.notice--primary}

<br>

## 애니메이션 효과를 부여함으로써 얻을 수 있는 장점?

- 시작적 매력 : 애니메이션은 사용자의 시선을 끌고 웹 요소를 더 동적이고 매력적으로 만들어준다  
- 사용자 경험 향상 : 부드러운 전환 효과나 요소의 확대/축소 등을 통해 사용자와의 상호작용을 강화시킬 수 있다  
- 정보 전달 강화 : 움직이는 그래프나 차트를 통해 데이터 변화를 보다 효과적으로 전달 할 수 있다  
- 컨텍스트 제공 : 사용자에게 화면의 구조나 계층을 더 명확하게 이해할 수 있도록 도와준다

💡 이러한 장점들을 가진 **애니메이션을 자바스크립트 없이도 간단하게 만들 수 있다**
{:.notice--primary}

<br>
<br>

## transition 속성

> CSS 애니메이션의 일종으로,  
> CSS 프로퍼티 값이 변화하면 즉각적으로 그 효과가 나타나는데  
> transition은 프로퍼티 값의 변화가 일정시간에 걸쳐 일어나도록 하는 것이다  
> 
> 즉, **요소의 스타일을 직접 변경하는 것이 아니고**  
> 요**소의 스타일 변화가 있을 때에만** 애니메이션 효과가 **작동**하도록 하는 것이다

💡 <u>transition 속성을 활용되는 경우</u>    
<br>
&emsp;&emsp;◼ `:hover` , `:active` , `:focus` 와 같은 **가상 클래스 선택자(Pseudo-Classes)를 사용하여 요소에 상호작용 효과를 부여할때**  
&emsp;&emsp;◼ 버튼을 클릭하면 모달창이 열리거나 배경색이 변경되는 등 **JavaScript의 부수적인 액션이 있을 때**  
&emsp;&emsp;◼ transform 속성을 이용해서 요소에 이동, 회전, 확대/축소, 비틀기 효과를 부여할 때 
{:.notice--primary}

⚠️ 모든 CSS 프로퍼티가 트랜지션의 대상이 될 수 없다는 것이다  
&emsp;예를 들어 width, font-size, background-color 등은   
&emsp;하나의 범주안에서 크기나 색상의 값이 변화가 가능하지만 display 프로퍼티는 그렇지 않다
{:.notice--warning}

<br>

### transition 속성값들
---

```css
transition: property | duration | timing-function | delay;
```

<br>
🔎 property : 애니메이션을 적용할 스타일 속성을 지정한다  
<br>
🔎 duration : 전환되는 애니메이션의 지속시간을 지정한다  
<br>
🔎 timing-function : 애니메이션의 타이밍을 조절하는 함수를 지정한다  
&emsp;함수로는 `ease` | `linear` | `ease-in` | `ease-out` | `ease-in-out` 등이 있다  
<br>
🔎 delay : 애니메이션이 시작하기 전의 지연 시간을 지정한다  
<br>
{:.notice--info}

<br>

```css
.element {
  width: 100px;
  transition: width 0.3s ease 1s;
}

.element:hover {
  width: 200px;
}
```

✔️ 위 코드는 요소에 호버할 때 width 속성의 변화가 1초 후에 0.3 초 동안 부드럽게 100px 에서 200px로 변화된다
{:.notice--success}

<br>
<br>

## animation 속성

> CSS 애니메이션의 일종으로,  
> 여러 개의 애니메이션 단계를 정의하고 이를 순차적으로 실행하거나 반복할 수 있다  
> 그러기 위해선 특별한 속성이 쓰여지는데  
> 이를 @keyframes 이라고 하고 animation 속성과 함께 사용된다  
> 이 **@keyframes** 를 통해 **요소의 시작부터 끝까지 모든 중간 프레임을 정의할 수 있다**

💡 `@keyframes`  
<br>
&emsp;**from 과 to 또는 백분율을 사용**해서 **중간 프레임을 정의**할 수 있다  
&emsp;이를 통해 요소의 상태 어떻게 변화해야하는지 세밀하게 제어할 수 있다
{:.notice--primary}

```css
@keyframes animationName {
 /* from, to 로 중간 프레임 정의 */
  from {
    /* 시작 상태 */
  }
  to {
    /* 끝 상태 */
  }
  /* 백분율로 중간 프레임 정의 */
  0% {
    /* 상태 */
  }
  50% {
    /* 상태 */
  }
  100% {
    /* 상태 */
  }
}
```

<br>

### animation 속성값들
---

```css
animation: name | duration | timing-function | delay | iteration-count | direction | fill-mode play-state;
```

🔎 name : 애니메이션 이름을 나타낸다  
<br>
🔎 duration : 애니메이션의 지속시간을 지정한다  
<br>
🔎 timing-function : 애니메이션의 타이밍을 조절하는 함수를 지정한다  
<br>
🔎 delay : 애니메이션이 시작하기 전의 지연 시간을 지정한다  
<br>
🔎 iteration-count : 애니메이션 반복 횟수를 지정하고, infinite로 무한반복 할 수 있다  
<br>
🔎 direction : 애니메이션 진행 방향을 지정한다  
<br>
🔎 fill-mode: 애니메이션 실행 전과 후의 스타일을 제어한다  
<br>
🔎 play-state: 애니메이션을 일시 중지하거나 다시 실행한다 기본값은 running이다  
{:.notice--info}

<span class='explain'>direction</span>

| 프로퍼티값 | 설명 |
| --- | --- |
| normal | 기본값으로 from(0%)에서 to(100%) 방향으로 진행한다 |
| reverse | to에서 from 방향으로 진행한다 | 
| alternate | 홀수번째는 normal로, 짝수번째는 reverse로 진행한다 | 
| alternate-reverse	 |	홀수번째는 reverse로, 짝수번째는 normal로 진행한다	| 

<span class='explain'>fill-mode</span>

| 프로퍼티값 | 상태 |	설명 |
| --- | --- | --- |
| none |	대기 | 시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다 |
| | 종료 | 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다 |
| forwards |	대기 |	시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다 |
| | 종료 | 종료 프레임(to)에 설정한 스타일을 적용하고 종료한다 |
| backwards |	대기 | 시작 프레임(from)에 설정한 스타일을 적용하고 대기한다 |
| | 종료 | 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다 |
| both | 대기 |	시작 프레임(from)에 설정한 스타일을 적용하고 대기한다 |
| | 종료 |	종료 프레임(to)에 설정한 스타일을 적용하고 종료한다 |

<br>

```css
@keyframes slideAndRotate {
  0% {
    transform: translateX(0) rotate(0);
    opacity: 1;
  }
  50% {
    transform: translateX(200px) rotate(180deg);
    opacity: 0.5;
  }
  100% {
    transform: translateX(400px) rotate(360deg);
    opacity: 0;
  }
}

.element {
  width: 100px;
  height: 100px;
  background-color: blue;
  position: relative;
  animation: slideAndRotate 4s ease-in-out 1s infinite alternate both paused;
}

.element:hover {
  animation-play-state: running;
}
```

✔️ 애니메이션의 실행 초기에는 정지된 상태(<u>paused</u>)이다  
&emsp;요소에 hover가 되면 애니메이션(<u>slideAndRotate</u>)이 시작된다  
&emsp;애니메이션은 1초(<u>1s</u>) 후에 시작되고 4초(<u>4s</u>) 동안 지속 된다  
&emsp;무한반복(<u>infinite</u>) 하면서 번갈아 가며 방향을 바꾸고(<u>alternate</u>)  
&emsp;애니메이션 종료 시 초기 상태로 되돌아갑니다
{:.notice--success}

<br>

## transition과 animation 속성의 차이점

> transition 속성은 요소의 프로퍼티값이 다른 값으로 변화할 때 애니메이션을 실행하기 때문에   
> 요소가 로드될 때 자동으로 발동되지 않는다  
> 반면에, animation 속성은 하나의 @keyframes이라는 속성으로 프레임을 정의하고,  
> 그 안에서 세부 움직임을 시간 단위로 제어할 수 있으며, 전체 프레임의 재생, 정지 및 반복을 제어할 수 있다  
>  
> 다시 말해, **transition 속성은 요소의 상태 변화가 발생할 때만 그 상태 변화를 애니메이션으로 표현**하지만,  
> **animation 속성은 요소의 상태 변화와는 무관하게 애니메이션을 실행**한다

<br>

## transition과 animation 속성의 성능

### transition

- 장점
  - 예를 요소의 색상, 크기, 위치 등의 속성 변화에 사용되고 간단한 변화에 적합하기 때문에  
    **브라우저에 큰 부하를 주지않고 성능측면에서 부하가 적다**
  - 많은 브라우저가 transition을 하드웨어 가속으로 처리하여 부드럽고 효율적인 애니메이션을 구현할 수 있다
  - 단순한 트랜지션 효과를 위한 **간단한 설정만으로도 적용이 가능하므로 초기 구현이 쉽다**

- 단점
  - 복잡한 애니메이션을 다루기에는 한계가 있고 **중간 프레임이나 여러 단계를 다루기 어렵다**
  - 속성의 변화가 transition과의 지속시간 동안에 일어나야 하며, transition과의 지속시간을 초과하면 더 이상 변화할 수 없다

### Animation

- 장점
  - @keyframes 을 사용하여 여러 단계 및 중간 프레임을 정의할 수 있기 때문에 **복잡한 애니메이션을 다루기에 적합하다**
  - 다양한 타이밍 함수와 시간 지연, 무한 반복 등을 적용할 수 있다
  - 자연스러운 동작보다는 강조된 움직임이 필요한 경우에 유용하다

- 단점
  - 브라우저가 **애니메이션을 처리할 때 레이어를 생성하여 독립적으로 처리**하는데, **이로 인해 메모리와 렌더링 성능에 영향을 미칠 수 있다**
  - **너무 많은 레이어 생성은 성능 저하를 유발**할 수 있다
  - 애니메이션의 중간 프레임이 부드럽지 않게 보일 수 있다
  - 애니메이션이 **복잡하거나 지속적으로 반복될 경우 최적화를 고려**해야 한다

💡 <u>요약하면</u>   
<br>
&emsp;transition은 간단한 상태 변화에 적합하고, animation은 복잡한 애니메이션을 다루는 데 적합하다   
&emsp;따라서, **어떤 것을 선택할지는 필요한 애니메이션의 복잡성과 원하는 효과에 따라 결정**해야 한다
{:.notice--primary}

<br>

## 질문

<details>
  <summary>✨ CSS의 animation과 transition의 차이점은 무엇일까?</summary><br>
    transition 속성은 요소의 상태 변화에 따라 애니메이션을 적용하기 때문에 단순한 속성 변화를 다루기에 적합하며,<br>
    요소가 로드될 때 자동으로 적용되지 않습니다.<br>
    반면에, animation 속성은 하나의 @keyframes이라는 속성으로 프레임을 정의하고, <br>
    그 안에서 세부 움직임을 시간 단위로 제어할 수 있으며, 전체 프레임의 재생, 정지 및 반복을 제어할 수 있습니다.
</details>

<br>

<details>
  <summary>✨ CSS 애니메이션 사용 시 주의할 점은 무엇일까?</summary><br>
    브라우저에서 애니메이션을 처리할 때 많은 레이어를 생성하면 메모리 부족, 렌더링 지연, 성능 저하 등의 문제가 발생할 수 있습니다. <br>
    따라서 복잡한 애니메이션은 프레임 드롭, 끊김 현상, 페이지의 반응성 저하를 유발할 수 있습니다. <br>
    뿐만 아니라 이로 인해 사용자의 경험이 저하될 수 있으므로 애니메이션을 구현할 때는 최적화와 렌더링 성능에 신경을 써야 합니다. <br>
    따라서 애니메이션 속성을 최소화하고, 하드웨어 가속을 활용하며, 애니메이션 효과를 필요한 부분에만 적용하는 등의 방법을 고려해야합니다.
</details>

<br>

<details>
  <summary>✨ CSS transition과 animation 중 어떤 것을 사용하여 애니메이션 효과를 구현하는 것이 좋을까요? 그 이유는 무엇인가요?</summary><br>
    transition은 요소의 속성이 변화할 때, 변화가 일어나는 동안에만 애니메이션을 적용하는 속성이기 때문에 <br>
    색상, 크기, 위치 등의 간단한 속성 변화에 사용하는 것이 적합하고<br>
    animation은 keyframe을 사용해서 요소의 속성을 여러 단계로 변화시킬 수 있으므로 <br>
    복잡하고 정교한 애니메이션 효과를 구현하는데 유용합니다. <br>
    예를 들어, 버튼에 마우스를 가져다 대면 부드러운 변화를 주는 transition을 사용하고, <br>
    로딩 중인 컴포넌트에 대해서는 더 복잡한 animation을 적용할 수 있습니다.<br>
    즉, 어떤 것을 선택할지는 필요한 애니메이션의 복잡성과 원하는 효과에 따라 결정하는 것이 바람직합니다.
</details>

<br>
<br>

<span class='explain'>Reference</span> 

- [inpa.tistory.com](https://inpa.tistory.com/entry/CSS-%F0%9F%93%9A-%ED%8A%B8%EB%9E%9C%EC%A7%80%EC%85%98-%ED%8A%B8%EB%9E%9C%EC%8A%A4%ED%8F%BC-%EC%95%A0%EB%8B%88%EB%A9%94%EC%9D%B4%EC%85%98)
- [webclub.tistory.com](https://webclub.tistory.com/620)
