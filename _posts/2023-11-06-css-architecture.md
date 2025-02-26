---
title: 'CSS Architecture'
excerpt_separator: '<!--more-->'
categories:
  - HTML CSS
tags:
  -
header:
  teaser: ./assets/image/40.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# CSS (Cascading)

<img class='img-full' src='https://github.com/user-attachments/assets/bd1afa75-1966-4a19-a247-3163cb0ee07f' alt=''>
  
> 웹 페이지의 스타일과 레이아웃을 정의하는 데 사용되는 스타일 시트 언어이다.  
웹은 문서를 공유하기 위해서 만들어졌고  
그 문서를 조금 더 잘 보여주기 위해서 서식이 만들어졌다.  
이러한 서식과 데이터가 복잡하게 공존하게 되면서  
서식과 컨텐츠를 분리해야겠다는 의도에서 CSS가 탄생하게 되었다  
>
  
  
> Cascading은 계단식, 위아래로 흐른다는 의미로  
CSS에서 스타일이 적용되는 방식과 관련이 있고  
Style Sheet는 글꼴의 크기, 모양, 컬러, 문단 설정 등을 미리 정의해 스타일로 만들었다가  
웹 문서의 본문에서 그 스타일을 참조해서 사용하는 기능을 말한다.  
>
  
  
<br><br>
## CSS 우선순위
  
  
> 앞서 웹은 문서를 공유하기 위해서 만들어졌고  
그 문서를 조금 더 잘 보여주기 위해서 서식이 만들어졌다고 했다.  
서식이 만들어진 초기에는  
태그에 직접 스타일을 입력하는 방식으로  
즉 inline-style로 스타일을 적용해야했기 때문에  
동일한 스타일이 반복적으로 작성되면서  
복잡한 문서가 만들어지는 문제가 있었다.  
>
  
  
```html
<strong style="color:red; text-decoration:underline">hello world</strong>
```
  
> 그래서 이 서식과 컨텐츠를 분리해야겠다는 의도에서 CSS가 탄생하게 되었는데  
문서에서 원하는 태그를 선택하는 `선택자(selector)`를 사용하여  
해당 선택자에 스타일을 적용할 수 있도록 하는 CSS Rule이라는 구조가 생기면서  
반복적으로 스타일을 작성했던 부분들이 일괄적으로 적용시킬 수 있게 되었다.  
이때, 각 선택자에 대한 스타일은 `선언(declaration)`을 통해 정의된다.  
>
  
  
> 이때 CSS의 Rule의 적용 대상과 서식이 겹칠 경우에는  
어떤 서식을 더 우선을 해야할지 미리 정해진 규칙이 있는데  
고유의 `명시도(Specificity)`에 따라서  
우선순위가 다르게 적용을 할 수 있도록 규칙이 설계 되어있다.  
이 규칙에 따라 css 우선순위가 달라지게 된다.  
>
  
  
> 각 CSS 선택자에 따라 점수를 가지고 있는데  
이 점수가 높으면 높을 수록 선언의 우선순위가 높아진다.  
만약 선언들 끼리 점수가 같다면 나중에 작서오딘 선언의 우선순위가 높아진다.  
>
  
  
1. important (9999999999점)  
2. Inline style 선언 (1000점)  
3. ID 선택자 (100점)  
4. Class 선택자 (10점)  
5. Tag 선택자 (1점)  
6. (전체) 선택자 (0점)  
7. 상속 => 상속이 가능하지만 별도로 점수를 계산하지 않음  
  
```css
list li.item {
}

/_ Class + Tag + Class _/
/_ 10+ 1+ 10 = 21 점 _/

list li:hover {
}

/_ Class + Tag + Class _/
/_ 10+ 1+ 10 = 21 점 _/

.box: :before {
}

/_ Class + Tag _/
/_ 10+ 1= 11 점 _/

````

<br><br>
## User-Agent StyleSheet


> 각 브라우저마다 각각의 요소에게 지정해놓은 스타일리스트가 존재하는데
우리는 이러한 스타일을 User-agent stylesheet 라고 부른다.
User-Agent StyleSheet는 각 브라우저만의 개성이자 표준이 없던 관계로
브라우저마다 서로 다른 서식을 가지게 된 것이다.
특히 IE가 주류이던 그 시절은 더 심했다.
>


{:.notice--primary}
💡 **`User-agent stylesheet !== initial value`**<br><br>
User-agent stylesheet가
초기값, 즉 initial value라고 오해할 수도 있는데
User-agent stylesheet 와 initial value 는 다른 것이다.


<br><br>
## 크로스 브라우징 (Cross Browsing)


> 각각의 브라우저(크롬,IE,사파리 등등)마다 기본으로 제공하는 스타일이 있어서
기본값을 설정을 해줄 수 있다.
예를 들면 margin, padding, font등 입력 양식 등의 차이가 있거나
기본 default값이 다르기 때문에 이러한 것들을 초기화 하면서
브라우저마다 스타일의 차이를 줄이고자 CSS 초기화를 해줄 수 있다.
>


> CSS 초기화를 통해서
브라우저에 구애받지 않고 최대한 동일한 사용자 경험을 제공할 수 있는데
이러한 작업이 크로스 브라우징인 것이다.
CSS 초기화 하기 위해서사용하는 것이 바로 CSS Normalize와 CSS Reset 이다.
>


<br>
### CSS Reset


> CSS Reset은
CSS Reset의 목적은 모든 태그에 적용된 서식에 margin과 padding을 제거하는 방식으로
브라우저의 기본요소의 디자인을 모두 없애자는 것이다.
>


```css
* {margin: 0; padding: 0}
````

> 즉, 스타일 거의 모두를 0으로 만드는 방법을 통해서  
> 브라우저들의 서식을 하나로 통일을 하는 것이다.  
> 거의 모든 스타일을 초기화하는 만큼  
> 일부 유용한 기본 스타일도 초기화 될 수 있고,  
> 스타일을 직접 정의해야하고 CSS 코드가 길어질 수 있는 단점이 있다.

{:.notice--primary}
💡 **`여러 버전의 Reset`**<br><br>
당시 브라우저에서 `* {...} Selector`는 모든 요소에 적용되기 때문에  
성능과 출력의 이슈가 있을 수 있어서  
조금씩 정교하게 CSS를 만들어가면서 여러가지 버전의 Reset이 만들어졌다.  
그중 에릭마이어의 CSS Reset이 가장 유명한 CSS가 되었고  
10년도 넘은 지금도 CSS Reset를 검색하면 최상위로 나타나는 자료로 아직까지도 쓰이고있다.  
브라우저 간 호환성이 좋아지면서  
불필요한 부분을 줄이고 최신 웹 개발 트랜드를 반영한  
엘라드 세흐터의 CSS Reset이 인기를 끌고 있다.

<br>
### CSS Normalize
  
  
> 브러우저 간 스타일 차이를 최소화 하는 것을 목표로  
Reset CSS와 달리 유용한 기본 스타일은 유지하면서 브라우저별 차이만 조정하는 방식이다.  
부분적으로 개선하는 방식이기 때문에 Reset CSS보다 코드량이적고 성능 면에서 유리하다는 장점이 있지만  
Reset CSS만큼 완벽하게 브라우저 간 스타일 차이를 없애지는 못한다는 단점이 존재한다.  
>
  
  
{:.notice--primary}
💡 **`Normalize + New Default Style`**<br><br>
Normalize는 브라우저 간의 스타일을 통일하는 데 도움을 주지만,  
특정 스타일 속성들이 과연 필요한지에 대한 의문이 들 수 있다.  
이러한 의문을 바탕으로 Normalize 방식과 New Default Style을 합쳐서 만든  
sanitize.css와 같은 라이브러리가 등장하기도하고,  
Reboot.css와 minireset.css와 같은 다른 스타일 리셋 방법들도 등장하기도 했다.

{:.notice--primary}
💡 **`실전에서는 어떻게 쓰이고있나?`**<br><br>
애플, 구글, 네이버, 카카오는 Normalize보다는 Reset이지만 일부만 사용하고 있는 형태를 사용하고 있다.  
대규모 서비스들이 Normalize + New Default Style 형태를 사용하고 있는만큼  
Normalize + New Default Style 형태가 좋을 수도 있지만  
프로젝트 상황과 여건에 맞춰 재정의할 필요가 있는 속성이 무엇인지 파악하고,  
이를 따로 파일로 구성한다면 그것이 최적의 reset CSS, normalize CSS가 될 수 있지않을까?

<br><br>

## SASS(SCSS) vs. Styled Component vs TailwindCSS (장단점)

> 크로스브라우징을 위해 CSS Reset 을 했다면  
> CSS 코드를 더 유지보수 할 수 있는 CSS 라이브러리가 등장하기 시작했다.

<br>
### SCSS
  
<img class='img-full' src='https://github.com/user-attachments/assets/0cea8b63-8a45-4f09-b091-3e2ce73e734c' alt=''>
  
> 가장 기본적인 CSS 라이브러리인 SCSS는  
CSS의 문법을 확장하여 변수, 중첩, Mixin 등 강력한 기능을 제공한다.  
마치 JavaScript처럼 CSS를 작성할 수 있게 되어 코드의 재사용성과 유지보수성이 크게 향상된다.  
SCSS 코드는 브라우저에서 바로 해석되지 않으므로,  
CSS로 컴파일하는 과정이 필요하다.  
>
  
  
```css
/* before */
.container {
  width: 800px;
  margin: 0 auto;
}

/_ after _/
$base-width: 800px;

.container {
width: $base-width;
margin: 0 auto;
}

````

```css
/* before */
.nav ul li a {
  color: white;
  text-decoration: none;
}

/* after */
.nav {
  ul {
    li {
      a {
        color: white;
        text-decoration: none;
      }
    }
  }
}
````

<br>
### Styled Components
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/36f31f34-e546-4485-b32f-dd20fbe68287' alt=''>
  
> Styled Components는 CSS-in-JS 라이브러리로,  
JavaScript 컴포넌트 내부에 CSS를 직접 작성하여 스타일과 컴포넌트를 함께 관리할 수 있도록 도와준다.  
>
  
- **동적 스타일링**: JavaScript 변수와 로직을 사용하여 스타일을 동적으로 변경할 수 있다.  
- **런타임 오버헤드**: 스타일 계산이 런타임에 이루어지므로 초기 로딩속도에 영향을 줄 수 있다.  
- **App Router 미지원**: next app router를 현재 지원하지 않기 때문에 라우팅과 관련된 스타일링이 제대로 작동하지 않을 수 있다.  
  
```jsx
import styled from 'styled-components';

const Button = styled.button`
background-color: #4CAF50; /_ Green _/
border: none;
color: white;
padding: 15px 32px;
text-align: center;
text-decoration: none;
display: inline-block;
font-size: 16px;
margin: 4px 2px;
cursor: pointer;

&:hover {
background-color: #3e8e41;
}
`;

function App() {
return (

<div>
<Button>Styled Components 버튼</Button>
</div>
);
}

````

<br>
### Tailwind CSS

<img class='img-full' src='https://github.com/user-attachments/assets/9cf5192f-ff90-4bcd-8c75-b7704881d009' alt=''>

> Tailwind CSS는 유틸리티 클래스 기반으로,
미리 정의된 클래스를 조합하여 스타일을 적용할 수 있는 CSS 프레임워크이다.
>

- **빠른 개발 속도**: 미리 정의된 클래스를 사용하여 스타일을 빠르게 적용할 수 있다.
- **반응형 디자인**: 반응형 디자인을 위한 유틸리티 클래스를 제공해서 다양한 화면 크기에 대응하기 쉽다.
- **HTML 마크업 복잡성 증가**: 많은 클래스를 사용하면 HTML 마크업이 길어지고 복잡해질 수 있다.
- **방대한 유틸리티 클래스**: Tailwind CSS의 방대한 유틸리티 클래스에 익숙해지는데 시간이 필요하다.

```html
<div class="container mx-auto bg-gray-100 p-4 rounded-md sm:px-6 md:max-w-2xl lg:max-w-4xl">
  <h1 class="text-2xl font-bold mb-2">Tailwind CSS 예시</h1>
  <p class="text-gray-700">
    hello world
  </p>
  <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
    버튼
  </button>
</div>
````

<br>
### 정리하자면
  
  
> SCSS는 CSS 전처리기로, 변수, 중첩, 믹스인 등 강력한 기능을 사용하여 CSS를 더욱 효율적으로 작성하고 관리할 수 있다.  
>
  
  
> Tailwind CSS는 빠른 스타일링과 일관된 디자인 시스템 구축을 우선시하는 경우에 좋은 선택지가 될 수 있다.  
>
  
  
> Styled Components와 Emotion은 컴포넌트와 스타일을 긴밀하게 연결하고 동적인 스타일링을 원하는 React 프로젝트에 이상적이다.  
프로젝트의 규모, 복잡도, 팀의 숙련도, 개발 방식 등을 종합적으로 고려해서 선택하는 것이 좋고  
>
  
  
> 나의 경우에는  
styled-components 라이브러리가 스타일과 로직을 함께 관리할 수 있어서  
가독성 측면에서 장점이 있지만,  
개인적으로는 스크롤을 통해 코드베이스를 자주 오가야 하는 점이 비효율적이라고 느껴져서  
미리 정의된 클래스를 사용하여 빠르게 스타일을 적용시킬 수 있는 Tailwind CSS를 더 선호하는 편이다.  
>
  
  
{:.notice--primary}
💡 **`shadcn/ui`** → https://ui.shadcn.com/<br><br>
프로젝트에서 실제로 써본 도구인데  
개인적으로 Tailwind CSS를 더 선호하는 입장에서 너무 유용하다는 생각이 들었다.  
shadcn/ui는 Vercel의 shadcn이 만든 UI 컴포넌트 라이브러리로  
Radix UI와 Tailwind CSS라는 기술을 기반으로 편리하고 다양한 컴포넌트를 제공한다.  
shadcn/ui는 다양한 UI 컴포넌트를 모듈화해서 제공하기 때문에  
필요에 따라 원하는 컴포넌트만 선택하여 사용할 수도 있고 커스터마이징 할 수도 있다.

<br><br>

## CSS Modules vs CSS Variables vs CSS in JS

<br>
### CSS Modules
  
  
> CSS 파일의 스코프를 컴포넌트 단위로 제한하여 스타일 충돌을 방지하는 기술로  
CSS 클래스 이름을 자동으로 고유하게 생성하여 다른 컴포넌트의 스타일과 충돌하지 않도록 한다.  
>
  
- **스타일 충돌 방지**: 클래스 이름이 고유하게 생성되므로, 다른 컴포넌트의 스타일과 충돌할 위험이 없다.  
- **코드 재사용성**: 컴포넌트 단위로 스타일을 관리하여 코드 재사용성을 높다.  
- **유지보수 용이성**: 스타일이 컴포넌트와 함께 관리되므로, 유지보수가 용이하다.  
- **번거로움**: CSS Module은 모듈마다 별도의 CSS 파일을 작성해야 하기 때문에 별도로 많은 CSS 파일을 만들어 관리해야 한다.  
- **클래스 이름 복잡성**: 클래스를 동적으로 추가할 경우 최종 렌더링된 결과물에서 자동 변환된 클래스명이 코드의 가독성을 어지럽히는 경우가 종종 발생할 수 있다.  
  
```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
}
```
  
```jsx
// Button.js
import styles from './Button.module.css';

function Button() {
return (
<button className={styles.button}>Click me</button>
);
}

````

<br>
### CSS Variables


> CSS Variables는 CSS 내에서 변수를 선언하고 값을 할당하여
스타일을 재사용하고 동적으로 변경할 수 있도록 도와주는 Custom Properties 이다.
>

- **코드 재사용성**: 공통 스타일을 변수로 정의하여 재사용할 수 있다.
- **유지보수 용이성**: 변수 값만 변경하면 해당 변수를 사용하는 모든 요소의 스타일이 한 번에 변경된다.
- **동적 스타일링**: JavaScript를 사용하여 변수 값을 동적으로 변경할 수 있다.
- **브라우저 호환성**: 구형 브라우저에서는 지원되지 않을 수 있다.
- **스코프 제한**: CSS 변수는 전역 스코프를 가지므로, 변수 이름 충돌에 주의해야 한다.

```css
:root {
  --primary-color: blue;
}

.button {
  background-color: var(--primary-color);
  color: white;
}
````

<br>
### CSS in JS
  
  
> 앞서 Styled Components 에서 언급한 기술로  
JavaScript 코드 내에서 CSS를 작성하고 관리하는 기술이다.  
>
  
- **유지보수 용이**: 컴포넌트 단위로 스타일을 적용하기 때문에, 컴포넌트 모듈 하나에서 스타일과 동작을 한번에 관리할 수 있기 때문에 스타일을 고치기 위해 불필요하게 프로젝트 폴더에서 왔다 갔다 하는 일을 줄일 수 있다.  
- **스타일 충돌 방지**: CSS Modules을 사용하지않아도 컴포넌트 자체에 스타일을 지정하는 방식으로 충돌을 쉽게 막을 수 있다.  
- **스타일 상속의 용이**: 공통 속성은 그대로 가져가고, 특정 스타일만 다르게 주고 싶을 때 상속을 이용하면 쉽게 스타일을 적용할 수 있다.  
- **런타임 오버헤드**: 스타일 계산이 런타임에 이루어지므로 초기 로딩속도에 영향을 줄 수 있다.  
- **App Router 미지원**: next app router를 현재 지원하지 않기 때문에 라우팅과 관련된 스타일링이 제대로 작동하지 않을 수 있다.  
  
<br><br>
**reference**
  
  
- <a href='https://velog.io/@teo/2022-CSS-Reset-%EB%8B%A4%EC%8B%9C-%EC%8D%A8%EB%B3%B4%EA%B8%B0' target='_blank'>https://velog.io/@teo/2022-CSS-Reset-%EB%8B%A4%EC%8B%9C-%EC%8D%A8%EB%B3%B4%EA%B8%B0</a>
  
- <a href='https://www.daleseo.com/css-normalize-reset/' target='_blank'>https://www.daleseo.com/css-normalize-reset/</a>
  
- <a href='https://autumnly1007.tistory.com/16' target='_blank'>https://autumnly1007.tistory.com/16</a>
  
- <a href='https://www.devdive.co.kr/react/css-library-comparison' target='_blank'>https://www.devdive.co.kr/react/css-library-comparison</a>
  
- <a href='https://dev.to/alexsergey/css-modules-vs-css-in-js-who-wins-3n25' target='_blank'>https://dev.to/alexsergey/css-modules-vs-css-in-js-who-wins-3n25</a>
