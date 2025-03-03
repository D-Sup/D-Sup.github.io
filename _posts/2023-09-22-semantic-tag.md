---
title: 'Semantic Tag'
excerpt_separator: '<!--more-->'
categories:
  - HTML CSS
tags:
  -
header:
  teaser: ./assets/image/38.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->


# 시맨틱 태그

_Semantic Tag_

> HTML5에서 도입된 **의미를 부여한 태그**로,   
> 웹 페이지의 구조와 콘텐츠를 설명하는 역할을 한다  
> 시맨틱 태그는 요소의 역할과 의미를 더 잘 나타내므로  
> 이러한 태그를 사용하면 웹 페이지의 내용을 더 명확하게 정의하고,   
> **코드의 가독성**, **검색 엔진 최적화(SEO)**, **웹 접근성**을 향상시킬 수 있다

<img class='img' src='https://github.com/FRONTENDSCHOOL5/final-02-Ta-dak_Ta-dak/assets/96939334/5ad5377c-e415-4f3d-a5ed-cd55f62be972' alt=''>

💡 semantic 태그가 나오기 이전에는   
&emsp;`<div>` 태그로 일일히 위치 범위를 지정하고   
&emsp;각 태그의 class 명으로 이 요소의 역할을 명시해야 했지만,   
&emsp;semantic 태그를 이용하면 태그 이름에서 **해당 엘리먼트의 위치와 역할이 무엇인지**   
&emsp;단번에 알 수 있기 때문에 좀 더 모던하다고 할 수 있다
{:.notice--primary}

<br>

# 시맨틱 태그 구성 요소

| 태그 | 의미 |
| --- | --- |
| header | 페이지의 머리부분을 나타내는 영역 |
| nav |	페이지의 내비게이션 영역 |
| main | 메인 콘텐츠를 나타내는 영역 |
| section | 제목별로 나눌 수 있는 문서의 콘텐츠 영역을 구성하는 요소 |
| article |	개별 콘텐츠를 나타내는 요소 |
| aside	| 좌우측의 사이드 영역 |
| footer	| 페이지의 바닥부분을 나타내는 영역 |

<br>

## 더 살펴보기 

### header

> 페이지 또는 특정 섹션의 헤더를 정의할 때 사용한다  
> 보통 페이지의 머리글, 제목, 로고, 메뉴, 검색 관련, 유틸, 작성자의 이름 등과 같은 정보를 포함하게 된다

```html
<header>
  <!-- 제목 요소 (h1 ~ h6 중 하나를 선택) -->
  <h1>웹 페이지 제목</h1>
  <!-- 로고 또는 아이콘 -->
  <img src="logo.png" alt="로고 이미지">
  <!-- 저자 정보 -->
  <p>저자: John Doe</p>
</header>
```

<br>

### nav

> 네비게이션 표현을 위해 동일한 페이지 안의 문서나  
> 다른 페이지의 문서로 연결하는 링크들의 모음을 나타내는 용도로,  
> 독립적으로 사용이가능하고 보통 메뉴, 목차, 인덱스로 사용이 된다

```html
<nav>
  <ul>
    <li><a href="#">홈</a></li>
    <li><a href="#">서비스</a></li>
    <li><a href="#">포트폴리오</a></li>
    <li><a href="#">연락하기</a></li>
  </ul>
</nav>
```

💡 위치에 영향을 받지 않기 때문에 `<header>`나 `<footer>`,  
&emsp;또는 `<aside>` 에 포함시키거나 따로 사용할 수 있다  
&emsp;글을 읽지 못하는 사용자를 위한 스크린 리더기와 같은 브라우저는   
&emsp;`<nav>` 요소를 사용하여 해당 콘텐츠의 초기 렌더링을 생략할지 여부를 결정하기도 한다
{:.notice--primary}

<br>

### main

> 웹 페이지의 주요 콘텐츠를 정의할 때 사용한다  
> 해당 문서의 중심 주제 또는 주요 기능과 직접적으로 관련되어 있거나 확장되는 콘텐츠로 구성되어야 한다  
> 웹 페이지 내에서 한 번만 사용되어야 한다   
> 다른 `<header>`, `<footer>`, `<nav>`, `<article>`, `<section>`, `<aside>`의 하위로 작성할 수 없다

```html
<main>
  <h2>주요 콘텐츠 제목</h2>
  <p>웹 페이지의 중심 주제나 주요 기능과 관련된 내용을 적는다.</p>
  <p>이 콘텐츠는 웹 페이지의 핵심이다.</p>
</main>
```

<br>

### section

> HTML 문서에서 독립적인 섹션을 정의하는 용도로,  
> 맥락에 따라 주제별로 컨텐츠를 묶을 때 사용한다  
> 각 `<section>` 요소는 특정 주제, 카테고리, 또는 콘텐츠 영역을 나타낸다  
> `<section>` 요소는 보통 섹션 제목을 나타내는 `<h1>` ~ `<h6>` 태그들이 포함되는 경우가 많다  
> `<article>` 태그와는 달리 `<section>` 태그는 재사용할 수 없는 콘텐츠로 인식한다

<br>

### article
	
> 해당 문서나 웹 페이지 또는 사이트와는 완전히 독립적으로 구성할 수 있는 요소를 정의하는 용도로,  
> 보통 뉴스 기사, 블로그 포스트 등에 사용된다  
> 태그를 적용한 부분을 떼어내 독립적으로 재사용하더라도 완전히 하나의 콘텐츠가 된다면 `<article>` 를 사용하면 된다

```html
<article>
  <h1>서울의 날씨 정보</h1>
  <p>서울 날씨 : 맑음</p>
  <p>하루 종일 맑은 날씨가 예상되며, 바람은 약간 불고 있습니다.</p>
  <p>날씨 업데이트: 2023년 10월 07일</p>
</article>
```

<br>

### aside

> 본문이 아닌 왼쪽이나 오른쪽에 부수적인 내용이 들어가는 부분을 표현하는데 사용한다  
> 예를 들어, 블로그 양 옆의 광고나 링크 같은 사이드 바를 표시할 때 사용할 수 있다  
> 필수요소가 아니기 때문에 필요한 경우에만 사용한다

```html
<aside>
  <h2>사이드 바</h2>
  <ul>
    <li><a href="#">광고 1</a></li>
    <li><a href="#">광고 2</a></li>
    <li><a href="#">광고 3</a></li>
  </ul>
</aside>
```

<br>

### footer

> 문서나 특정 섹션의 푸터(footer)를 정의할 때 사용한다  
> `<footer>` 안에  `<section>`, `<article>` 등 다른 레이아웃 태그들을 모두 사용할 수 있고   
> 이런 태그들을 활용하여 `<footer>` 안에 저자정보, 저작권 정보, 연락처, 사이트맵 연관 페이지 등 다양한 정보들을 넣을 수 있다

```html
<footer>
  <p>저자: Dong Sup</p>
  <p>&copy; 2023. 모든 권리 보유.</p>
  <p>이메일: DongSup@example.com</p>
</footer>
```

<br>
<br>

## 시맨틱 태그의 장점

**코드 가독성 향상**   
- HTML 코드가 더 의미적이고 읽기 쉬워지므로, 코드의 가독성이 향상되어 협업과 유지보수가 더 쉬워진다  

**SEO 개선**   
- 검색 엔진 최적화를 위해 웹 페이지 구조와 콘텐츠를 명확하게 정의할 수 있으므로  
  검색 엔진은 웹 페이지를 더 잘 이해하고 색인화할 수 있게되어 검색 엔진 순위가 향상된다  

**웹 접근성 향상**   
- 스크린 리더는 의미적인 태그를 해석해서 사용자에게 콘텐츠를 읽어주므로  
  스크린 리더와 같은 보조 기술을 활용하는 사용자들이 웹 페이지를 더 쉽게 이해하고 활용할 수 있다  
  이로 인해 웹 접근성이 향상된다  

**유지보수 용이성**  
- 의미적인 마크업을 사용하면 특정 부분을 찾고 수정하기가 더 쉽기때문에  
  레이아웃을 변경하거나 스타일을 업데이트할 때 수월하다   
  즉, 웹 사이트의 유지보수 비용을 절감하는 데 도움이 된다

<br>
<br>

## 시맨틱 태그의 부재

**웹 접근성 저하**
> React를 사용하면 컴포넌트 기반의 개발을 할 수 있다  
> 이때 시맨틱 태그보다는 컴포넌트가 중심이 되면, 시맨틱 태그가 무시되거나 부적절하게 사용될 수 있다  
> 이로인해 웹 접근성에 문제를 일으킬 수 있다

<img class='img' src='https://github.com/FRONTENDSCHOOL5/final-02-Ta-dak_Ta-dak/assets/96939334/c9f4357f-4a5c-4f4f-8dce-3e4035a6842b' alt=''>

<br>

**의미 전달의 모호성**   
> 시맨틱 태그를 사용하지 않으면 HTML 요소가 단순한 `<div>`나 `<span>`으로만 정의되어,   
> 각 요소의 역할과 의미를 명확하게 전달하기 어렵다  
> 이로 인해 웹 페이지의 구조를 이해하기 어렵고, 코드를 작성하거나 유지보수하기 어려워진다

```html
<div id="news">
  <div class="section-title">최신 뉴스</div>
  <div class="article">
    <div class="article-title">기사 제목 1</div>
    <div class="article-content">기사 내용 1</div>
  </div>
</div>
```
```html
<section>
  <h2>최신 뉴스</h2>
  <article>
    <h3>기사 제목 1</h3>
    <p>기사 내용 1</p>
  </article>
</section>
```

<br>

**검색 엔진 최적화 (SEO) 문제**   
> 검색 엔진은 웹 페이지의 구조와 콘텐츠를 이해하고 인덱싱하는 데 시맨틱 태그가 중요한 역할을 한다   
> 시맨틱 태그를 사용하지 않으면 검색 엔진이 웹 페이지를 제대로 이해하지 못하고,   
> 검색 결과에서 높은 순위를 얻기 어려울 수 있다

<br>

# 질문

<details>
  <summary>✨ 시맨틱 태그란 무엇인가요?</summary><br>
  HTML5에서 도입된 의미를 부여한 태그로, <br>
  웹 페이지의 구조와 콘텐츠를 설명하는 역할을 합니다.<br>
  시맨틱 태그는 요소의 역할과 의미를 더 잘 나타내므로<br>
  이러한 태그를 사용하면 웹 페이지의 내용을 더 명확하게 정의하고, <br>
  코드의 가독성, 검색 엔진 최적화(SEO), 웹 접근성을 향상시킬 수 있습니다.
</details>

<br>

<details>
  <summary>✨ 시맨틱 HTML5 요소는 무엇이 있고, 각각 어디에 사용되나요?</summary><br>
  HTML5에서 도입된 시맨틱 요소는 header, main, footer, section, article, nav, aside 가 있습니다<br>
  header 는 로고, 제목, 검색 폼 등과 같이 웹 페이지의 머리말을 나타내며데 사용합니다.<br>
  main 은 웹 페이지의 주요 콘텐츠를 감싸는 요소로, 웹 페이지의 주요 콘텐츠를 정의할 때 사용합니다.<br>
  footer 는  저작권 정보, 연락처 등과 같이 웹 페이지의 바닥글을 나타내는데 사용합니다.<br>
  section 은 콘텐츠를 그룹화하여 나타내는 요소로, 일반적으로 헤딩태그와 함께 사용합니다.<br>
  article 은 블로그 글, 뉴스 기사 등과 같이 독립적인 콘텐츠 조각을 나타내는데 사용합니다.<br>
  nav 은 네비게이션 링크 그룹을 나타내며, 주 메뉴, 사이드바 링크 등에 사용합니다.<br>
  aside 는 별도의 사이드바 영역을 나타내며, 광고 같은 부수적인 내용을 표현하는데 사용합니다.
</details>

<br>

<details>
  <summary>✨ 시맨틱 태그를 사용하면 웹 개발이나 유지보수에 어떻게 도움이 될까요?</summary><br>
  시맨틱 태그를 사용하면 웹 개발과 유지보수가 용이해집니다. <br>
  먼저, 웹 페이지의 구조가 명확하게 정의되므로 개발자들은 각 요소의 역할을 더 쉽게 이해할 수 있습니다. <br>
  이로써 협업과 코드 리뷰가 원활해지며, 개발 과정에서 오류를 예방할 수 있습니다.<br>
  또한, 시맨틱 태그를 사용하면 스타일과 구조가 분리되어 디자인 변경이나 업데이트가 간편해집니다. <br>
  웹 접근성 측면에서도 시맨틱 태그를 사용하면 스크린 리더 및 보조 기술을 <br>
  사용하는 사용자에게 웹 페이지를 더 쉽게 이해할 수 있도록 도와줍니다. <br>
  마지막으로, 검색 엔진 최적화 (SEO)에도 도움이 되어 <br>
  검색 결과에서 웹 사이트를 높은 순위로 노출시킬 수 있습니다.
</details>

<br>

<details>
  <summary>✨ 시맨틱 태그를 사용하지 않고 웹 개발을 하는 경우 어떤 문제가 발생할 수 있나요?</summary><br>
  React 를 사용할 때, 컴포넌트 기반의 개발을 하게되면 시맨틱 태그를 무시하거나 <br>
  부적절하게 사용해서 웹 접근성에 영향을 미칠 수 있습니다. <br>
  이는 시각이나 신체적 제한이 있는 사용자가 웹 콘텐츠를 이용하기 어렵게 만들 수 있습니다. <br>
  더불어, 시맨틱 태그를 사용하지 않으면 HTML 요소의 역할과 <br>
  의미를 명확하게 전달하기 어려워 웹 페이지의 구조를 파악하기 어렵고, <br>
  코드를 작성하거나 유지보수하기 어려워질 수 있습니다. <br>
  또한, 검색 엔진은 시맨틱 태그를 활용하여 웹 페이지를 이해하며, <br>
  시맨틱 태그를 사용하지 않으면 검색 엔진에서 웹 페이지를 제대로 인식하지 못할 수 있습니다. <br>
  이로 인해 검색 결과에서 상위 랭크를 얻기 어려울 수 있습니다.
</details>

<br>
<br>

<span class='explain'>Reference</span> 

- [Inpa Dev](https://inpa.tistory.com/entry/HTML-%F0%9F%93%9A-%EC%8B%9C%EB%A7%A8%ED%8B%B1-%ED%83%9C%EA%B7%B8-%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EC%9D%84-%EC%A7%80%EC%A0%95)
