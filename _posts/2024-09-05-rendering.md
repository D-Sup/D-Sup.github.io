---
title: '렌더링'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/47.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

## Static Sites

> 서버에 미리 만들어진 HTML 문서들이 저장되어 있고,  
> 사용자가 브라우저로 사이트에 접속하면 서버에 저장된 HTML 문서를 받아와서 화면에 표시하는 방식이었다.  
> 이러한 방식을 MPA 라고 한다.

{:.notice--primary}
💡 **`Static Sites의 단점`**<br><br>
페이지 내에서 다른 링크를 클릭하면  
서버에서 해당 페이지의 HTML을 다시 받아와서 페이지 전체를 업데이트해야 하기 때문에  
사용성이 떨어지는 단점이 존재했다.

<br><br><br>

## MPA (MultiPageApplication)

> 여러개의 페이지로 구성된 어플리케이션으로  
> 사용자가 페이지를 요청할 때마다 서버로부터 새로운 HTML을 받아와  
> 페이지 전체를 새로 랜더링 하는 전통적인 웹페이지 구성방식이다.

- **SEO 유리** : 완성된 형태의 HTML파일을 서버에서 전달받기때문에 검색엔진이 페이지를 크롤링하기 적합하다.
- **화면 깜빡임 문제** : 매번 페이지 전체를 새로 불러와서 렌더링해야하기 때문에 화면이 깜빡이는 문제가 존재한다.
- **개발 복잡도 증가** : 각 페이지가 독립적으로 개발되므로, 페이지 간의 코드 중복이 발생할 수 있다.

<br><br><br>

## SPA (Single Page Application)

> SPA는 한개의 페이지로 구성된 어플리케이션으로  
> MPA의 불편함을 개선하고자  
> 최초 페이지 로딩 시 필요한 모든 리소스를 다운로드한 후,  
> 사용자가 페이지를 요청할 때 갱신에 필요한 데이터만 전달 받아 페이지를 갱신하는 방식이다.

- **향상된 사용자 경험** : 사용자는 페이지 이동 시 깜빡임 현상 없이 자연스러운 화면 전환을 경험할 수 있다.
- **초기 구동속도의 느림** : 초기에 앱에 필요한 모든 정적 리소스를 다 받아야하기 때문에 초기 구동속도가 느리다.

<br><br><br>

## 웹 어플리케이션의 발전

> **1990년**  
> 1990년 중반까지는 모두 다 Static Sites 였다.

> **1996년**  
> 문서 내에서 또 다른 문서를 담을 수 있는 iframe태그가 도입이 되면서  
> 페이지 내에서 부분적으로 문서를 받아와서 업데이트할수 있게 되었다.

> **1998년**  
> 우리가 많이 쓰고 있는  
> fetch API의 원조 XmlhHttpRequest API가 개발이 되었다.  
> 그 이후 HTML 문서 전체가 아니라 JSON과 같은 포맷으로  
> 서버에서 가볍게 필요한 데이터만 받아올 수 있게 되면서  
> 받아온 데이터를 통해 자바스크립트를 이용해서 동적으로 HTML요소를 생성해서 페이지에 업데이트하는 방식을 사용했다.

> **2005년~**  
> 2005년, 이러한 방식이 드디어 공식적인 AJAX라는 이름을 가지게 되었고,  
> 구글에서도 AJAX를 이용해서 우리가 많이 쓰고있는 웹 어플리케이션을 만들기 시작했다.  
> 이러한 트렌드가 이어지면서  
> 사용자들의 PC성능이 향상되고,  
> 자바스크립트 표준화가 이루어지고,  
> Angular, React, Vue와 같은 자바스크립트 기반 프레임워크가 나오면서  
> CSR 시대에 접어들게 된다.

{:.notice--primary}
💡 **`짚고 넘어가기`**<br><br>
MPA, SPA는 웹 페이지 구성 방식에 따른 분류이고  
CSR, SSR 등은 렌더링 방식에 따른 분류이다.  
일반적으로 SPA에서는 CSR로 렌더링하고, MPA에서는 SSR로 렌더링 한다.

<br><br><br>

## CSR (Client Side Rendering)

<img class='img-full' src='https://github.com/user-attachments/assets/e0ce763c-275c-4725-b4d2-f20206295f07' alt=''>
  
> 자바스크립트를 사용해서 브라우저에서 직접 페이지를 렌더링하는 렌더링 방식이다.  
>
  
  
<br>
### 동작방식
  
1. 사용자가 웹페이지 방문 (request)  
2. CDN이 HTML 파일과 JS로 접근할 수 있는 링크를 클라이언트로 보낸다.  
3. 클라이언트는 최소한의 HTML파일(script, meta, link 등을 포함한 빈 컨텐츠)을 다운로드한다.(response)  
4. 위에서 다운로드한 HTML파일에 포함된 script태그를 읽어 자바스크립트 번들 파일을 다운로드한다.  
5. 다운이 완료된 JS가 실행된다. 데이터를 위한 API가 호출된다. (이때 유저들은 placeholder(최종 콘텐츠가 로드되기 전까지 사용자에게 보여지는 임시적인 시각적 요소)를 보게된다.)  
6. API로부터 받아온 data를 placeholder 자리에 넣어준다. 이제 페이지는 상호작용이 가능해진다.  
7. 사용자가 페이지를 이동할 경우, 서버에 추가 HTML파일을 요청하는게 아니라 이미 받은 자바스크립트를 이용하여 렌더링한다.  
  
<br>
### 특징
  
- **서버 부하가 적음** : 서버가 빈 뼈대HTML를 넘겨주는 역할만 수행하면 되기 때문에 서버측 부하가 적다.  
- **빠른 반응속도** : 서버와의 통신 없이도 클라이언트 측의서의 연산, 라우팅 등을 모두 직접 처리하기 때문에 반응속도가 빠르고 UX도 우수하다는 장점이 있다.  
- **SEO 불리** : 검색엔진들이 CSR로 작성된 웹페이지를 분석하기에는 HTML 바디의 내용이 너무 부족하기 때문에 분석에 어려움을 겪는다.  
- **초기 구동속도가 느림** : 브라우저가 자바스크립트 번들 파일(어플리케이션에서 필요한 로직들 뿐만 아니라, 어플리케이션을 구동하는 프레임워크와 라이브러리의 소스 코드들이 포함)을 다운로드 받고, 동적으로 DOM을 생성하는 시간을 기다려야하기 때문에 초기 로딩속도가 느리지만 초기 로딩 이후에 페이지 일부를 변경할 때는 서버에 해당 데이터만 요청하면 되기 때문에 이후 구동속도는 빠르다. 번들사이즈가 커지면 로딩 속도가 느려질 수 있다.  
- **클라이언트에 의존** : 클라이언트의 하드웨어 및 소프트웨어에 많이 의존한다.  
- **TTFB 와 TTI 사이 간극이 없다** : 사용자가 웹 사이트를 볼 수 있음과 동시에 클릭을 하거나 인터랙션이 가능해진다.  
- **빠른 TTFB** : 초기 페이지 로드 시 서버가 최소한의 HTML을 빠르게 전달하기 때문에 TTFB가 빠르다  
  
{:.notice--primary}
💡 **`CDN (Content Delivery Network)`**<br><br>
지리적으로 분산된 서버들을 연결한 네트워크로  
사용자가 웹 사이트를 방문할 때, 서버의 데이터는 인터넷을 통해 이동해야 하며,  
서버와 멀리 떨어져 있을 경우 대용량 파일 로드에 시간이 오래 걸리기 때문에  
웹 사이트 콘텐츠는 지리적으로 사용자와 가까운 CDN 서버에 저장되어 더 빠르게 컴퓨터에 도달할 수 있다.  
<br>
예를 들어, 한국에서 미국사이트에 접속하는 경우  
미국 서버에 데이터를 요청하고 받아오는 과정이 오래 걸리기 때문에  
두 국가의 중간 지점에 있는 국가의 서버에 접속하고자하는 사이트의 데이터가 있는지 물어본다.  
만약 데이터가 없으면 미국 서버에 데이터를 요청해 받은 후 캐싱해두고  
이후에 다른 사용자가 또 같은 데이터를 요청하면  
미국 서버에 요청할 필요 없이 캐싱해둔 데이터를 보내줄 수 있는 것이다.  
  
  
{:.notice--primary}
💡 **`성능 관련 용어`**<br><br>
**TTI(Time to Interactive)** : 페이지가 완전히 로드되어 사용자가 상호작용할 수 있는 상태가 되는 데 걸리는 시간  
<br>
**TTFB (Time to First Byte)** : HTTP 요청을 했을 때 처음 byte(정보)가 브라우저에 도달하는 시간  
<br>
**FCP (First Contentful Paint)** : 웹 페이지가 로드될 때 사용자가 처음으로 콘텐츠를 시각적으로 인식할 수 있는 시간  
  
  
{:.notice--primary}
💡 **`CSR의 고민거리`**<br><br>
사용자가 웹 페이지를 빠르게 로드할 수 있도록 필수적인 요소들을 포함한 자바스크립트 파일을 최적화하여 전송하는 것이 중요하기 때문에  
최종적으로 번들링된 자바스크립트 파일을 효율적으로 분할하여,  
사용자가 처음 페이지를 방문할 때 필요한 최소한의 데이터만을 전송하는 방법에 대해 고민하는 것이 필요하다.  
  
  
{:.notice--primary}
💡 **`CSR의 가장 큰 문제점`**<br><br>
사용자가 처음에 접속하면  
서버는 빈 뼈대HTML 을 클라이언트에게 전달하기 때문에 빈화면을 보게되는데  
이후에 브라우저가 자바스크립트 번들 파일을 다운로드 받을때 번들 파일이 클 경우  
사용자가 첫 화면을 보기까지 시간이 오래 걸릴 수 있다는 점과  
SEO 불리하다는 점이  
사용자 경험과 웹 페이지의 성능에 큰 영향을 미쳤고  
Static Sites에서 영감을 받은 SSR을 도입되게 되었다.  
  
  
<br><br><br>
## SSR (Server Side Rendering)
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/f0c65776-329b-4cc9-a69c-fd43e5e69bd9' alt=''>
  
> 브라우저에서 웹 페이지를 렌더링하는 대신 서버에서 웹 페이지를 생성하는 방법으로  
서버에서 완전히 렌더링 된 페이지를 클라이언트로 보내서 렌더링하는 렌더링 방식이다.  
>
  
  
<br>
### 동작 방식
  
1. 사용자가 웹페이지 방문 (request)  
2. 서버는 즉시 렌더링 가능한 html파일을 만든다. (리소스 체크, 컴파일 후 완성된 HTML 컨텐츠로 만든다.)  
3. 클라이언트에 전달되는 순간, 이미 렌더링 준비가 되어있기 때문에 HTML은 즉시 렌더링 된다. 그러나 사이트 자체는 조작 불가능하다. (Javascript가 읽히기 전이다.)  
4. 클라이언트가 자바스크립트를 다운받는다.  
5. 다운 받아지고 있는 사이에 유저는 컨텐츠는 볼 수 있지만 사이트를 조작 할 수는 없다. 이때의 사용자 조작을 기억하고 있는다.  
6. 브라우저가 Javascript 프레임워크를 실행한다.  
7. JS까지 성공적으로 컴파일 되었기 때문에 기억하고 있던 사용자 조작이 실행되고 이제 웹 페이지는 상호작용 가능해진다.  
8. 사용자가 페이지를 이동할 경우, 위 과정을 반복한다.  
  
<br>
### 특징
  
- **SEO 유리** : 서버에서 랜더링 준비를 마친 HTML을 브라우저에 응답으로 전달하여 이미 HTML에 담겨진 채로 브라우저에 전달되기 때문에 SEO가 유리하다.  
- **초기 구동속도가 빠름** : 자바스크립트 코드를 다운받고 실행하기 전에 사용자가 화면을 볼 수 있다.  
- **서버 부하 증가** : 서버에서 렌더링까지 진행하면 서버가 담당하는 일이 많기 때문에 서버에 부하가 걸릴 위험이 존재한다.  
- **TTFB 와 TTI 사이 간극이 존재** : 데이터가 채워진 html문서를 화면에 띄운 다음 js를 다운받기 때문에 렌더링과 동시에 인터렉션할 수 없다.  
- **느린 TTFB** : 페이지 이동 시마다 서버에서 페이지를 생성하기 때문에 TTFB가 느리다.  
  
{:.notice--primary}
💡 **`SSR의 고민거리`**<br><br>
사용자가 자바스크립트를 통해 상호작용하는 시간을 줄이기 위해  
어떤 전략을 사용할 수 있을지에 대해 심도 있게 고민하는 것이 필요하다.  
  
  
<br><br><br>
## SSG (Static Site Generation)
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/afcaddd1-3902-400f-8c99-a61ba5a54639' alt=''>
  
> 정적 사이트 생성으로,  
페이지를 빌드 시점에 미리 렌더링하여 정적인 HTML 파일을 생성하는 방식이다.  
이렇게 생성된 HTML 파일은 서버에 저장되고,  
사용자가 웹 페이지에 접속하면 서버는 해당 HTML 파일을 즉시 제공한다.  
>
  
  
<br>
### 동작 방식
  
1. SSG 도구는 작성한 웹 페이지 템플릿과 콘텐츠 데이터를 결합해서 정적인 HTML 파일을 생성한다.  
2. 생성된 HTML 파일은 웹 서버에 저장됩니다. 이 파일은 나중에 사용자에게 제공될 준비가 된다.  
3. 사용자가 웹 페이지에 접속하면, 브라우저가 웹 서버에 해당 HTML 파일을 요청한다.  
4. 웹 서버는 요청받은 HTML 파일을 즉시 브라우저에 전송한다.  
5. 브라우저는 서버로부터 받은 HTML 파일을 통해 화면에 표시한다.  
  
<br>
### 특징
  
- **빠른 성능 지표 제공** : 빌드 타임에 HTML파일을 생성하기 때문에 빠른 FCP, TTI를 제공한다.  
- **일관된 응답 속도** : 매 요청마다 HTML파일을 생성하는 것이 아니기 때문에 SSR과 달리 일관성있게 빠른 TTFB를 달성할 수 있다.  
- **SEO 유리** : 미리 생성된 HTML 파일을 제공하므로 검색 엔진이 페이지를 쉽게 크롤링하고 인덱싱할 수 있어 SEO가 유리하다.  
- **URL 예측의 어려움** : 모든 URL에 대한 개별 HTML파일을 생성해야 하기 때문에 URL을 미리 예측할 수 없으면 적용이 어렵다.  
- **데이터 업데이트 문제** : 콘텐츠가 변경되면 웹 페이지를 다시 빌드해야 한다.  
- **빌드 시간 증가** : 웹 페이지가 많거나 콘텐츠가 자주 변경되는 경우 빌드 시간이 증가할 수 있다. 대규모 웹 사이트의 경우 빌드가 오래 걸리는 것은 말할 것도 없고, 관리하는 것도 매우 번거롭다.  
  
<br><br><br>
## ISR (Incremental Static Regeneration)
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/eca84223-7d5a-43e3-b865-886bb726ad51' alt=''>
  
> 점진적 정적 재생성으로,  
SSG와 같이 정적 페이지를 초기 빌드로 제공하면서,   
설정된 주기에 따라 백그라운드에서 페이지를 재생성해서 최신 데이터를 반영하는 방식으로  
SSG의 장점인 빠른 로딩 속도와 SEO 유리함은 유지하면서,  
동적인 콘텐츠를 효과적으로 업데이트할 수 있도록 개선된 방식이다.  
>
  
  
<br>
### 동작 방식
  
1. ISR 도구는 작성한 웹 페이지 템플릿과 콘텐츠 데이터를 결합하여 정적인 HTML 파일을 생성한다.  
2. 생성된 HTML 파일은 웹 서버에 저장되며, 이 파일은 나중에 사용자에게 제공될 준비가 된다.  
3. 사용자가 웹 페이지에 접속하면, 브라우저가 웹 서버에 해당 HTML 파일을 요청한다.  
4. 웹 서버는 요청받은 HTML 파일을 즉시 브라우저에 전송한다.  
5. 브라우저는 서버로부터 받은 HTML 파일을 통해 화면에 표시한다.  
6. 이후 설정된 재생성 간격이 지나면, 다음 사용자의 요청 시 서버는 백그라운드에서 페이지를 재생성하여 최신 콘텐츠를 반영한다.  
7. 재생성이 완료되면, 이후 요청하는 사용자에게는 업데이트된 HTML 파일이 제공된다.  
  
<br>
### 특징
  
- **빠른 성능** : ISR은 페이지를 미리 렌더링하고 캐시하기 때문에 매우 빠른 응답 속도를 제공한다.  
- **배포 불필요** : 콘텐츠가 변경되더라도 사이트를 다시 배포할 필요가 없어 운영이 용이하다.  
- **검색 엔진 최적화(SEO)** : 정적인 HTML 파일을 제공하므로 검색 엔진 최적화에 유리하다.  
- **콘텐츠 업데이트 지연** : ISR의 한 가지 큰 단점은 콘텐츠가 변경된 후 사이트를 방문하더라도 이전의 콘텐츠를 보게 되는 경우가 발생할 수 있다.  
- **최신 정보 반영 지연** : 새 버전의 웹사이트가 확인되기까지 시간이 걸리므로, 사용자가 최신 정보를 즉시 확인하지 못할 수 있다.  
  
# CSR의 단점을 보완할 수 있는 방법들  
  
<br><br><br>
## CSR + SSR/SSG
  
  
> CSR과 SSR 또는 SSG를 결합한 애플리케이션(초기 렌더링으로는 SSR + 이후 CSR)은  
"Isomorphic App" 또는 "Universal App"이라고 불린다.  
이러한 접근 방식은 CSR의 장점과 SSR 또는 SSG의 장점을 결합하여 SEO와 성능을 개선한다.  
>
  
  
<br>
### 프레임워크 없이 구현하는 방식
  
- **Express.js** : 별도의 서버를 직접 운영하여 CSR과 SSR/SSG를 결합할 수 있다. Express.js를 사용하면 유연하게 서버를 구성할 수 있다.  
- **NestJS** : TypeScript를 기본적으로 지원하는 NestJS를 활용하여 서버를 구축할 수 있다. 그러나 NestJS는 설정이 복잡할 수 있으므로 주의가 필요하다.  
  
<br>
### 프레임워크를 활용하기
  
- **Next.js**: 페이지의 성격에 따라 SSR과 SSG를 선택할 수 있는 강력한 프레임워크이다. Next.js는 동적 라우팅과 API 라우트를 지원하여 다양한 애플리케이션 요구 사항을 충족할 수 있다.  
- **Gatsby.js** : SSG에 최적화된 리액트 기반 정적 페이지 생성 프레임워크로, 다양한 플러그인을 제공한다. Gatsby.js는 CSR, SSR, 레이지 로딩 등도 지원하며, 페이지 수가 적고 작은 서비스에 적합한 솔루션이 될 수 있다.  
  
<br><br><br>
## 초기 로딩 속도 보완
  
  
> code splitting, tree-shaking, chunk 분리를 통해 JS 번들 크기를 줄여서  
초기 DOM 생성 속도를 줄이면 초기 로딩속도를 개선할 수 있다.  
>
  
  
<br>
### `Code Splitting`
  
> Code splitting은 애플리케이션의 JavaScript 코드를 여러 개의 작은 청크로 나누는 기법이다.  
Code splitting을 적용하기 위해서는 주로 Webpack과 같은 모듈 번들러를 사용한다.  
다음은 Webpack을 이용한 code splitting의 기본적인 방법이다.  
>
  
- Dynamic Imports 사용하기: JavaScript의 import() 함수를 사용하여 동적으로 모듈을 로드할 수 있다. 예를 들어, 특정 컴포넌트를 필요할 때만 로드하도록 설정할 수 있다.  
javascript  
  
```jsx
// 예시: React 컴포넌트에서 동적 import 사용
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
return (
<React.Suspense fallback={<div>Loading...</div>}>
<LazyComponent />
</React.Suspense>
);
}

````

- Entry Points 설정하기: Webpack의 entry 속성을 사용하여 여러 개의 진입점을 설정할 수 있다. 각 진입점은 별도의 청크로 나뉘어 로드된다.


```JSON
module.exports = {
entry: {
main: './src/index.js',
admin: './src/admin.js',
},
output: {
filename: '[name].bundle.js',
path: path.resolve(\_\_dirname, 'dist'),
},
};
```

<br>
### `Tree Shaking`

> Tree shaking은 사용하지 않는 코드를 제거하는 과정으로
> 모듈 번들러를 사용해서 애플리케이션의 의존성을 분석하고,
> 실제로 사용되는 코드만 포함시킴으로써 번들 크기를 줄일 수 있다.
> Tree shaking을 적용하기 위해서는 ES6 모듈을 사용해야 하며,
> Webpack과 같은 모듈 번들러에서 기본적으로 지원한다.
> 다음은 tree shaking을 적용하는 방법이다.

- ES6 모듈 사용하기: CommonJS 대신 ES6 모듈을 사용하여 코드의 의존성을 명확히 한다.
  javascript

```jsx
// 예시: ES6 모듈
export const usedFunction = () => {
  /* ... */
};
export const unusedFunction = () => {
  /* ... */
};
```

- Production 모드로 빌드하기: Webpack을 production 모드로 설정하면 tree shaking이 자동으로 활성화된다.

```bash
webpack --mode production
```

- Side Effects 설정하기: package.json 파일에 sideEffects 속성을 추가하여 어떤 파일이 부작용을 일으키지 않는지 명시할 수 있다.

```json
{
  "sideEffects": false
}
```

<br>
### `Chunk 분리`

> Chunk 분리는 애플리케이션의 특정 기능이나 페이지에 따라 코드를 나누는 방법이다
Chunk 분리를 적용하기 위해서는 Webpack의 optimization.splitChunks 옵션을 활용할 수 있다.
이를 통해 공통 모듈을 별도의 청크로 분리할 수 있다.
>

- SplitChunks 설정하기: Webpack의 설정 파일에서 splitChunks 옵션을 사용하여 청크 분리를 설정한다.

```json
module.exports = {
optimization: {
splitChunks: {
chunks: 'all',
minSize: 20000,
maxSize: 0,
minChunks: 1,
maxAsyncRequests: 30,
maxInitialRequests: 30,
automaticNameDelimiter: '~',
automaticNameMaxLength: 30,
name: true,
},
},
};
```

- Lazy Loading 사용하기: 특정 페이지나 기능에 대해 lazy loading을 적용하여 필요할 때만 청크를 로드하도록 설정한다.

```jsx
// 예시: React Router와 함께 lazy loading 사용
import { BrowserRouter as Router, Route } from 'react-router-dom';

const Home = React.lazy(() => import('./Home'));
const About = React.lazy(() => import('./About'));

function App() {
return (
<Router>
<React.Suspense fallback={<div>Loading...</div>}>
<Route path="/" exact component={Home} />
<Route path="/about" component={About} />
</React.Suspense>
</Router>
);
}

```

<br><br><br>

# 적재적소에 렌더링 방식을 이용하려면?


<br>
### CSR

- 유저와 상호작용이 많다?
- 대부분이 고객의 개인정보로 이루어진 페이지들이라 검색엔진에 노출될 필요는 없다?

<br>
### SSR

- 회사 홈페이지여서 홍보나 상위노출이 필요하다?
- 누구에게나 항상 같은 내용을 보여준다?
- 업데이트가 빈번해 해당 페이지 데이터가 자주 바뀐다?

<br>
### SSG

- 회사 홈페이지여서 홍보나 상위노출이 필요하다?
- 누구에게나 항상 같은 내용을 보여준다?
- 업데이트를 거의 하지 않는다?

<br>
### ISR

- 사용자에게 최신 정보를 제공하면서도 정적 페이지의 성능을 유지하고 싶다?
- 특정 데이터가 자주 변경되지만, 전체 페이지를 매번 새로 고칠 필요는 없다?

<br>
### Universal Rendering

- 사용자에 따라 페이지 내용이 달라진다?
- 빠른 interaction과 화면 깜빡임이 없어야 한다?
- SEO를 포기할 수 없어 상위노출이 되면 좋겠다?


<br><br><br>
**reference**

- <a href='tps://www.youtube.com/watch?v=iZ9csAfU5Os' target='_blank'>tps://www.youtube.com/watch?v=iZ9csAfU5Os</a>

- <a href='tps://www.youtube.com/watch?v=YuqB8D6eCKE&t=408s' target='_blank'>tps://www.youtube.com/watch?v=YuqB8D6eCKE&t=408s</a>

- <a href='tps://velog.io/@cookie004/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A0%8C%EB%8D%94%EB%A7%81-SSG-vs-ISG-vs-SSR-vs-CSR-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%A4-%EA%B2%83%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C%EC%9A%94' target='_blank'>tps://velog.io/@cookie004/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A0%8C%EB%8D%94%EB%A7%81-SSG-vs-ISG-vs-SSR-vs-CSR-%EC%96%B8%EC%A0%9C-%EC%96%B4%EB%96%A4-%EA%B2%83%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C%EC%9A%94</a>

- <a href='tps://limelbe.tistory.com/90' target='_blank'>tps://limelbe.tistory.com/90</a>
```
````
