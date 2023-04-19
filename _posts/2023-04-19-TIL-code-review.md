---
title: '랜딩 페이지 제작과 반응형 구현 && 코드리뷰'
excerpt_separator: '<!--more-->'
categories:
  - CodeLion
tags:
  -
header:
  teaser: ./assets/image/.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

[URL](https://github.com/D-Sup/HODU_LandingPage)

<img class='img' style="max-height:none" src='https://user-images.githubusercontent.com/96939334/232327121-7eef7806-dcff-45c3-9a25-e041f60763f9.png' alt=''>

<span class='explain'>간단한 랜딩페이지 제작에 이어 반응형까지 구현해보았다</span> 

## 전체코드

```
<!DOCTYPE html>
<html lang="ko-KR">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HODU</title>
  <link rel="stylesheet" href="style.css">
</head>

<body>
  <div class="l-wrapper">
    <div class="header-bg">
      <header class="header">
        <div class="top-header">
          <h1 class="header-logo">
            <span class="a11y-hidden">HODU 로고</span>
            <a href="#none">
              <img src="./images/logo.svg" alt="">
            </a>
          </h1>
          <nav class="header-nav">
            <ul>
              <li>
                <a href="#none">Home</a>
              </li>
              <li>
                <a href="#none">About</a>
              </li>
              <li>
                <a href="#none">Support</a>
              </li>
              <li>
                <a class="header-btn" href="#none">
                  Download
                </a>
              </li>
            </ul>
          </nav>
          <img class="toogle-btn" src="./images/toggle.svg" alt="">
        </div>
        <div class="about">
          <div class="about-explain">
            <h2 class="about-headings">
              Lorem Ipsum is simply<br>
              dummy text of the printing and
            </h2>
            <p class="about-contxt">Lorem Ipsum is simply dummy text of the printing and<br>
              typesetting industry.<br>
              Lorem Ipsum has been the industry's standard dummy text<br>
              ever since the 1500s, when an unknown</p>
            <button class="about-btn">Download</button>
          </div>
          <img class="about-img" src="./images/cat.png" alt="박스를 머리에 뒤집어쓴 고양이 이미지">
        </div>
      </header>
    </div>
    <main class="main">
      <section class="section-body">
        <figure class="body-intro">
          <img class="intro-img" src="./images/img_4.png" alt="옆으로 눕고 고개를 옆으로 슥하고 덩그러니 무엇인가를 바라보는 고양이 이미지">
          <figcaption>
            <h3 class="intro-headings">
              Lorem Ipsum is simply<br>
              dummy text of the printing<br>
              and dummy text</h3>
            <p class="intro-contxt">
              Lorem Ipsum is simply dummy text of the printing and typesetting<br> industry.
            </p>
          </figcaption>
        </figure>
        <div class="body-info">
          <h3 class="info-headings">
            dummy text of the printing and dummy
          </h3>
          <p class="info-contxt">Lorem Ipsum is simply <strong>dummy text</strong> of the printing and typesetting
            industry.<br>
            Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown</p>
        </div>
        <div class="body-gallery">
          <ul class="gallery-list">
            <li>
              <button>
                <img src="./images/img_1.png" alt="캣타워에서 얼굴만 내밀고 무언가를 지켜보는 고양이 이미지">
              </button>
            </li>
            <li>
              <button>
                <img src="./images/img_2.png" alt="캣타워에서 얼굴만 내밀고 무언가를 지켜보는 고양이 이미지">
              </button>
            </li>
            <li>
              <button>
                <img src="./images/img_3.png" alt="나와 눈을 마주치고있는 고양이 이미지">
              </button>
            </li>
          </ul>
          <div class="gallery-caption">
            <p class="caption-contxt">Lorem Ipsum is simply dummy text of the printing and typesetting industry.<br>
              Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown<br>
              Ipsum is simply dummy text of the printing and typesetting industry. </p>
            <button class="caption-btn">Learn More</button>
          </div>
        </div>
      </section>
      <article class="banner-subscribe">
        <div class="subscribe-title">
          <h4 class="subscribe-headings">
            Subscribe to our Blog post
          </h4>
          <p class="subscribe-contxt">
            Lorem Ipsum is simply dummy text of the printing and typesetting industry.<br>
            Lorem Ipsum has been the industry's standard dummy text
          </p>
        </div>
        <form class="subscribe-form">
          <div class="subscribe-inpt">
            <img src="./images/msg.svg" alt="">
            <label for="email" class="a11y-hidden"></label>
            <input id="email" type="email" placeholder="Enter your e-mail address">
          </div>
          <button type="submit" class="subscribe-btn">Subscribe</button>
        </form>
      </article>
    </main>
    <footer class="footer">
      <img src="./images/img_5.png" alt="" class="footer-img">
      <div class="section-share">
        <h2 class="footer-logo">
          <span class="a11y-hidden">HODU 로고</span>
          <a href="#none">
            <img src="./images/logo_footer.svg" alt="HODU 로고 이미지">
          </a>
        </h2>
        <ul class="share-link">
          <li>
            <a href="#none">
              <img src="./images/sns1.svg" alt="bing 로고 이미지">
            </a>
          </li>
          <li>
            <a href="#none">
              <img src="./images/sns2.svg" alt="instagram 로고 이미지">
            </a>
          </li>
          <li>
            <a href="#none">
              <img src="./images/sns3.svg" alt="facebook 로고 이미지">
            </a>
          </li>
          <li>
            <a href="#none">
              <img src="./images/sns4.svg" alt="youtube 로고 이미지">
            </a>
          </li>
        </ul>
        <nav class="footer-nav">
          <ul>
            <li><a href="#none">About</a></li>
            <li><a href="#none">Blog</a></li>
            <li><a href="#none">Support</a></li>
            <li><a href="#none">Terms of Use</a></li>
          </ul>
        </nav>
      </div>
    </footer>
  </div>
</body>

</html>
```
{: .language-html .line-numbers}

```
body,
p,
ul,
figure,
button {
  margin: initial;
  padding: initial;
}
ul {
  list-style-type: none;
}
a {
  text-decoration: none;
  color: inherit;
}
h1,
h2,
h3,
h4 {
  font-size: inherit;
  font-weight: initial;
  margin: initial;
}
:root {
  font-family: 'Spoqa Han Sans Neo';
  font-weight: 400;
  --font-size-lg: clamp(24px, 3.5vw, 48px);
  --font-size-md: clamp(24px, 2.5vw, 36px);
  --font-size-sm: clamp(14px, 2vw, 16px);
}
img {
  /* max-width: 100%; */
  height: auto;
  vertical-align: top;
}
.a11y-hidden {
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: inset(50%);
  width: 1px;
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
}
body {
  background: #fff;
}
.l-wrapper {
  margin: 0 auto;
  width: 1920px;
}
.header-bg {
  background-color: #f2e9d8;
}
.header {
  position: relative;
  width: 66%;
  margin: 0 auto;
  padding-top: 31px;
  padding-bottom: 262px;
}
.top-header {
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 231px;
}
.header-logo a img {
  width: clamp(92px, 18vw, 130px);
}
.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.header-nav ul {
  display: flex;
  align-items: center;
  list-style-type: none;
  font-size: var(--font-size-sm);
}
.header-nav ul li + li {
  margin-left: 30px;
}
.header-btn {
  padding: 15px 30px;
  border: none;
  background: #d97652;
  font-size: var(--font-size-sm);
  font-weight: 700;
  color: #fff;
  border-radius: 40px;
}
.toogle-btn {
  display: none;
}
.about-headings {
  font-weight: 700;
  font-size: var(--font-size-lg);
}
.about-contxt {
  padding: 40px 0;
  font-size: var(--font-size-sm);
}
.about-btn {
  border: none;
  background: #d97652;
  padding: 15px 30px;
  border-radius: 40px;
  font-weight: 700;
  font-size: var(--font-size-sm);
  color: #fff;
}
.about-img {
  position: absolute;
  width: clamp(197px, 50%, 415px);
  margin-bottom: -120px;
  bottom: 0;
  right: 0;
}

/* main */

.main {
  width: 66%;
  margin: auto;
}
.section-body {
  padding: 80px 0 90px 0;
}
.intro-img {
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.25);
  border-radius: 30px;
  width: clamp(326px, 50%, 660px);
}
.body-intro {
  display: flex;
  gap: 110px;
  align-items: center;
}
.intro-headings {
  font-weight: 700;
  font-size: var(--font-size-md);
  padding-bottom: 30px;
}
.intro-contxt {
  font-size: var(--font-size-sm);
}
.info-headings {
  font-size: var(--font-size-lg);
  font-weight: 700;
  color: #d97652;
}
.body-info {
  margin-top: 120px;
  margin-bottom: 88px;
  text-align: center;
}
.info-contxt {
  font-size: var(--font-size-sm);
  padding-top: 40px;
}
.info-contxt strong {
  color: #d97652;
}
.gallery-list {
  display: flex;
  justify-content: space-between;
  overflow-x: hidden;
  overflow-x: scroll;
  margin-bottom: 90px;
}
.gallery-caption {
  display: flex;
  align-items: center;
  justify-content: center;
}
.gallery-list li ~ li {
  padding-left: clamp(20px, 2vw, 72px);
}
.gallery-list li button {
  border: none;
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.25);
  border-radius: 30px;
  overflow: hidden;
}
.gallery-list li button,
.gallery-list li button img {
  width: clamp(260px, 20vw, 378px);
}
.caption-contxt {
  font-size: var(--font-size-sm);
  padding-right: 52px;
}
.caption-btn {
  padding: 15px 30px;
  font-weight: 700;
  font-size: var(--font-size-sm);
  border: none;
  background: #d97652;
  border-radius: 40px;
  color: #fff;
}
.banner-subscribe {
  position: relative;
  margin-bottom: -100px;
  width: 100%;
  min-width: 326px;

  padding: 50px 58px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #263140;
  border-radius: 30px;
  color: #fff;
}
.subscribe-headings {
  font-weight: 700;
  font-size: var(--font-size-md);
  padding-bottom: 18px;
}
.subscribe-contxt {
  font-size: 14px;
  color: #f2e9d8;
}
.subscribe-form {
  width: 530px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #fff;
  padding-right: 5px;
  border-radius: 60px;
}
.subscribe-inpt {
  display: flex;
  align-items: center;
}
.subscribe-inpt img {
  padding-left: 30px;
  padding-right: 18px;
}
.subscribe-inpt input {
  font-size: var(--font-size-sm);
  border: none;
  background-color: none;
}
.subscribe-inpt input:focus {
  outline: none;
}
.subscribe-btn {
  border: none;
  padding: 15px 30px;
  background: #d97652;
  border-radius: 40px;
  font-weight: 700;
  font-size: var(--font-size-sm);
  color: inherit;
}

/* footer */

.footer-img {
  width: 100%;
  height: 340px;
  object-fit: cover;
}
.footer-logo a img {
  width: clamp(112px, 20vw, 226px);
}
.section-share {
  width: 66%;
  margin: auto;
  height: 160px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.share-link {
  display: flex;
}
.share-link li ~ li {
  padding-left: 20px;
}
.footer-nav {
  color: #767676;
  text-align: right;
  font-size: 14px;
  display: none;
}
.footer-nav ul li + li {
  padding-top: 16px;
}

@media (max-width: 1900px) {
  .l-wrapper {
    width: 100%;
  }
}
@media (max-width: 1730px) {
  .top-header {
    margin-bottom: 40px;
  }
  .about-img {
    display: block;
    position: static;
    margin: auto;
    margin-top: 30px;
  }
  .header {
    padding-bottom: 0px;
  }
  .banner-subscribe {
    flex-direction: column;
    gap: 30px;
  }
}
@media (max-width: 1410px) {
  .gallery-caption {
    flex-direction: column;
    gap: 41px;
    align-items: end;
  }
  .caption-contxt {
    margin: auto;
    padding: 0px;
  }
}
@media (max-width: 1250px) {
  .about-explain {
    display: flex;
    text-align: center;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
@media (max-width: 860px) {
  .body-info {
    margin-top: 40px;
    margin-bottom: 60px;
  }
  .gallery-list {
    margin-bottom: 60px;
  }
  .section-body {
    padding: 32px 0 50px 0;
  }
  .header {
    width: 85%;
  }
  .toogle-btn {
    display: block;
  }
  .main {
    width: 85%;
  }
  .section-share {
    width: 85%;
  }
  .header-nav {
    display: none;
  }
  .body-intro {
    flex-direction: column;
    gap: 40px;
  }
  .banner-subscribe {
    padding: 36px 32px 100px 32px;
  }
  .subscribe-btn {
    position: absolute;
    right: 0;
    bottom: -68px;
  }
  .subscribe-form {
    position: relative;
    width: 100%;
    height: 40px;
  }
  .subscribe-inpt img {
    padding-left: 17px;
    padding-right: 14px;
  }
}
@media (max-width: 450px) {
  .section-share {
    flex-direction: column;
    align-items: start;
    justify-content: space-evenly;
  }
  .footer-nav {
    display: block;
    position: absolute;
    right: 20px;
  }
  .share-link li ~ li {
    padding-left: 16px;
  }
}
```
{: .language-css .line-numbers}

<br>
<br>

## 생각해보기

> 랜딩 페이지를 구현하면서 내가 고민했던 부분인데 그 부분에 대해 답변 주신 내용은 이렇다

1) header 부분만 div로 감싸서 별개로 background-color 주었지만 이것이 올바른 방법일까? 더 좋은 방법이 없을까요? 
{:.notice--primary}


```html
<div class="header-bg">
<header class="header">
</header>
</div>
```

✔️ header 에 background-color 를 주고 그 안에 div를 사용해 감싸주는 방법이 더 나아보입니다.  
✔️ 형제요소들로 main 과 footer 가 있기 때문에  
&emsp;header가 이들의 형제요소로 사용되는게 좀 더 의미가 분명하고 통일성 있어 보입니다.
{:.notice--success}

<br>

---

2) 클래스 명을 짓는 부분에서 요소를 감싸고 컨테이너 역할을 하고있는 것을  
&emsp;조금 더 명시적으로 표현하기 위해 클래스 명을 지었는데  
&emsp;이것이, 컨테이너에 담긴 컨텐츠를 재사용할 수 있는 가능성과  
&emsp;문서를 더 구조적으로 생각할 수있다는 점을 고려 했을때 장점으로 느껴졌지만,  
&emsp;코드가 늘어나는 것을 고려해서 더 간결하게 구조 짜는 연습이 필요할까요?
{:.notice--primary}

```html
<section class="section-body">
  <figure class="body-intro">
  </figure>
  <div class="body-info">
  </div>
  <div class="body-gallery">
  </div>
</section>
```

✔️ 접두사로 section이나 body 같은 이름을 넣어서 지으면  
실제 존재하는 요소의 이름과 겹치기 때문에 혼란스럽지 않을까 생각합니다.  
✔️ 보통 많이 사용하는 컨테이너의 클래스 네이밍은 wrapper, box, cont 이런것들이 있으니 참고바랍니다 🙂
{:.notice--success}

<br>

---

3) 반응형을 위해 컨텐츠들이 최대한 .l-wrapper 의 너비에 맞게 요소가 늘어나고 줄어들 수 있도록 구현했는데,   
&emsp;header부터 footer까지 전체적인 컨텐츠의 너비가 동일하게 존재하기 때문에  
&emsp;.l-wrapper 너비 기준으로 컨텐츠의 너비를 %로 지정 하는 방법이 올바른 것일까요?
{:.notice--primary}

```css
.header{
  width: 66%;
}
.main {
  width: 66%;
}
.section-share{
  width: 66%;
}
```

✔️ 반응형을 위해 %를 많이 사용하면 생각보다 비율을 디자인 처럼 맞추기 쉽지 않습니다.  
&emsp;그래서 보통 반응형에서는 max-width 같은 속성을 많이 사용하는데요.  
&emsp;디자인 시안의 컨텐츠 넓이가 최대 크기라고 감안하고 작업하기 때문입니다.  
<br>
✔️ 만약 l-wapper 기준으로 안에 들어있는 컨텐츠의 넓이를 %로 줘버리면  
&emsp;l-wapper 커지면 커질 수록 컨텐츠의 넓이도 계속 커지게 됩니다.  
&emsp;디자이너는 그렇게 계속 커지기 보다는 일정 크기까지 커지면 커지기를 멈추는 디자인을 많이 사용합니다.  
&emsp;그래서 max-width 속성을 많이 사용합니다.
{:.notice--success}

<br>
<br>

## 피드백 받은 내용

<br>
&emsp;1. 디자인 시안에서 컨텐츠의 최대 넓이는 1280px 입니다. 이 넓이 이상 넘어가지 않게 작업해주세요 🙂  
<br>
&emsp;2. header-logo 에서 a11y-hidden은 필요 없을것 같습니다. Img 태그의 alt 값에 숨김 텍스트를 넣어도 충분합니다 🙂  
<br>
&emsp;3. .header-nav ul 의 list-style-type속성은 이미 ul 요소 전체적으로 리셋스타일로 넣어주셨습니다.  
&emsp;&emsp;이렇게 중복되는 속성은 모두 제거해주세요 🙂  
<br>
&emsp;4. about 과 about-explain 두 가지로 컨테이너를 나눠야 할 필요가 있을까 하는 생각이 듭니다.  
&emsp;&emsp;필요없는 요소는 과감히 제거해주세요!  
<br>
&emsp;5. about-btn에 type 이 명시되어있지 않습니다. 데이터 전달을 위한 submit 버튼이 아니라면 타입을 설정해주세요 🙂  
<br>
&emsp;6. .gallery-list li ~ li 에서 요소와 요소 사이의 간격은 마진으로 처리 부탁드립니다.  
&emsp;&emsp;그리고 여기서는 flex를 사용하셨기 때문에 gap 으로 간격을 주시는것도 좋아보입니다.  
<br>
&emsp;7. gallery-list 안의 버튼들에도 타입이 없습니다!  
<br>
&emsp;8. subscribe-inpt 요소안의 label에 텍스트가 없습니다. 무엇을 위한 input 요소인지 설명을 넣어주시기바랍니다.  
<br>
&emsp;9. subscribe-inpt 요소안의 input 의 넓이가 조금 긴 이메일을 담기에는 너무 좁아보입니다.  
&emsp;&emsp;사용자 편의를 위해 넓혀주세요 🙂 옆에있는 버튼 근처까지 가면 좋을것 같습니다.  
<br>
&emsp;10. share-link 안의 img 태그 alt 값에는 앵커 태그를 누르면 어디로 향하는지에 대한 설명 역할도 포함됩니다.   
&emsp;&emsp;‘bing 로고 이미지’ 보다는 ‘bing 으로 이동하기’ 정도의 대체 텍스트가 적당해보입니다.  
<br>
&emsp;11. l-wrapper 마진때문에 화면이 넓어지면 배경색이 끊어집니다. 재배치가 필요해보입니다.  
<br>
{:.notice--info}


