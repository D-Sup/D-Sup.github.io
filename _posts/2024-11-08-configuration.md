---
title: '설정관련'
excerpt_separator: '<!--more-->'
categories:
  - frontend
tags:
  -
header:
  teaser: ./assets/image/49.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->
  
# **빌드 도구(Build Tool)**  
  
  
> **빌드 도구**는 웹 애플리케이션의 개발 및 빌드 과정에서 다양한 작업을 처리하는 도구들이라고 할 수 있는데  
아래와 같은 요소들을 빌드 도구라고 할 수 있다.  
>
  
- **모듈 번들러**: 여러 개의 모듈이나 파일을 하나의 파일로 묶어주는 도구로, Webpack이나 Rollup이 이에 해당한다.  
- **트랜스파일러 및 폴리필**: 최신 JavaScript 문법을 구형 브라우저에서도 사용할 수 있도록 변환하는 도구로, Babel이 대표적이다,  
- **코드 린터 및 포맷터**: 코드의 품질을 검사하고 일관된 스타일로 포맷팅하는 도구로, ESLint나 Prettier가 있다.  
- **패키지 관리자**: 라이브러리나 패키지를 관리하는 도구로, npm이나 Yarn이 있다.  
- **개발 서버**: 개발 중에 애플리케이션을 실행하고 테스트할 수 있는 서버로, 예를 들어 Webpack Dev Server가 있다.  
- **작업 실행기(태스크 러너)**: 반복적인 작업을 자동화하는 도구로, 예를 들어 Gulp나 Grunt가 있다.  
  
이 중에서 오늘은 **`모듈 번들러`, `트랜스파일러 및 폴리필`, `코드 린터 및 포맷터`, `패키지 매니저` 에 대해서 알아볼 것 이다.**  
  
<br><br><br>
## 모듈 번들러 (module bundler) 란?
  
  
의존성이 있는 여러개의 모듈( HTML, CSS, JavaScript, Images 등)을 하나의 JS파일로 합쳐주는 도구이다.  
그럼 모듈을 왜 하나로 합치는 걸까?  
  
<br>
### 모듈 번들러의 탄생배경
  
> 초기 JavaScript에서는 스크립트가 순차적으로 실행되며,   
여러 개의 자바스크립트 파일에서 변수를 불러올 때 `변수 이름이 충돌하는 문제`가 발생했다.  
즉 var로 선언된 변수가 전역 변수로 취급해서   
서로 다른 파일에서 동일한 변수 이름을 사용할 경우  
선언된 변수가 이전의 값을 덮어쓰는 현상이 발생한다.  
>
  
  
> 이러한 문제를 해결하기 위해 “`모듈`” 개념이 도입되었다.  
모듈 개념이 도입되면서 JavaScript의 코드 구조화가 가능해졌고,   
이에 따라 `CommonJS`가 등장한다.   
CommonJS는 주로 서버 환경인 Node.js에서 사용되고  
서버 전용으로 설계되었기 때문에 브라우저 환경에서는 비동기적으로 모듈을 로드하는 데 한계가 있었다.   
이러한 한계를 극복하기 위해 `AMD(Asynchronous Module Definition)`가 도입된다.   
AMD는 브라우저에서 비동기적으로 모듈을 로드할 수 있도록 설계되었는데  
AMD는 복잡한 의존성 관리와 CommonJS와의 호환성 문제가 있었다.  
이러한 문제들을 해결하기 위해 `ES6(ECMAScript 2015)`가 도입되었고,  
ES6 모듈 시스템은 각 파일 단위로 스코프를 제한하여 모듈 간의 이름 충돌을 방지할 수 있게 하였고,   
정적 분석이 가능해져 최적화와 트리 쉐이킹 같은 기능을 지원하게 된다.  
>
  
  
> 하지만 이러한 모듈의 기능은 최신 브라우저에서만 지원되었고  
각 모듈 파일을 `개별적으로 불러와야 하는 문제점`이 있다.   
예를 들어, 100개의 모듈 파일이 있다면 100번 이상의 네트워크 통신이 필요하게 되어   
초기 로딩 속도가 저하되고, 네트워크 부하가 증가하여 서비스 제공이 느려지는 문제가 발생한다.  
>
  
  
> 이러한 문제를 해결하기 위해 개발자들은 여러 모듈을 하나로 합쳐서 제공하는 방법을 고민하게 되었고,   
이로 인해 `모듈 번들러`가 탄생하게 된다.   
모듈 번들러는 여러 개의 파일을 하나로 통합하고,   
파일 크기를 줄이고,   
사용되지 않는 소스코드를 분석하여 제거하는 등의 기능을 제공한다.  
>
  
  
{:.notice--primary}
💡 **`모듈 번들러와 함께 사용되는 Babel과 Polyfill`**<br><br>
대표적인 모듈 번들러로는 `Webpack`이 있다.  
Webpack은 `Polyfill`과 `transpiler`을 함께 사용하여  최신 자바스크립트 문법을 구형 브라우저에서도 사용할 수 있도록 변환해줄 수 있다.   
이를 통해 개발자는 최신 JavaScript 문법을 사용하면서도 모든 사용자에게 일관된 경험을 제공할 수 있다.  
  
  
<br>
### 모듈 번들러의 역할
  
- **모듈 번들링**: 웹 애플리케이션에서 사용되는 JavaScript, CSS, 이미지 등의 모든 자원을 모듈로 관리하고, 이를 하나의 번들 파일로 묶어준다.  
- **의존성 해결**: 애플리케이션의 모듈 간의 의존성을 분석하여 필요한 모듈을 동적으로 로드하고 순서대로 실행할 수 있게 해준다.  
- **번들 최적화**: 중복되는 코드나 사용되지 않는 코드 등을 제거하여 번들 크기를 최소화하고 로딩 속도를 향상시켜준다.  
- **코드 스플리팅**: 애플리케이션을 여러 개의 작은 청크(chunk)로 분할해서 필요한 부분만 로드하여 초기 로딩 시간을 단축시킬 수 있다.  
- **리소스 관리**: 번들에 포함된 이미지, 폰트 등의 리소스를 관리하고 필요한 경우 최적화하여 제공한다.  
  
<br>
### Webpack
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/b48da21a-ad18-40f3-b3de-115a4b955895' alt=''>
  
> 대표적인 JavaScript 모듈 번들러로,  
명령어 환경(CLI) 또는 Webpack 구성 파일을 통해 Webpack을 사용할 수 있다.  
Webpack은 Entry 포인트를 시작으로, 의존적인 모듈을 전부 찾아내서 하나의 파일로 번들링 해준다.  
>
  
  
**기능**  
  
- CommonJS, AMD, ES6 Module 포맷을 모두 지원한다.  
- JS 뿐만 아니라 CSS, Image 등의 복잡한 의존성을 관리한다.  
- 성능 최적화  
    - 사용하지 않는 코드를 제거하는 Tree Shaking 같은 최적화를 수행한다.  
    - HTTP 요청 수가 감소하여 네트워크 비용이 감소된다.  
- Code Splitting  
    - 원하는 때에 모듈을 로딩할 수 있는 Dynamic Loading, Lazy Loading이 가능하다.  
  
**핵심 사용법**  
  
  
자세한 사용법은 → https://r0k.wiki/posts/javascript/webpack/#%EC%9B%B9%ED%8C%A9-%EB%8D%B0%EB%B8%8C-%EC%84%9C%EB%B2%84   
  
> `webpack.config.js` 라는 설정 파일을 기반으로 동작하게 되어있고  
다양한 속성들을 사용해서 웹팩의 동작을 커스터마이징할 수 있다.  
그 중에서도 핵심 속성에 해당하는 요소들은 아래와 같다.  
>
  
  
```jsx
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
entry: './path/to/my/entry/file.js', // 엔트리 파일 경로
output: {
path: path.resolve(\_\_dirname, 'dist'), // 출력 디렉토리
filename: 'my-first-webpack.bundle.js' // 출력 파일 이름
},
module: {
rules: [
{
test: /\.css$/, // CSS 파일을 처리
use: ['style-loader', 'css-loader'] // style-loader와 css-loader 사용
}
]
},
plugins: [
new HtmlWebpackPlugin({
template: './src/index.html', // HTML 템플릿 파일 경로
filename: 'index.html' // 생성할 HTML 파일 이름
})
],
};

````

- **Entry** : 모듈의 시작점으로 번들링할 모듈의 시작점을 지정해줄 수 있다.
- **Output** : 모든 모듈을 하나로 합쳐서 번들된 파일이 저장될 경로를 지정해 줄 수 있다.
- **Loaders** : 로더를 이용하여 자바스크립트 파일 이외에 CSS나 이미지 등의 다양한 파일을 로드할 수 있도록 로더를 통해 이런 파일들을 자바스크립트 코드로 변환하는 작업을 수행할 수 있다.
- **Plugins** : 번들 파일 관련된 후처리를 담당하여 로더가 수행할 수 없는 다양한 기능을 수행할 수 있다. 예를 들어 번들 최적화, 자산관리, 환경주입 과 같은 일을 진행할 수 있다.

{:.notice--primary}
💡 **`플러그인이 수행하는 주요 작업`**<br><br>
◾ **번들 최적화 (Bundle Optimization)**: 플러그인은 최종 번들 파일의 크기를 줄이고 성능을 향상시키기 위해 코드 스플리팅, 트리 쉐이킹, 압축 등의 최적화 작업을 수행한다.
◾ **자산 관리 (Asset Management)**: 플러그인은 이미지, 폰트, 스타일시트 등과 같은 자산을 관리하고, 이들을 최적화하여 번들에 포함시키거나 외부 CDN에 배포하는 등의 작업을 한다.
◾ **환경 주입 (Injection of Environment)**: 플러그인은 빌드 시점에 환경 변수를 주입하여, 개발, 테스트, 프로덕션 환경에 따라 다른 설정이나 API 키 등을 사용할 수 있도록 한다.


<br><br><br>
## 폴리필 (Polyfill) 이란?


> 최신 JavaScript 기능이나 API가 지원되지 않는 환경에서 해당 기능을 사용할 수 있도록 해주는 코드 조각이다. 예를 들어, Promise, Array.prototype.includes와 같은 ES6 기능은 구형 브라우저에서 지원되지 않지만, Polyfill을 통해 이러한 기능을 사용할 수 있게 된다.
다시말해서, 트랜스파일러로 ES6 문법을 ES5 문법으로 변환한다고 해도
ES5 문법에 없던 메서드나 새로운 객체 새로운 함수는 만들어줄 수 없기 때문에
구형 브라우저에서 사용할 수 있게 해주는 코드 뭉치인 Polyfill로 부족함을 채울 수 있는 것이다.
Polyfill은 주로 core-js와 같은 라이브러리 형태로 제공되고
필요한 기능을 선택적으로 사용할 수 있다.
>


```jsx
import 'core-js/features/array/includes';

const array = [1, 2, 3];
console.log(array.includes(2)); // true
console.log(array.includes(4)); // false
````

{:.notice--primary}
💡 **`최신 브라우저에서 불필요한 Polyfill 스크립트 로딩`**<br><br>
위의 코드와 같이 설정을 하게 되면  
최신 버전의 브라우저에서는 대부분의 ECMAScript 표준 객체와 메서드가 포함되어있음에도 불구하고 불필요한 Polyfill 스크립트를 내려받게된다.  
이럴 경우에는 선택적으로 불러올 수 있는 방법을 찾아야한다.

<br><br><br>

## **트랜스파일러 (transpiler) 란?**

> 코드를 변환하는 도구로  
> JavaScript의 ES6 문법을 ES5 문법으로 변환하거나,  
> React의 JSX 및 Typescript 코드를 브라우저가 이해할 수 있는 Javascript로 변환하는 도구이다.  
> Transpiler 덕분에 여러 브라우저 호환성을 유지하면서 다양한 문법을 활용할 수 있다.  
> Transpile은 주로 빌드 과정에서 수행된다.

<br>
### Babel
  
  
> Babel은 최신 JavaScript 문법을 구형 브라우저에서도 호환되도록 변환해주는 트랜스파일러로  
ES6, ES7 버전의 JavaScript 코드를,   
ES5버전의 코드로 바꾸어주는 Node.js 패키지이다.  
>
  
  
```jsx
// Babel Input: ES2015 arrow function
[1, 2, 3].map(n => n + 1);

// Babel Output: ES5 equivalent
[1, 2, 3].map(function(n) {
return n + 1;
});

````

**핵심 사용법**


자세한 사용법은 → https://babeljs.io/docs/usage

> 아래와 같은 Babel 라이브러리를 조합해서
최신 JavaScript, TypeScript, Flow, React 코드를 구형 브라우저에서도 호환되도록 변환할 수 있다.
>

- `@babel/core` : 바벨의 핵심기능이 담겨있는 기본 라이브러리로,
JavaScript 코드를 변환하는 기본 엔진 역할을 한다.

    ```bash
    npm install @babel/core
    ```

- `@babel/cli` : 커맨드 라인에서 바벨을 실행할 수있는 라이브러리로,
커맨드 라인 인터페이스(CLI)를 제공하여 터미널에서 Babel을 실행할 수 있게 해준다.

    ```bash
    npm install @babel/cli
    ```

- `@babel/preset-env` : Babel 플러그인 설치 명령어로,
최신 JavaScript 문법을 구형 브라우저에서 사용할 수 있도록 변환하는 프리셋이다.
사용자가 지정한 브라우저 환경에 맞춰 필요한 변환만 수행한다.

    ```bash
    npm install @babel/preset-env (기본 js코드 검사)

    npm install @babel/preset-flow (플로우 코드 검사)

    npm install @babel/preset-typescript (타입스크립트 코드 검사)

    npm install @babel/preset-react (리액트 코드 검사)
    ```


- Babel에서 Polyfill을 사용하는 방법

    ```bash
    npm install core-js
    npm install regenerator-runtime
    ```

    ```jsx
    import "core-js/stable";
    import "regenerator-runtime/runtime";
    ```

    ```jsx
    module.exports = {
      presets: [
        [
          "@babel/preset-env",
          {
            useBuiltIns: "entry",
            corejs: 3 // core-js의 버전
          }
        ]
      ]
    };
    ```

    - `최신 브라우저에서 불필요한 Polyfill 스크립트 로딩` 하는 문제에 대한 해결 방법

        > @babel/preset-env `Smart Preset`을 사용해서
        Smart Preset이 이미 정의된 브라우저 목록에 따라서 자동으로 필요 없는 Polyfill을 제거해서 해결할 수 있다.
        아래와 같이 @babel/preset-env에 브라우저 지원 범위를 설정하면
        Polyfill을 안정적으로 포함하면서 스크립트의 크기를 감축할 수 있다.
>


        ```jsx
        module.exports = {
        	presets: [
            	['@babel/preset-env', { targets: { ie: 11 } }],
            ]
        /* 그 외의 설정 값들 */
        }
        ```


- Webpack에서 Babel을 로더를 통해 사용하는 방법

    ```jsx
    const path = require('path');

    module.exports = {
      entry: './src/index.js', // 진입점
      output: {
        filename: 'bundle.js', // 출력 파일 이름
        path: path.resolve(__dirname, 'dist'), // 출력 경로
      },
      module: {
        rules: [
          {
            test: /\.js$/, // .js 파일에 대해
            exclude: /node_modules/, // node_modules 제외
            use: {
              loader: 'babel-loader', // Babel 로더 사용
              options: {
                presets: ['@babel/preset-env'], // 사용할 프리셋
              },
            },
          },
        ],
      },
    };
    ```


<br><br><br>
## 코드 린터 (Linter) 및 포맷터 (formatter) 란?


> 코드 린터와 포맷터는 소프트웨어 개발에서
코드 품질을 유지하고 일관성을 확보하기 위해 사용되는 도구로
코드 린터는 소스 코드에서 문법 오류, 스타일 문제, 잠재적인 버그 등을 찾아내는 도구로
`ESLint` 가 주로 사용되고
코드 포맷터는 소스 코드를 자동으로 정리하고 일관된 형식으로 변환하는 도구로
`Prettier` 가 주로 사용된다.
>


<br>
### **ESLint**


> 자바스크립트 코드에서 발견된 문제 패턴을 식별하기 위한 정적 코드 분석 도구로
자바스크립트 문법에서 에러가 발생하면 표시해서 코드 퀄리티를 일관적으로 유지할 수 있게 해준다.
대부분의 프로그래밍 언어에는 컴파일하는 과정에서 수행되는 Linter가 기본적으로 내장되어 있지만,
인터프리터 언어인 자바스크립트는 Linter가 내장되어 있지 않다.
런타임 환경에서 에러가 발생할 확률이 높기 때문에
ESLint와 같은 Linting 도구를 사용한다.
>


**핵심 사용법**


자세한 사용법은 → https://eslint.org/docs/latest/use/getting-started#main

- 코드 스타일 규칙 정의

    > 들여쓰기, 괄호 사용, 세미콜론 사용 여부 등 다양한 스타일 규칙을 프로젝트에 맞게 설정할 수 있다.
>


    ```json
    {
      "rules": {
        "indent": ["error", 2], // 들여쓰기 2칸
        "quotes": ["error", "double"], // 쌍따옴표 사용
        "semi": ["error", "always"] // 세미콜론 필수
      }
    }
    ```

- 규칙 선택 및 설정

    > 프로젝트에 필요한 규칙을 선택하고, 각 규칙의 수준을 설정할 수 있다.
>

    - **오류(error)**: 코드의 문제점을 나타내며, 반드시 수정해야 하는 것을 의미.
    - **경고(warn)**: 코드의 잠재적인 문제점을 나타내며, 수정하는 것이 좋다는 의미.
    - **정보(off)**: 해당 규칙을 사용하지 않는다는 의미.

    ```json
    {
      "rules": {
        "no-unused-vars": "warn", // 사용하지 않는 변수 경고
        "no-console": "error", // console.log 사용 오류
        "eqeqeq": "error" // ===, !== 사용 필수
      }
    }
    ```

- 확장 설정

    > ESLint는 다양한 스타일 가이드(Airbnb, Standard 등)를 제공하는데
    이러한 스타일 가이드를 확장해서 프로젝트에 적용할 수 있다.
>


    ```json
    {
      "extends": ["airbnb-base"] // Airbnb 스타일 가이드 확장
    }
    ```

- 플러그인 사용

    > 다양한 플러그인을 통해 기능을 확장할 수 있는데
    React, Vue.js 등 특정 프레임워크나 라이브러리에 대한 규칙을 추가할 수 있다.
>


    ```json
    {
      "plugins": ["react"], // React 플러그인 사용
      "extends": ["airbnb-base", "plugin:react/recommended"] // React 규칙 확장
    }
    ```

- 무시 설정

    > 특정 파일이나 디렉토리, 특정 코드 블록에 대한 ESLint 검사를 무시하도록 설정할 수 있다.
>


    ```json
    node_modules/
    dist/
    *.test.js
    ```

- 자동 수정 기능

    > `--fix` 옵션을 통해 일부 문제점을 자동으로 수정해주는 기능을 제공하는데
    이를 통해 코드 수정에 소요되는 시간을 절약할 수 있다.
>


    ```bash
    eslint --fix src/**/*.js
    ```


<br>
### Prettier


> 일관적인 **코드 스타일**을 유지할 수 있게 도와주는 도구로,
줄 바꿈, 공백, 들여 쓰기 등과 같은 스타일을 교정을 통해 일관성 있는 코드 스타일을 유지할 수 있게 해준다.
>


**핵심 사용법**


자세한 사용법은 → https://prettier.io/docs/install

- 코드 스타일 규칙 정의

    > 들여쓰기, 괄호 사용, 따옴표 사용 여부 등 다양한 스타일 규칙을 프로젝트에 맞게 설정할 수 있다.
>


    ```json
    module.exports = {
      semi: true, // 세미콜론 사용
      trailingComma: 'es5', // 후행 쉼표 ES5 스타일
      singleQuote: true, // 작은따옴표 사용
      printWidth: 80, // 한 줄 최대 문자 수
      tabWidth: 2, // 탭 너비 2칸
      useTabs: false, // 탭 대신 스페이스 사용
      bracketSpacing: true, // 객체 리터럴의 괄호 안에 공백 추가
    };
    ```

- Editor 연동

    > Visual Studio Code, Sublime Text 등 개발 에디터와 연동하면
    코드 작성 중에 실시간으로 코드를 포맷팅할 수 있다.
>

- ESLint와 함께 사용

    > Prettier와 ESLint를 함께 사용해서 코드 스타일과 코드 품질을 모두 관리할 수 있다.
>


    ```json
    module.exports = {
      // ...
      extends: ['prettier'], // Prettier 규칙 확장
      plugins: ['prettier'], // Prettier 플러그인 사용
      rules: {
        'prettier/prettier': 'error', // Prettier 규칙 오류로 설정
      },
    };
    ```

- Git Hook과 함께 사용

    > Git Hook(명령어 실행 전후에 뭔가를 더 실행할 수 있는 방법)을 사용하면
    코드를 커밋하기 전에 Prettier를 실행하여 자동으로 코드를 포맷팅할 수 있다.
    커밋생성 직전에 린트가 실행되고 오류를 발견하면 커밋 과정은 취소된다.
    무조건 린트에 통과해야만 코드가 커밋생성 되도록 설정한 것이다.
>

    1. husky 설치

        ```bash
        npm install husky
        ```

    2. husky 를 설치하고 package.json에 설정을 추가한다.

        ```json
        "husky": {
        	"hooks": {
        		"pre-commit": "echo \"커밋전에 출력\""
        	}
        }

        ```

    3. lint 명령어를 바꿔준다.

        ```json
        "husky": {
        	"hooks": {
        		"pre-commit": "npm run lint"
        	}
        }

        ```


<br><br><br>
## **package.json 파일에서 중요한 필드**


package.json 파일에 들어가면 `dependencies`와 `devDependencies`를 볼 수 있다.

그 중에 devDependencies에는 애플리케이션 동작과 직접적인 연관은 없지만, 
개발할 때 필요한 라이브러리이 들어가게 된다.

**dependencies (의존성)**


> 프로젝트가 실행되기 위해 반드시 필요한 패키지를 명시하는 필드로
애플리케이션 배포 시 node_modules 폴더에 포함되어 함께 배포된다.
애플리케이션이 정상적으로 작동하는 데 필요한 핵심 라이브러리, 프레임워크, 유틸리티 등을 여기에 포함한다.
>


```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "axios": "^1.2.0"
  }
}
````

**devDependencies (개발 의존성)**

> 프로젝트를 개발하는 과정에서만 필요한 패키지를 명시하는 필드로  
> 애플리케이션 배포 시 node_modules 폴더에서 제외되어 배포되지 않는다.  
> 테스트 도구, 빌드 도구, 코드 분석 도구 등을 여기에 포함된다.

```json
{
  "devDependencies": {
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "eslint": "^8.0.1"
  }
}
```

dependencies와 devDependencies의 주요 차이점

| 구분        | dependencies                                        | devDependencies                                                   |
| ----------- | --------------------------------------------------- | ----------------------------------------------------------------- |
| 용도        | 애플리케이션 실행 시 필요한 패키지                  | 개발 과정에서만 필요한 패키지                                     |
| 포함 여부   | 배포 시 포함                                        | 배포 시 제외                                                      |
| 설치 명령어 | `npm install <패키지명>` 또는 `yarn add <패키지명>` | `npm install <패키지명> --save-dev` 또는 `yarn add <패키지명> -D` |

- eslint, prettier 등 코드를 작성할 때 사용되는 라이브러리들은 사용자와 만날 이유가 없다.
- build 할 때는 의존성에서 제외함으로써 최종 결과물의 크기를 줄일 수 있다면 좋을 것이다.  
  즉, devDependencies에 저장하여 로컬 개발 단계에서만 이용하면 된다.

**private**

프로젝트가 npm 패키지 저장소에 발행될 수 있는지 여부를 지정하는 필드로 기본값이 false이다.  
이 값을 명시하지 않으면 실수로 패키지가 업로드될 수 있으므로,  
보안상 npm에 올리지 말아야 할 프로젝트에서는 private을 true로 설정하는 것이 좋다.

```json
{
  "private": true
}
```

<br><br><br>

## 패키지 매니저 (Package Manager) 란?

> JavaScript 프로젝트에서 패키지 관리는 필수적으로  
> 패키지 매니저는 패키지 다운로드, 업데이트, 의존성 관리 등 복잡한 과정을 효율적으로 수행하도록 도와준다.  
> 위에서 다룬 webpack, Linter, Formatter와 같은 도구들은  
> 일반적으로 npm이나 yarn과 같은 패키지 매니저를 통해 설치된다.

<br>
### npm (Node Package Manager)
  
  
> Node.js의 기본 패키지 관리자로, JavaScript 생태계에서 가장 널리 사용되는 패키지 매니저이다.  
>
  
  
**장점**  
  
- **접근성**: Node.js와 함께 기본적으로 제공되므로 별도의 설치가 필요하지 않다.  
- **광범위한 생태계**: JavaScript 생태계에서 가장 큰 패키지 저장소를 보유하고 있어 다양한 패키지를 쉽게 찾을 수 있다.  
  
**단점**  
  
- **버전 불일치 문제**: 프로젝트에 사용되는 패키지의 버전이 명확하게 고정되지 않아, 개발 환경에 따라 패키지 버전이 달라질 수 있다.  
- **설치 순서 문제**: 패키지 설치 순서가 보장되지 않아 예기치 않은 문제가 발생할 수 있다.  
- **느린 설치 속도**: 패키지를 한 번에 하나씩 순차적으로 설치해서 설치 시간이 오래 걸릴 수 있다.  
- **중복된 패키지**: 의존성 트리가 복잡해질 경우, 중복된 패키지가 설치될 수 있어 node_modules의 크기가 증가하고 관리가 어려워질 수 있다.  
  
<br>
### yarn
  
  
> yarn은 npm의 단점을 개선하기 위해 개발된 패키지 매니저로  
npm과 호환되면서도 더 빠르고 안정적인 패키지 관리를 제공한다.  
>
  
  
**장점**  
  
- **빠른 설치 속도**: 패키지를 병렬로 다운로드하여 설치 시간을 단축시켰다.  
- **안정적인 버전 관리**: yarn.lock 파일을 통해 패키지 버전을 고정해서 개발 환경을 일관성 있게 유지한다.  
- **checksum 검사**: 패키지 설치 과정에서 checksum을 사용하여 파일 무결성을 검사한다.  
- **캐시 기능**: 다운로드한 패키지를 캐시에 저장하여 재설치 시 시간을 단축시킨다.  
  
**단점**  
  
- **추가 설치 필요**: npm과 달리 별도로 설치해야 한다.  
- **중복된 패키지**: 의존성 트리가 복잡해질 경우, 중복된 패키지가 설치될 수 있어 node_modules의 크기가 증가하고 관리가 어려워질 수 있다.  
  
{:.notice--primary}
💡 **`npm과 yarn의 문제점`**<br><br>
◾ **무거운 node_modules**: node_modules 폴더가 너무 커서 프로젝트 용량을 증가시키고, 설치 시간도 오래 걸린다.  
◾ **유령 의존성**: 설치하지 않은 패키지가 의존성 때문에 함께 설치되는 현상이 발생한다.  
  
  
<br>
### yarn berry (yarn v2+)
  
  
> yarn berry는 yarn v2 이상을 지칭하고  
기존 yarn의 문제점을 해결하기 위해 새로운 방식을 도입했다.  
>
  
  
```bash
npm install -g yarn
yarn set version berry
```
  
- **Plug'n'Play (PnP)**: 용량을 줄이고 설치 속도를 높이고 중복 설치를 방지  
    - Yarn Berry는 `node_modules` 대신 패키지 정보를 `.zip` 파일로 압축해서 `.yarn/cache` 폴더에 저장한다.  
    - 각 패키지의 의존성 정보는 `.pnp.cjs` 파일에 기록된다.  
    - `.pnp.cjs`는 의존성 트리를 중첩된 맵 형태로 표현해서 디스크 I/O 없이도 패키지의 위치와 의존성을 빠르게 파악할 수 있도록 한다.  
      
    ```json
    /* react 패키지 중에서 */
    ["react", [
      /* npm:17.0.1 버전은 */
      ["npm:17.0.1", {
        /* 이 위치에 있고 */
        "packageLocation": "./.yarn/cache/react-npm-17.0.1-98658812fc-a76d86ec97.zip/node_modules/react/",
        /* 이 의존성들을 참조한다. */
        "packageDependencies": [
          ["loose-envify", "npm:1.4.0"],
          ["object-assign", "npm:4.1.1"]
        ],
      }]
    ]],
    ```
      
  
- **Zero Install**: 저장소를 복제하거나 브랜치를 전환할 때 별도의 설치 과정 없이 바로 사용가능  
    - Yarn Berry는 패키지 의존성을 압축 파일 형태로 관리하므로, 의존성 용량이 작고 파일 수도 적다.  
    따라서 의존성을 Git 저장소에 포함시켜 버전 관리를 할 수 있고 (zero-install)  
    git clone 이후 별도의 설치 필요없이 바로 사용할 수 있다.  
    - CI에서 의존성을 설치하는 시간을 크게 절약할 수 있다.  
    - 오프라인 캐시 기능으로 인터넷이 연결되지 않아도 패키지를 사용할 수 있다.  
  
<br>
### 패키지 매니저 비교
  
| 기능 | npm | yarn | yarn berry |  
| --- | --- | --- | --- |  
| 기본 제공 | O | X | X |  
| 설치 속도 | 느림 | 빠름 | 매우 빠름 |  
| 버전 관리 | 느슨함 | yarn.lock | yarn.lock |  
| 안정성 | 낮음 | 높음 | 높음 |  
| 편의성 | 높음 | 높음 | 높음 |  
| 추가 기능 | - | checksum, 캐시 | PnP, Zero Install |  
  
<br>
### 패키지 매니저의 선택기준
  
  
- **npm**: JavaScript 생태계에 익숙하고 기본적인 기능을 원하는 경우  
- **yarn**: 빠른 설치 속도와 안정적인 버전 관리를 원하는 경우  
- **yarn berry**: node_modules 용량 문제와 Zero Install 기능을 활용하고 싶은 경우  
  
<br><br><br>
**reference**
  
  
- <a href='https://www.youtube.com/watch?v=o-5K5Sc7L1k' target='_blank'>https://www.youtube.com/watch?v=o-5K5Sc7L1k</a>
  
- <a href='https://www.youtube.com/watch?v=xLziDinqBj0&t=417s' target='_blank'>https://www.youtube.com/watch?v=xLziDinqBj0&t=417s</a>
  
- <a href='https://www.youtube.com/watch?v=Ds7EjE8Rhjs&t=704s' target='_blank'>https://www.youtube.com/watch?v=Ds7EjE8Rhjs&t=704s</a>
  
- <a href='https://onlydev.tistory.com/163' target='_blank'>https://onlydev.tistory.com/163</a>
  
- <a href='https://velog.io/@hozzijeong/WebPack%EA%B3%BC-Babel-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Polyfill%EC%97%90-%EB%8C%80%ED%95%B4' target='_blank'>https://velog.io/@hozzijeong/WebPack%EA%B3%BC-Babel-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Polyfill%EC%97%90-%EB%8C%80%ED%95%B4</a>
  
- <a href='https://velog.io/@treejy/React-ESLint%EC%99%80-Prettier%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80' target='_blank'>https://velog.io/@treejy/React-ESLint%EC%99%80-Prettier%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%98%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80</a>
  
- <a href='https://www.daleseo.com/js-package-json/' target='_blank'>https://www.daleseo.com/js-package-json/</a>
