---
title: '브라우저 렌더링 과정'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/48.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 브라우저에 URL을 입력했을 때의 흐름

웹 브라우저에 URL을 입력하면, 먼저 DNS 조회를 통해 도메인 이름을 IP 주소로 변환하고  
이후 브라우저는 해당 서버에 TCP 연결을 설정하고, 이를 통해 HTTP 요청을 보내게 된다.  
그럼 서버는 요청을 처리하고, 웹 페이지 콘텐츠를 담은 HTTP 응답을 반환하게 되고  
브라우저는 받은 데이터를 `렌더링`하여 사용자에게 보여주고, 마지막으로 TCP 연결을 종료한다.

그럼 이 `렌더링` 과정에서는 어떤 일이 일어날까?

<br><br><br>

## HTML 파싱 및 DOM (Document Object Model) 트리 생성

<img class='img-full' src='https://github.com/user-attachments/assets/947862a3-e148-4a26-98df-c7d4e0c7c92b' alt=''>
  
1. **변환 (Conversion)**  
브라우저는 HTML의 원시 바이트를 읽어와서,   
HTML 문서에 정의된 문자 인코딩(예: UTF-8)에 따라 개별 문자를 변환한다.  
이 과정에서 브라우저는 바이트 스트림을 이해할 수 있는 문자로 변환한다.  
2. **토큰화 (Tokenization)**  
변환된 문자열은 브라우저에 의해 W3C 표준에 지정된 고유 토큰으로 변환되는데,  
이 과정에서 HTML 태그, 속성, 텍스트 등 다양한 구성 요소를 식별해서 각각의 토큰으로 변환한다.  
만약 이미지와 같은 논 블로킹 자원을 발견하면, 브라우저는 해당 자원을 요청하고 토큰화를 계속 진행한다.  
3. **렉싱 (Lexing)**  
변환된 토큰은  객체 형태로 변환되는데  
이 과정에서 각 토큰은 태그의 이름, 속성, 속성 값 등이 함께 객체 형태로 변환된다.  
4. **DOM 생성 (DOM Construction)**  
변환된 객체들은 서로 부모-자식 관계를 파악해서 트리 구조로 연결된다.  
이 과정에서 각 객체들은 노드로 변환되고, 노드들이 연결되어 최종적으로 DOM 트리가 생성된다.  
만약 `<script>` 태그를 만나면 HTML 파싱을 중단하고 스크립트 코드를 실행하여   
브라우저는 스크립트 실행이 완료될 때까지 DOM 트리 생성을 중단하고, 렌더링을 진행하지 않는다.  
따라서, 스크립트 실행이 완료된 후에야 DOM 트리 생성이 재개된다.  
  
{:.notice--primary}
💡 **`프리로드 스캐너 (Preload Scanner)`**<br><br>
토큰화 과정에서 이미지와 같은 논 블로킹 자원을 발견하는 주체가 바로 프리로드 스캐너이다.  
프리로드 스캐너는  
HTML 코드에서 외부 자원 (CSS, JavaScript, 이미지 등)에 대한 참조를 미리 파악하고,   
발견된 자원을 서버에 요청하고 다운로드를 진행한다.  
이를 통해 파싱 과정에서 자원 다운로드 시간을 단축하고, 웹 페이지 로딩 속도를 향상시킬 수 있다.  
  
  
{:.notice--primary}
💡 **`왜 DOM 트리 생성을 중단하고 렌더링을 진행하지 않을까?`**<br><br>
스크립트가 DOM 트리를 조작하거나 CSSOM에 영향을 줄 수 있기 때문에  
자바스크립트 코드를 실행하기 위해 렌더링 엔진에서 자바스크립트 엔진으로 제어권을 넘긴다.  
따라서 `<script>` 태그가 있을 경우 렌더링이 차단 되는 것이다.  
<br>
<br>
`<script>` 태그에 `async` 또는 `defer` 속성을 추가하면 HTML 파싱을 중단하지 않고 스크립트 처리를 비동기적(병렬적으로)으로 처리하여 렌더링을 차단하지 않고 웹 페이지 로딩 속도를 향상시킬 수 있다.  
<br>
◾ `<script>` 태그에 `async` 속성이 있으면 스크립트 다운로드와 실행 모두 HTML 파싱과 병렬적으로 처리된다.  
<br>
    ```jsx
    <script async src="script.js"></script>
    ```
<br>
<br>
◾ `<script>` 태그에 `defer` 속성이 있으면 스크립트 다운로드는 HTML 파싱과 병렬적으로 처리되지만, 실행은 HTML 파싱이 완료된 후에 수행된다.  
<br>
    ```jsx
    <script defer src=”script.js”></script>
    ```
<br>

{:.notice--primary}
💡 **`<script> 태그의 위치`**<br><br>
브라우저는 HTML 문서를 위에서 아래로 순차적으로 파싱하면서  
`<script>` 태그를 만나면 파싱을 중단한다.  
즉, HTML 문서의 head 섹션에 스크립트가 있을 경우 블로킹 렌더링을 유발한다  
<br>
<br>
만약에 스크립트 파일을 먼저 로드하게 되는 경우를 생각해보자.  
스크립트가 DOM이 완전히 생성되기 전에 실행되면, 아직 존재하지 않는 요소를 조작하려고 할 때 오류가 발생할 수 있다.  
예를 들어, 스크립트에서 특정 요소를 찾으려고 할 때 해당 요소가 DOM에 없으면 `null`이 반환되어 에러가 날 수 있다.  
<br>
<br>
위 문제를 해결하기 위해 `<script>`를 body 태그 끝에 두면 된다.  
그럼 브라우저가 DOM과 CSSOM을 모두 생성한 다음에 스크립트를 실행하기 때문에,  
스타일이 적용된 상태에서 DOM 조작이 가능하다.  
<br>
<br>
마찬가지로 생각해보면  
페이지가 처음 로드되면 HTML과 CSS가 동시에 파싱되기 때문에  
CSS 파일을 최대한 빠르게 읽어와 렌더링을 완성하기 위해서는  
HTML 문서의 상단에 있는 head 섹션에 link 요소를 자식 요소로 하여 css 파일을 불러오는 것이 좋다는 것을 알 수 있다.

{:.notice--primary}
💡 **`DOM (Document Object Model)`**<br><br>
DOM은 문서의 구조를 표현하는 객체 모델로,  
DOM 은 HTML 문서의 내용을 트리형태로 구조화하여 웹페이지와 프로그래밍 언어를 연결시켜주는 역할을 한다.  
이때 각각의 요소와 속성, 콘텐츠를 표현하는 단위를 **노드(node)** 라고 한다.  
JavaScript와 같은 프로그래밍 언어는 DOM을 통해 HTML 요소를 조작할 수 있게 해준다.

<br><br><br>

## CSS 파싱 및 CSSOM (CSS Object Model) 트리 생성

> 브라우저가 CSS 파일을 다운로드하면  
> 브라우저는 CSS 파일을 위에서 아래로 읽어 내려가면서 파싱을 진행한다.  
> CSS 파싱 과정에서 브라우저는 CSS 규칙을 읽고,  
> 이를 노드로 변환하여 최종적으로 CSSOM 트리가 생성된다.  
> DOM 트리를 생성하는 과정과 동일한 과정으로 CSSOM 트리를 생성한다.

<img class='img-full' src='https://github.com/user-attachments/assets/c3383cd3-d1ec-443c-9ceb-cae7e66a36ed' alt=''>
  
<img class='img-full' src='https://github.com/user-attachments/assets/62b21ca2-f8d3-4f2a-80f5-5e90332fa011' alt=''>
  
<br><br><br>
## 렌더 (Render) 트리 생성
  
  
> DOM 트리와 CSSOM 트리가 만들어지면,   
이 둘을 결합해서 렌더링 트리를 생성한다  
렌더링 트리에는 페이지를 렌더링 하는데 필요한 노드만 포함된다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/2faac6ae-24c0-460a-82d9-38d4fce81751' alt=''>
  
<br><br><br>
## **레이아웃 (Layout) 단계**
  
  
> 레이아웃 단계에서는 뷰포트 안에서 각 요소의 정확한 위치와 크기를 결정하는 Box 모델이 생성된다.  
DOM 트리와 스타일 시트를 기반으로 어떤 요소를 어디에 렌더링 해야하는 지를 결정해야하기 때문에  
이 과정에서 모든 상대적인 측정값(%, em, rem 등)은 화면에서 절대적인 픽셀 값으로 변환된다.  
>
  
  
> 레이아웃 트리엔 페이지에 렌더링 되는 정보만 포함되기 때문에 **`display: none`** 으로 처리된 요소는 포함되지 않는다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/5863bd34-46c7-4013-8d1a-0ddfe5261c64' alt=''>
  
<br>
### **리플로우(Reflow)**
  
> 사용자가 웹 페이지에 처음 접속을 하면,   
렌더링 과정을 거쳐서 화면에 모든 요소가 그려지게 되는데  
이후에 사용자는 다양한 액션을 수행하게 되고,   
여기서 발생되는 이벤트로 인해서 새로운 HTML 요소가 추가되거나,   
기존 요소의 스타일이 바뀌거나 하는 변경이 일어나게 된다.  
이런 변경을 통해 영향을 받게되는 모든 노드에 대해서   
렌더링 트리 생성과 레이아웃 과정을 다시 수행하게 되는 과정을 리플로우(Reflow)라고 한다.  
>
  
- 기존 요소에 변경 사항이 생겼다고 해서 항상 리플로우(Reflow)-리페인트(Repaint)가 일어나는 것은 아니다.  
- 리플로우가 발생하면 레이아웃이 변경되고,   
그에 따라 화면에 표시되는 내용도 달라지기 때문에   
리플로우가 일어나면 반드시 리페인트가 일어나게 된다.  
- 리플로우가 일어나는 주요 속성들  
    - `position`  
    - `width`  
    - `height`  
    - `margin`  
    - `padding`  
    - `border`  
    - `border-width`  
    - `font-size`  
    - `font-weight`  
    - `line-height`  
    - `text-align`  
    - `overflow`  
  
<br><br><br>
## **페인트 (Paint) 단계**
  
  
> 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환하게 되고,  
레이아웃 단계에서 모든 계산이 완료가 되면,   
화면에 요소들을 그리게 된다.  
이미 레이아웃 단계에서 각 노드들이 위치, 크기, 색상 등 스타일이 모두 계산이 되었기 때문에   
화면에 실제 픽셀로 변환하게 되는 과정이다.  
>
  
  
<br>
### **리페인트(Repaint)**
  
> 리플로우는 단지 변경사항을 반영하기 위해서 렌더링 트리를 생성하고   
레이아웃 과정을 다시 수행하는 것이고,   
실제 이 결과를 화면에 그려지기 위해서는 다시 페인팅 단계를 수행해야 하는데  
이 과정을 리페인트(Repaint)라고 한다.  
>
  
- 레이아웃에 영향이 미치지 않는 단순한 색상 변경 같은 변경사항은 리플로우(Reflow)   
수행 없이 바로 리페인트(Repaint)만 수행하게 된다.  
- 리페인트가 일어나는 주요 속성들  
    - `background`  
    - `color`  
    - `text-decoration`  
    - `border-style`  
    - `border-radius`  
    - `visibility` : visibility 속성은 요소의 보이는 상태를 제어하는 속성이기때문에   
    visibility 속성을 변경하면 브라우저는 해당 요소의 스타일을 업데이트해야 하므로 리페인트가 발생한다.  
    그러나 visibility 속성이 요소의 크기나 위치와 같은 레이아웃에는 영향을 주지 않으므로, 리플로우는 발생하지 않는다.  
  
{:.notice--primary}
💡 **`리플로우와 리페인트의 발생조건은?`**<br><br>
◾ 리플로우는 DOM의 구조 변경으로 인해   
요소의 크기나 위치등의 변경으로 레이아웃에 영향을 미칠 때 발생합니다.  
◾ 리페인트는 레이아웃에 영향을 미치지 않고 요소의 색상 변경과 같은 스타일 변경이 일어날떄 발생한다.  
  
  
{:.notice--primary}
💡 **`리플로우와 리페인트 무엇이 더 무거운 작업일까?`**<br><br>
위에서 말했듯이,   
브라우저가 페이지의 각 요소의 크기나 위치등을 재계산하는 과정으로   
페이지의 레이아웃을 재생성하는 작업이이고  
리페인트는  이미 계산된 레이아웃에 스타일(색상, 그림자 등)을 적용해서 화면에 픽셀을 그리는 과정이다.  
리플로우는 레이아웃의 변경을 동반하므로 비용이 크지만,   
리페인트는 레이아웃의 변경 없이 스타일 변경만을 동반하기때문에 상대적으로 비용이 적다.  
  
  
<br><br><br>
## 합성 (Compositing) 단계
  
  
> 페인트 과정에서 생성된 여러 개의 레이어들은 하나의 화면으로 결합하는 컴포지팅(Compositing) 과정이 필요하다.  
브라우저는 각 레이어를 별도로 렌더링하고, 올바른 순서로 합성해 화면에 정확히 렌더링한다.  
특히 요소가 겹치는 경우, z-index나 position과 같은 속성을 고려해 겹치는 레이어들의 순서를 정확히 처리한다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/d0a418a2-9417-4181-b7b1-ff76c224ce0b' alt=''>
  
- **레이어의 생성**: 합성 과정에서 레이어는 다양한 이유로 생성될 수 있다.  
예를 들어, CSS 속성인 transform, opacity, filter 등이 적용된 요소는 별도의 레이어로 처리되는데 GPU 가속을 통해 성능을 향상시키기 위함이다.  
- **GPU 가속**: 합성 과정은 CPU 대신 GPU에서 수행될 수 있다.   
GPU는 병렬 처리에 강점을 가지고 있기 때문에 복잡한 그래픽 작업을 더 빠르게 처리해서 애니메이션이나 전환 효과가 자연스러울 수 있다.  
- **합성 순서**: 레이어가 겹치는 경우, z-index와 position 속성 외에도, DOM의 구조와 렌더링 순서에 따라 합성 순서가 결정된다.   
이 과정에서 브라우저는 각 레이어의 시각적 우선순위를 계산해서 최종적으로 화면에 표시할 순서를 정한다.  
- **디버깅 도구**: 브라우저의 개발자 도구를 사용하면 합성 레이어를 시각적으로 확인할 수 있는데 이를 통해서 레이어가 어떻게 구성되고 있는지 확인하고 성능 문제를 진단하는 데 도움이 된다.  
  
<br><br><br>
## **렌더링 성능 최적화 전략**
  
  
> 브라우저 렌더링의 이러한 과정은 사용자의 환경, 컴퓨터 사양, 브라우저 종류 등에 따라 속도가 다르게 나타날 수 있다.   
따라서 웹 개발자는 이러한 렌더링 과정을 최적화하여 사용자 경험을 개선할 필요가 있다.  
>
  
  
<br>
### **CSS와 JS 로드 방식 최적화**
  
> CSS와 JS 파일이 로드되고 실행되는 방식 또한 페이지 렌더링 성능에 중요한 영향을 미친다.  
CSS는 HTML 문서를 파싱하기 전에 가능한 한 빨리 로드되어야 하기 때문에  
CSS 파일은 rel=”stylesheet” 속성을 통해 <head> 태그 내부에 포함하고  
블로킹 렌더링을 유발하는 스크립트의 경우  
앞서 살펴본 async와 defer 속성을 적절히 활용해서  
CSS와 JS 파일 로드 방식을 최적화 할 수 있다.  
>
  
  
<br>
### **리플로우와 리페인트 줄이기**
  
> 리플로우와 리페인트는 브라우저 성능 최적화의 주요 대상이고  
이 두 작업이 빈번하게 발생하면 CPU와 GPU에 높은 부하를 주어 프레임 속도가 저하될 수 있다.  
>
  
- **DOM 변경 최소화**  
      
    ```jsx
    // 비효율적인 예시
    for (let i = 0; i < 1000; i++) {
      document.body.appendChild(document.createElement('div'));
    }
    
    // 최적화된 예시
    const fragment = document.createDocumentFragment();
    for (let i = 0; i < 1000; i++) {
      fragment.appendChild(document.createElement('div'));
    }
    document.body.appendChild(fragment);
    ```
      
- **CSS 클래스 사용**  
      
    ```jsx
    <div id="box" style="width: 100px; height: 100px; background-color: lightblue;"></div>
    
    <script>
      const box = document.getElementById("box");
      
      // 직접 스타일을 변경
      box.style.width = "200px"; // 크기 변경
      box.style.height = "200px"; // 크기 변경
    </script>
    
    // 대신 이렇게
    
    <div id="box" class="small"></div>
    <style>
      .small { width: 100px; height: 100px; background-color: lightblue; }
      .large { width: 200px; height: 200px; background-color: lightblue; }
    </style>
    
    <script>
      const box = document.getElementById("box");
      
      // 클래스 추가/제거로 크기 변경
      box.classList.remove("small");
      box.classList.add("large");
    </script>
    ```
      
- **레이아웃 트리거 속성 사용 지양**  
      
    ```jsx
    // 강제 리플로우를 유발하는 예시
    const height = element.clientHeight;
    element.style.width = `${height}px`;
    ```
      
- **Composite-Only 속성 사용**  
      
    ```css
    .element {
      transform: translateX(100px);
      opacity: 0.5;
    }
    ```
      
- **요청 애니메이션 프레임 사용**  
      
    ```jsx
    // 브라우저의 렌더링 주기에 맞춰 최적화
    function updateElement() {
      requestAnimationFrame(() => {
        element.style.transform = "translateX(100px)";
      });
    }
    ```
      
  
<br>
### **리소스 최적화**
  
> 웹 페이지에서 이미지는 시각적으로 중요한 요소지만, 파일 크기가 커 로딩 속도에 큰 영향을 미친다.   
특히 사용자가 즉시 보지 않는 이미지를 미리 로드하면,   
불필요한 네트워크 요청과 자원 소비로 인해 페이지 로딩 속도가 느려질 수 있다.  
>
  
- **파일 압축**: 서버에서 HTML, CSS, JavaScript 파일을 전송할 때 압축 알고리즘을 적용하면 파일 크기가 줄어 전송 속도가 빨라진다.  
- **이미지 최적화**: 이미지 포맷을 WebP나 AVIF와 같은 최신 포맷을 사용하면 동일한 화질에서 파일 크기를 줄일 수 있다. 이미지 크기를 줄이기 위해 다양한 이미지 최적화 도구를 통해 용량을 줄일 수 있다.  
- **Lazy Loading**: 스크롤할 때 필요한 이미지나 콘텐츠를 지연 로드하여 초기 시간을 줄일 수 있다.  
이미지에 `loading="lazy"` 속성을 사용하거나   
JavaScript 라이브러리를 활용하여 구현할 수 있다.  
      
    ```html
    <img **src**="example.jpg" **alt**="Example Image" **loading**="lazy">
    ```
      
- **Code Splitting**: JavaScript 번들을 여러 개로 나누어 필요한 부분만 로드하도록하여 페이지 로딩 속도를 최적화 할 수 있다.  
  
<br>
### **캐싱 전략**
  
> 캐싱은 웹 브라우저나 서버가 반복 요청 시 동일한 리소스를 다시 다운로드하지 않고 저장된 데이터를 재사용하는 기법으로  
캐싱을 적절히 설정하면 서버 부하가 줄어들고 응답 속도가 개선된다.  
>
  
- **서비스 워커 캐싱**: 서비스 워커는 브라우저가 백그라운드에서 작동하는 스크립트로, 네트워크 요청을 가로채고 캐시된 파일을 반환할 수 있다. 오프라인 모드에서도 웹 애플리케이션을 사용할 수 있도록 도와준다.  
- **CDN**: CDN은 여러 위치에 분산된 서버를 통해 사용자와 가까운 서버에서 리소스를 제공하여 로딩 속도를 단축시킬 수 있다.  
  
<br><br><br>
## **크로스 브라우징 사전 대응**
  
  
> 크로스 브라우징이란 웹 페이지 제작 시 모든 브라우저에서 깨지지 않고 의도한 대로 나오게 하는 작업을 말한다.  
브라우저마다 사용하는 렌더링 엔진이 모두 다르기 때문에  
각 렌더링 엔진에 따라 웹 페이지의 레이아웃이나 스타일이   
브라우저마다 다르게 나타날 수 있다.   
따라서 모든 브라우저에서 웹 피이지가 의도한 대로 나올 수 있도록 하는 작업이 필요하다.  
>
  
  
{:.notice--primary}
💡 **`크로스 브라우징에 대한 대응이 없다면?`**<br><br>
1)  **디자인 불일치**: 서로 다른 브라우저는 HTML, CSS, JavaScript를 다르게 해석할 수 있다. 이로 인해 특정 브라우저에서는 요소가 올바르게 정렬되지만 다른 브라우저에서는 엉망이 될 수 있다.  
2)  **기능적 오류**: JavaScript 코드가 특정 브라우저에서만 작동하거나, 특정 기능이 지원하지 않아서 사용자가 웹 페이지의 기능을 제대로 사용할 수 없게 될 수 있다.  
3)  **호환성 문제**: 최신 웹 기술(CSS Grid, Flexbox, ES6+)이 모든 브라우저에서 지원되지 않을 수 있다.  
4)  **SEO 문제**: 특정 브라우저에서 페이지가 제대로 표시되지 않으면 검색 엔진이 페이지를 올바르게 크롤링하지 못할 수 있다.  
  
  
<br>
### 코드 유효성 검사
  
> 잘못된 HTML 및 CSS 코드로 인해 웹사이트가 모든 브라우저에서 다양한 방식으로 오작동할 수 있다.   
이를 방지하는 방법은 코드의 유효성을 검사하는 것이다.   
W3C 유효성 검사기를 사용하여 HTML 및 CSS의 유효성을 검사 하거나   
IDE에 확장을 설치하여 코드를 작성할 때 검사 할 수 있다.  
>
  
  
- <a href='https://validator.w3.org/#validate_by_input+with_options' target='_blank'>https://validator.w3.org/#validate_by_input+with_options</a>
  
- <a href='https://jigsaw.w3.org/css-validator/#validate_by_input' target='_blank'>https://jigsaw.w3.org/css-validator/#validate_by_input</a>
  
<br>
### CSS 초기화
  
> 각각의 브라우저(크롬,IE,사파리 등등)마다 기본으로 제공하는 스타일이 있어서   
기본값을 설정을 해줄 수 있다.  
예를 들면 margin, padding, font등 입력 양식 등의 차이가 있거나   
기본 defalt값이 다르기 때문에 이러한 것들을 초기화 하면서   
브라우저마다 스타일의 차이를 줄이고자 CSS 초기화를 해줄 수 있다.  
>
  
  
<br>
### CSS 속성에 대한 지원 검토
  
> 다른 브라우저는 모두 CSS 속성을 동일하게 지원하지 않는다.  
한 브라우저에서 완벽하게 사용할 수 있는 것이 다른 브라우저에는 없을 수 있다.   
그런 문제를 해결하기 위해 CSS 브라우저 호환성을 확인할 수 있는 사이트를 통해 검토할 수 있다.  
>
  
  
- <a href='tps://caniuse.com/' target='_blank'>tps://caniuse.com/</a>
  
<br>
### **Vender Prefix**
  
> CSS 속성 앞에 브라우저 별 접두사를 붙이면   
특정 브라우저에서 적용할 CSS 속성을 지정할 수 있다.  
>
  
  
```css
.example {
    -webkit-transition: all 0.5s; /* Chrome, Safari */
    -moz-transition: all 0.5s;    /* Firefox */
    -o-transition: all 0.5s;      /* Opera */
    transition: all 0.5s;         /* 표준 */
}
```
  
<br>
### Polyfill 사용
  
> 구형 브라우저에서 지원하지 않는 JavaScript 기능을   
폴리필을 통해 구현하여 크로스 브라우징 문제를 해결할 수 있다.  
>
  
  
<br><br><br>
**reference**
  
  
- <a href='https://www.youtube.com/watch?v=z1Jj7Xg-TkU' target='_blank'>https://www.youtube.com/watch?v=z1Jj7Xg-TkU</a>
  
- <a href='https://www.youtube.com/watch?v=sJ14cWjrNis' target='_blank'>https://www.youtube.com/watch?v=sJ14cWjrNis</a>
  
- <a href='https://www.youtube.com/watch?v=kP-H1GXD_nI' target='_blank'>https://www.youtube.com/watch?v=kP-H1GXD_nI</a>
  
- <a href='https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work' target='_blank'>https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work</a>
  
- <a href='https://so-so.dev/web/browser-rendering-process/' target='_blank'>https://so-so.dev/web/browser-rendering-process/</a>
  
- <a href='https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce' target='_blank'>https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce</a>
  
- <a href='https://velog.io/@hyebinee/%ED%81%AC%EB%A1%9C%EC%8A%A4%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A7%95cross-browsing%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1%EA%B3%BC-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95' target='_blank'>https://velog.io/@hyebinee/%ED%81%AC%EB%A1%9C%EC%8A%A4%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A7%95cross-browsing%EC%9D%98-%EC%A4%91%EC%9A%94%EC%84%B1%EA%B3%BC-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95</a>
