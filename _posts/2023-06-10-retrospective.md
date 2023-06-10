---
title: '동료특강기'
excerpt_separator: '<!--more-->'
categories:
  - 
tags:
  -
header:
  teaser: ./assets/image/39.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 동료특강을 시작하게 된 계기
---
> **지식과 이해의 경계에 대한 고찰**  
> 동료들과 함께 빠르게 배우고 적용해야 하는 상황에서  
> 지식과 이해를 명확히 구분하지 않는 한,   
> 배워온 것들이 내가 모르는 사이에 잊혀져 간다는 생각이 들었다.

> 지식과 이해에 대한 경계를 명확히 구분하기위해  
> 최대한 내가 알고 있는 사실에 대해서 의심하고  
> 동료들과 같이 공유하며 성장하는 것을 목표로 삼고있었다.

<span class='explain'>그러던 중...</span>

<br>
<br>

> 운영자님께서 DM으로 동료특강 요청에 대한 부탁을 주셨다. 🫢

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/a1ecffa3-e353-4682-8e83-8e438077ca99' alt=''>

<br>
<br>

그 동안 중간과제를 수행할 때마다 더 재미있게 구현하려는 목표로,  
작지만 재미있는 이스트에그 같은 기능을 추가하곤 했는데,   
강의 중에 그런 기능에 대한 반응이 꽤 좋아서 그런지 좋게 봐주셨던 것 같다.

<img class="img" src="https://user-images.githubusercontent.com/96939334/224724167-d7ac7645-ad11-4bc2-9ca4-08422735e7da.gif">
<img class="img" src="https://user-images.githubusercontent.com/96939334/227728438-e80c5eae-0732-4e01-8276-5e96cf4e5e2d.gif" alt="">

나를 이미 알고 계신 동료분들도 많았지만,   
나를 잘 모르시는 동료분들도 많았기에,   
많은 동료들 앞에서 특강을 진행하는 상상을 하니 어지러웠다...

<br>
<br>

> 아무튼 더 생각할 것 없이 바로 **승낙**했다 😄

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/4e48e392-0395-4c47-96f5-4ba2e2b7c552' alt=''>

<span class='explain smallText'>사실 조금 망설였다 ㅎㅎ</span>

<br>
<br>
<br>

# 동료특강 준비과정
---

> 특강 주제를 정하기 위해 먼저 우리 **회고조 분들에게 도움**을 청했다.  

최근에 공부하면서 어떤 부분이 이해가 안되고 어려웠는지,   
어떤 부분을 복습하고 싶은지에 대한 이야기를 나누었다.  

<span class='explain smallText'>내용은 이러했다~</span>

💡 `Promise` 를 실제로 코드에 적용하려니 어려워요.  
<br>
💡 `Promise` 는 이해가 가는데, `Promise.all` 로 **병렬 처리**하는 부분이 아직 어려워요.   
<br>
💡 `JavaScript` 에서 비동기 통신을 구현하는 것은 익숙해졌는데,  
&emsp;&emsp;**리액트**에서 `useState` 와 `useEffect` 로 렌더링을 제어하면서 **비동기 통신**을 처리하는 부분이 좀 어려워요.
{:.notice--primary}

<br>

마침 React hooks에 대한 내용을 학습하고 있었기에,  
API 비동기 통신과 React를 복습하는 느낌으로 특강을 준비하고 진행하면  
나 스스로도 많이 배우고 앞으로 진행할 팀 프로젝트에 있어서 동료들에게 도움이 될 것 같다는 생각이 들었다. 

<br>
<br>

> 그래서 주말 동안 **나만의 날씨 위젯**을 만들었다.

아무래도 특강은 라이브 코딩으로 진행되기 때문에,   
위젯을 만들어 내는 과정을 **목차별로 세분화**해서, 단계별로 파일을 하나씩 따로 만들었다.   
그리고 특강의 원활한 진행을 위해 각 파일에 대한 **스크립트**를 작성했다.

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/172d47ff-7b83-43fa-833b-e53927811a1d' alt=''>
<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/665f329a-c4a9-4cb6-98aa-191dd1996159' alt=''>

💡 날씨 위젯 관련 포스팅 링크  
<br>
&emsp;[[chapter.1] React로 날씨 Open API 다루기](https://d-sup.github.io/react/TIL-weather-api/)  
&emsp;[[chapter.2] React로 날씨 Open API 다루기](https://d-sup.github.io/react/TIL-weather-api-2/)  
{:.notice--primary}

<br>
<br>

> 요구사항에 맞게 구현만 하는 중간 과제를 수행할 때와는 완전히 다른 느낌이었다.   
> **문제는 여기서부터 시작되었다.**

특강을 진행하는데 주어진 시간은 1시간~1시간30분...  
목차별로 스크립트를 써내려가다 보니, 특강에서 전달해야 할 내용이 생각보다 많았다.   
무엇보다 문제는 이 모든 것이 라이브 코딩으로 이루어지기 때문에,   
빠르게 설명하는 동시에 에러 없이 진행되어야 시간에 맞게 끝낼 수 있는 분량이었다.

<span class='explain'>그래서..</span>

<br>
<br>

> 특강까지 남은 시간 동안,   
> 어떻게든 분량을 줄이기 위해 꼭 설명하지 않고 넘어갈 수 있는 내용들을 교안에 담으려고 노력했다.

시간이 부족해서 특강을 하루 더 미루어야 하나 고민했지만,   
이미 특강 공지를 올린 이상 무를 수 없었다. 

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/e9938a5f-0ac2-4993-8a82-52ae90310f2e' alt=''>

<br>
<br>
<br>

# 동료 특강 진행하며 겪은 순간들
---
<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/da90f073-f3fe-4abe-b57f-e4c2e1f2c044' alt=''>

<span class='explain smallText'>교안까지 모두 준비완료!</span>


> 드디어 기다리던 그 순간이 왔다...

> 마음을 다잡고 ZOOM에 입장했다.   
> 예상보다 훨씬 많은 동료분들이 참석해 주셨고, 그런 모습에 놀라움을 금치 못했다.   
> 그래도 차분함을 유지하며 차근차근 특강을 진행시켰고,   
> 교안을 최대한 활용하여 보여주고자 했던 내용을 차근차근 전달하려고 했다.

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/8e7713d6-510d-4190-a5e0-4f579d6f00aa' alt=''>

> 막힘없이 진행하고 싶었지만   
> 설명을 하면서 올바르게 코드를 작성하는 일이 정말 힘들다는 걸 다시금 느꼈다.  
> 에러가 발생하는 순간마다 머릿속이 하얘져서 어지럽기도 했지만,  
> 응원해주는 동료들 덕분에 바로 정신을 차리고   
> 에러를 하나씩 해결하면서 원활한 진행을 할 수 있었다.  
> 그러나 결국 분량 조절 실패로 2시간동안 특강을 진행했다..

<br>
<br>
<br>

# 마치며
---
> 준비기간이 넉넉하지 않아서  
> 특강을 더 철저하게 준비하지 못한 점이 개인적인 아쉬움으로 다가왔다.  
> 그러나 밤새 코드를 처음부터 끝까지 다시 타이핑하는 것을 무한반복하고,  
> 중간중간 교안도 수정하며 정신없는 상황 속에서도  
> 내 개인공부를 이어가며 정말 열심히 준비했다는 것에 자부심을 느낄 수 있었다. 😌

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/9a492272-b667-465f-8b34-9fbde77d0a0d' alt=''>

> 특강을 마친 이후  
> 많은 동료분들이 DM을 남겨주셨고,  
> 이런 기회로 동료들과 소통하고 나의 배움을 나눌 수 있다는 것에  
> 큰 보람을 느끼며 마무리 지을 수 있었다.

<br>
<br>
<br>

# 수상 🎉
---

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/caf615b5-ef98-4d4e-aae9-1d4a62f8f490' alt=''>

🎖️ 수료하면서 받은 콘텐츠상 🎖️  
😊 좋은 자리를 마련해주신 운영진분과 동료들에게 감사의 마음을 전합니다. 😊


