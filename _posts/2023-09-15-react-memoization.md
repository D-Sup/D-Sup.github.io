---
title: 'React 에서의 Memoization'
excerpt_separator: '<!--more-->'
categories:
  - React
tags:
  -
header:
  teaser: ./assets/image/37.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 메모이제이션

_Memoization_

> 컴퓨터가 동일한 계산을 반복해야 할 때,   
> 이전에 계산한 값을 메모리에 저장함으로서   
> 동일한 계산의 **반복 수행을 최소화**하고 **재사용**하는 것을 말한다

💡 어떤 이점이 있을까?  
<br>
&emsp;**성능 향상** - 동일한 입력 값에 대한 결과를 캐시에 저장하여 이후에 재사용함으로써 애플리케이션의 반응성을 향상시킬 수 있다  
&emsp;**리소스 절약** - 계산 비용이 높은 작업을 반복 수행하지 않으면 시간과 메모리를 절약할 수 있다  
&emsp;**예측 가능성 증가** - 동일한 입력 값에 대한 함수나 연산의 결과가 항상 동일하다는 것을 보장할 수 있기 때문에 코드의 동작이 예측 가능해진다
{:.notice--primary}

<br>

## 리액트의 메모이제이션 구현

> 리액트에서는 메모이제이션을 위한 세가지 api를 제공한다

- `React.memo`  
- `useMemo`   
- `useCallback`


<br>

### `React.memo`
---

- **컴포넌트 자체를 메모이제이션**하는 방식으로, 컴포넌트의 메모이제이션을 위한 내장 함수이다  
- 리렌더링이 일어났을 때 React.memo()로 감싼 컴포넌트 스스로가 props 값의 변경 여부를 확인하고 처리한다  
- 만약 전 props와 새로운 props가 동일하면 DOM에서의 비교 작업,   
  즉 React가 이전 props와 새로운 props를 비교하는 작업이 생략된다  
- 다시말해, 컴포넌트가 리렌더링될 때 달라진 부분이 없다면   
  리액트는 리렌더링을 하지 않고 이전 결과를 재사용한다

```jsx
// 방법 1
const Result = React.memo(() => {
  return ()
})

// 방법 2
export default React.memo(Result)
```

💡 React.memo는 컴포넌트를 감싸서 사용하며,  
&emsp;들어온 props의 값이 바뀌었다면 해당 컴포넌트의 렌더링을 발생시킨다  
&emsp;따라서, props만 받는 자식 컴포넌트에서 사용하는 것을 권장한다  
{:.notice--primary}

<br>

### `useMemo`
---

> **메모이제이션 된 값을 반환**하는 hook이다  
> 첫번째 인자로 함수와 두번째 인자로 dependency 배열을 받는다  
> 렌더링하는 과정에서 두번째 인자 중에 하나라도 값이 변경되면 첫 번째 인자로 전달한 함수가 실행된다  
> 값이 바뀌지 않았다면 함수는 실행되지않고 이전에 계산한 결과가 그대로 사용된다 

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

<br>

### `useCallback`
---

> **메모이제이션 된 콜백을 반환**하는 hook이다  
> 자바스크립트에서 함수는 객체로 취급되기 때문에 리렌더링이 일어날 때마다 새로운 함수가 생성된다  
> 따라서 컴포넌트 내부에 있는 위치해있는 컴포넌트가 렌더링 될 때마다 다시 함수를 생성한다  
> useCallback으로 감싸주게 되면 첫 렌더할 때에만 함수를 생성하고 그 이후에는 함수를 기억하고 있어 재생성하지 않는다.  
> useMemo와 마찬가지로 두번째 인자로 dependency 배열을 받는다  
> 특정 값의 변화로 함수를 재생성 해야 한다면 그 값을 dependency 배열에 넣어주면 된다

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

💡 일반적으로 함수가 불필요하게 재생성되는 것을 방지하기 위해  
&emsp;dependency 배열을 빈 배열로 작성한다
{:.notice--primary}

<br>

```jsx
function App() {
  const [language, setLanguage] = useState('kr');

  const greetings = {
    en: 'Hello!',
    es: 'Hola!',
    kr: '안녕!'
  };

  const showMessage = useCallback(() => {
    return greetings[language]
  }, [language]); 

  return (
    <>
      <h1>{showMessage()}</h1>
      <button onClick={() => setLanguage('en')}>English</button>
      <button onClick={() => setLanguage('es')}>Spanish</button>
      <button onClick={() => setLanguage('kr')}>Korea</button>
    </>
  );
}
```

💡 그렇다면 함수를 재생성 해야 한다면 어떤 경우가 있을까..?  
<br>
&emsp;위 코드는 버튼을 클릭할 때 그에 맞는 인사말이 그려지게끔 구현한 코드인데   
&emsp;useCallback을 이용해서 language 상태가 변경 될때마다   
&emsp;showMessage 함수가 새로 생성되도록 설정해보았다
{:.notice--primary}

<span class='explain smallText'>이런식으로 사용하는 경우말고도 뭐가 있을까..? 잘 모르겠다</span>

<br>

## 예제코드

> 아래 코드는 키와 몸무게를 입력 받아 BMI 지수를 측정하는 코드이다  
> 이 코드를 봤을 때 메모이제이션을 고려한다면 어떤 부분에서 메모이제이션이 필요할까?

### _수정 전_


```jsx
export const Header = () => <h1>BMI 지수</h1>;

export const Input = ({ handleChange, value, children }) => {
  return (
    <>
      <label>{children}</label>
      <input onChange={handleChange} value={value}></input>
    </>
  )
}

const App = () => {
  const [weight, setWeight] = useState(0);
  const [height, setHeight] = useState(0);

  const handleWeight = (e) => setWeight(e.target.value);
  const handleHeight = (e) => setHeight(e.target.value);

  return(
    <>
      <Header/>
      <Input handleChange={handleWeight} value={weight}>몸무게 : </Input>
      <Input handleChange={handleHeight} value={height}>키 : </Input>
      <span>{`측정 결과 : ${weight / (height * 0.01) ** 2}`}</span>
    </>
  );
}
```

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/ca9fc45f-7f1d-4803-92f9-3fbfc885383b' alt=''>

⚠️ Input 컴포넌트가 의존하고 있는 props 나 state의 변화가 생길 때 렌더링이 일어나는데  
&emsp;상태값에 의존하지 않는 Header 컴포넌트까지도 렌더링이 일어나고 있다  
<br>
⚠️ BMI를 계산결과는 weight와 height 상태에 의존하고 있기 때문에  
&emsp;weight와 height가 변경되지 않아도 불필요한 계산이 될 수 있다  
<br>
⚠️ Input 컴포넌트는 handleWeight와 handleHeight 함수를 prop으로 전달받고 있다  
&emsp;따라서, handleWeight와 handleHeight 함수가 리렌더링될 때마다 새로 생성되기 때문에  
&emsp;Input 컴포넌트도 불필요하게 리렌더링 되어야하는 문제점이 있다
{:.notice--warning}

<br>

### _수정 후_

```jsx
export const Header = React.memo(() => <h1>BMI 지수</h1>);

export const Input = React.memo(({ handleChange, value, children }) => {
  return (
    <>
      <label>{children}</label>
      <input onChange={handleChange} value={value}></input>
    </>
  );
});

const App = () => {
  const [weight, setWeight] = useState(0);
  const [height, setHeight] = useState(0);

  // useCallback을 사용하여 함수를 메모이제이션
  const handleWeight = useCallback((e) => setWeight(e.target.value), []);
  const handleHeight = useCallback((e) => setHeight(e.target.value), []);

  // useMemo를 사용하여 BMI 계산 결과를 캐싱
  const bmiResult = useMemo(() => {
    return weight / (height * 0.01) ** 2;
  }, [weight, height]);

  return (
    <>
      <Header />
      <Input handleChange={handleWeight} value={weight}>몸무게 : </Input>
      <Input handleChange={handleHeight} value={height}>키 : </Input>
      <div>{`측정 결과 : ${bmiResult}`}</div>
    </>
  );
};
```

✔️ React.memo를 사용하면 컴포넌트들은 props가 변경되지 않는 한 같은 결과를 반환하고, 리렌더링을 방지할 수 있다  
<br>
✔️ useMemo를 사용하여 BMI 계산 결과를 캐싱하면 weight와 height가  
&emsp;변경되지 않는 한 같은 결과를 반환하고, 불필요한 계산을 방지할 수 있다  
<br>
✔️ useCallback을 사용하면 렌더링마다 새로운 함수를 생성하지 않고, 함수를 재사용하여 성능을 최적화할 수 있다
{:.notice--success}

⚠️ 하지만, 성능 최적화와 리렌더링을 최소화하기 위한 연습이라   
&emsp;이 코드에 메모이제이션을 고려한다고해서 큰 이점을 얻기는 힘들다 
{:.notice--warning}

<br>

## 잘못된 메모이제이션 사용

- **오버 사용** - 메모이제이션을 사용해야 할 필요가 없는 간단한 연산이나 데이터에 메모이제이션을 적용하면 오히려 추가적인 오버헤드를 발생시킬 수 있다


- **메모리 누수** - 메모리 누수가 발생할 수 있습니다. 
  캐시된 데이터가 오래 사용되지 않는 경우에도 계속해서 메모리에 남아있게 되어 메모리 사용량이 계속해서 증가할 수 있다

- **부작용 문제** - 메모이제이션된 함수 내에서 외부 상태를 변경하는 경우, 예상치 못한 부작용을 유발할 수 있다

💡 이러한 문제를 방지하기 위해 메모이제이션은   
&emsp;주로 계산 **비용이 높은 연산**이나 **의존성이 변경되지 않는 데이터**에 적용하는 것이 좋고  
&emsp;또한, 부작용이 없는 **순수(pure)한 함수**에만 메모이제이션을 적용하는 것이 중요하다
{:.notice--primary}

<br>

## 요약

- `React.memo`는 컴포넌트가 변경되지 않았을 때 이전 렌더링 결과를 재사용하는 목적
- `useMemo`는 함수의 연산량이 많을 때 이전 결과값을 재사용하는 목적
- `useCallback`은 함수가 재생성 되는 것을 방지하기 위한 목적

<br>

# 질문

<details>
  <summary>✨ 리액트에서 메모이제이션은 언제 사용되고 어떻게 구현할 수 있나요?</summary><br>
  메모이제이션은 주로 렌더링 성능 최적화를 위해 사용됩니다.<br>
  컴포넌트나 값, 함수의 중복 계산을 방지하고 불필요한 렌더링을 최소화하여 애플리케이션의 성능을 향상시키는데 활용됩니다.<br>
  React.memo를 사용하여 컴포넌트 메모이제이션을, <br>
  useMemo를 사용하여 값 메모이제이션을, <br>
  그리고 useCallback을 사용하여 함수 메모이제이션을 구현 할 수 있습니다.
</details>

<br>

<details>
  <summary>✨ useMemo와 useCallback의 차이점은 무엇인가요?</summary><br>
  useMemo와 useCallback의 차이점은 반환 값입니다. <br>
  useMemo는 값을 반환하고, useCallback은 메모이제이션된 함수를 반환합니다. <br>
  useMemo는 애플리케이션 성능을 향상시키기 위해 <br>
  함수나 연산 결과의 중복 계산을 방지하는 데 사용됩니다. <br>
  예를 들어, 복잡한 계산 또는 비용이 많이 드는 작업의 결과를 캐싱하여 <br>
  동일한 계산을 반복하지 않고 재사용할 수 있습니다.<br>
  반면에 useCallback은 함수의 재생성을 방지하여 불필요한 렌더링을 최소화합니다. <br>
  특히 자식 컴포넌트에 콜백 함수를 전달할 때 유용하며, <br>
  의존성 배열에 포함된 값들이 변경되지 않으면 함수를 동일하게 유지합니다.
</details>

<br>

<details>
  <summary>✨ 메모이제이션을 잘못 사용하면 어떤 문제가 발생할 수 있나요?</summary><br>
  메모이제이션을 잘못 사용하면 성능 최적화 목적으로 의도한 것과는 반대로 문제가 발생할 수 있습니다. <br>
  예를 들어, 메모이제이션된 값이 오래된 채로 남아 있으면 메모리 누수가 발생할 수 있으며, <br>
  올바른 의존성 배열을 설정하지 않으면 원하지 않는 렌더링이 발생할 수 있습니다. <br>
  또한, 과도한 최적화나 부작용이 있는 값 또는 함수를 메모이제이션하면 예상치 못한 동작을 유발할 수 있습니다. <br>
  따라서 메모이제이션을 사용할 때는 주의가 필요하며, <br>
  성능 최적화가 필요한 상황에서만 적절하게 활용하는 것이 중요합니다.;
</details>

<br>
<br>

<span class='explain'>Reference</span> 

- [thisblogfor.me](https://thisblogfor.me/react/hooks_memoization/)
- [yceffort.kr](https://yceffort.kr/2020/11/react-memoization)