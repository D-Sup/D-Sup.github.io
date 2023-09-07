---
title: 'React에서의 event handling'
excerpt_separator: '<!--more-->'
categories:
  - React
tags:
  -
header:
  teaser: ./assets/image/36.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 이벤트 핸들링

> 웹 애플리케이션에서 사용자의 상호작용에 대응하기 위해  
> 이벤트(예: 클릭, 키보드 입력, 마우스 움직임 등)를 감지하고 처리하는 과정이다

💡 사용자의 액션에 반응하여 동적으로 동작하고  
&emsp;데이터를 업데이트하는 데 이벤트 핸들링이 중요한 역할을 한다
{:.notice--primary}

<br>

## HTML에서의 이벤트 핸들링

> 이벤트명은 소문자 **lowerCase** 로 작성되어야 한다

```html
<button onclick="handleButton()"></button>
```

<br>

> **false를 반환**하는 식으로 이벤트의 기본동작을 방지한다

```html
<a href="#" onclick='console.log("The link was clicked."); return false;' />
```

<br>

## React에서의 이벤트 핸들링

> lowerCase가 아닌 **camelCase**로 작성되어야 한다  
> 또한, 문자열의 형태가 아닌 **함수 그 자체로 핸들러를 전달**한다

```jsx
<button onClick={handleButton}></button>
```

<br>

> HTML에서 false를 반환하는 식으로 이벤트의 기본동작을 방지하지만,  
> React에서는 `event.preventDefault` 를 명시적으로 호출**하여 이벤트의 기본동작을 막아야한다

```jsx
<form onSubmit={e => {
  e.preventDefault();
  alert('제출되었습니다!');
}}>
```

<br>

## DOM 이벤트 종류

🔎 `onClick` : Element를 클릭했을 때  
<br>
🔎 `onChange` : Element의 내용이 변경되었을 때(input의 텍스트를 변경, 파일 선택 등)  
<br>
🔎 `onKeyDown`, `onKeyUp`, `onKeyPress` : 키보드 입력이 일어났을 때  
<br>
🔎 `onDoubleClick` : Element를 더블 클릭했을 때  
<br>
🔎 `onFocus` : Element에 Focus 되었을 때  
<br>
🔎 `onBlur` : Element가 Focus를 잃었을 때  
<br>
🔎 `onSubmit` : Form Element에서 Submit 했을 때
{:.notice--info}

<br>

## 이벤트 핸들링 예제

### 별도의 핸들링 함수를 선언

```jsx
const App = () => {
  const handleClick = () => {
    alert("클릭했습니다.");
  }
  return (
    <div>
        <button onClick={handleClick}>클릭하세요</button>
    </div>
  );
};
```

💡 컴포넌트 내부 또는 외부에서 함수를 선언하고 해당 함수 내부에 로직을 구현해서   
&emsp;DOM 이벤트 속성의 이벤트 핸들러 함수로 정의할 수 있다  
{:.notice--primary}

<br>

### 익명 함수로 처리

```jsx
const App = () => {
  return (
    <div>
      <button onClick={() => { alert('클릭했습니다.') }}>클릭하세요</button>
    </div>
  )
}
```
💡 이벤트 핸들러를 인라인으로 정의할 때는 익명함수를 래핑해야한다  
<br>
💡 DOM 이벤트 속성을 정의하는 부분에서   
&emsp;콜백에 화살표 함수로 익명 함수 문법을 사용하여  
&emsp;이벤트 핸들러를 인라인으로 정의할 수 있다
{:.notice--primary}

<br>

### 주의점  

> 함수가 이벤트 핸들러로 전달되는 것이기 때문에  
> 함수가 전달되면 React가 이것을 기억하고 있다가  
> 사용자에 의해서 이벤트가 발생하면 함수를 호출하도록 지시한다  

```jsx
<button onClick={handleClick()}> X
<button onClick={handleClick}> O
```

⚠️ 즉, DOM 이벤트 속성을 정의할 때 함수를 호출하지 않도록 작성해야한다  
&emsp;이벤트 핸들러는 호출하는 것이 아닌 전달하는 것이라는 걸 기억하자  
{:.notice--warning}

<br>

### 이벤트 핸들러를 props로 전달하기

```jsx
const Form = ({ onChange }) => {
  return (
    <div>
      <input onChange={onChange} />
    </div>
  )
}

const App = () => {
  const [username, setUsername] = useState('')
  function handleChange(event) {
    setUsername(event.target.value);
  }
  return (
    <div>
      <h1>{username}</h1>
      <Form value={username} onChange={handleChange}/>
    </div>
  )
}
```

💡 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없기때문에  
&emsp;부모 컴포넌트가 자식의 이벤트 핸들러를 지정하고 싶을 때에는   
&emsp;부모로부터 받은 프로퍼티를 이벤트 핸들러로 전달하면 된다
{:.notice--primary}

<br>

### 이벤트 커스텀 

```jsx
const Button = ({ onDoubleClick, children }) => {
  const [count, setCount] = useState(0);

  const handleClick = () => {
      if(count >= 2) {
        onDoubleClick();
      }
      setCount(count + 1);
  }

  return <button onClick={handleClick}>{children}</button>
}
const App = () => {
  return (
      <div>
        <Button onDoubleClick={() => { alert("두번 클릭했습니다!"); }}>
          클릭하세요
        </Button>
      </div>
  );
};
```

💡 기본적으로 존재하는 이벤트를 확장시켜서   
&emsp;커스텀 동작을 추가하여 컴포넌트를 더 유연하고 재사용이 용이하게 만들 수 있다
{:.notice--primary}

<br>

## 이벤트 핸들링 중에 발생할 수 있는 문제

### 조건부 이벤트 처리

> 만약 여러 개의 요소나 컴포넌트가 동일한 이벤트 핸들러를 공유하고 있을 경우,  
> 특정 요소나 컴포넌트에 대한 액션을 취했을 때 예상치 못한 동작을 야기 할 수 있다

```jsx
const App = () => {
  function handleButtonClick(event) {
    const buttonId = event.currentTarget.dataset.id;
    if (buttonId === '확인') {
      ...
    } else if (buttonId === '취소') {
      ...
    }
  }
  return (
    <button data-id="확인" onClick={handleButtonClick}>확인</button>
    <button data-id="취소" onClick={handleButtonClick}>취소</button>    
  );
};
```

💡 `data-*` 속성과 같은 식별자를 사용하여 이벤트 핸들러에서 조건을 검사하고  
&emsp;조건에 따라 동적으로 이벤트 핸들러를 실행시킬 수 있다
{:.notice--primary}

<br>

### 이벤트 전파

> 이벤트 핸들러는 이벤트가 발생한 요소뿐만 아니라 그 부모 요소로도 이벤트가 전파가 된다  
> 즉, 이벤트 버블링이 발생하는데   
> 이벤트가 발생한 요소에서부터 부모 요소로 계속해서 이벤트가 전파된다

```jsx
const App = () => {
  function handleFocus {
    alert('포커스!')
  };
  function handleButtonClick() {
    alert('버튼 클릭')
  }
  return (
    <div onClick={handleFocus}>
      <button onClick={handleButtonClick}>확인</button>
      <button onClick={handleButtonClick}>취소</button>    
    </div>
  );
};
```

💡 아래 코드의 경우 `<div>` 안에 있는 버튼을 누를 경우 이벤트 전파로 인해   
&emsp;상위 요소인 `<div>` 의 이벤트 핸들러도 실행된다
{:.notice--primary}

<br>

```jsx
const App = () => {
  function handleFocus {
    alert('포커스!')
  };
  function handleButtonClick() {
    alert('버튼 클릭')
    event.stopPropagation()
  }
  return (
    <div onClick={handleFocus}>
      <button onClick={handleButtonClick}>확인</button>
      <button onClick={handleButtonClick}>취소</button>    
    </div>
  );
};
```

💡 이것을 해결하기 위해선, 이벤트 버블링을 중단하면 된다  
&emsp;이벤트 핸들러 내에서 `event.stopPropagation()` 을 이용하면  
&emsp;이벤트가 상위 요소까지 전파되지 않아 충돌 문제를 해결할 수 있다
{:.notice--primary}

<br>

## 질문

<details>
  <summary>✨ 리액트에서 이벤트 핸들링을 어떻게 구현하나요?</summary><br>
  컴포넌트 내부 또는 외부에서 함수를 선언하고 해당 함수 내부에서<br>
  상태를 변경하거나, 다른 함수를 호출하거나, 렌더링을 갱신하는 등의 로직을 구현해서<br>
  원하는 DOM 이벤트 속성의 이벤트 핸들러 함수로 정의함으로써<br>
  이벤트가 발생했을 때 이벤트 핸들러를 실행시킬 수 있습니다.<br>
</details>

<br>

<details>
  <summary>✨ HTML과 React 이벤트 처리의 차이점은 무엇인가요?</summary><br>
  HTML에서는 이벤트 핸들러 이름은 lowerCase 로 작성하고<br>
  기본동작을 막기 위해서 return false를 반환할 수 있습니다.<br> 
  React에서는  이벤트 핸들러 이름을 camelCase 로 작성하고<br>
  문자열의 형태가 아닌 함수 그 자체로 핸들러를 전달합니다.<br>
  또한 preventDefault 메서드를 명시적으로 호출하여 이벤트의 기본동작을 막아야합니다.<br>
</details>

<br>

<details>
  <summary>✨ 이벤트 핸들링 중에 발생한 버그나 문제를 어떻게 해결했나요?</summary><br>
  sns 프로젝트에서 키워드를 검색하고 선택할 수 있는 페이지를 구현하는데,<br>
  여러 개의 버튼이 동일한 이벤트 핸들러를 공유하고 있어서,<br>
  버튼 중 하나를 클릭하면 다른 버튼도 예상치 못한 동작을 수행하는 문제가 발생했었습니다.<br>
  이 문제를 해결하기 위해 각 버튼에 data-* 속성과 같은 고유한 식별자를 부여하고,<br>
  이벤트 핸들러에서 조건을 검사하고 조건에 따라 동적으로 이벤트 헨들러를 실행시킬 수 있도록 했습니다.<br>
</details>

<br>
<br>

<span class='explain'>Reference</span> 

- [react.dev](https://react.dev/learn/responding-to-events)
- [lakelouise.tistory.com](https://lakelouise.tistory.com/261)
