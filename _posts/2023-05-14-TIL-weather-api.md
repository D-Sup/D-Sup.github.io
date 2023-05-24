---
title: '[동료특강] React로 날씨 Open API 다루기'
excerpt_separator: '<!--more-->'
categories:
  - React
tags:
  -
header:
  teaser: ./assets/image/29.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 📚 What is TIL?

## Open API 선정

> 현위치의 날씨와 일기예보 정보를 얻기위해 아래 두 가지 API를 이용해볼 것이다

- [현재 날씨 API](https://openweathermap.org/current)
- [5일치 일기예보 API](https://openweathermap.org/forecast5)

```js
api.openweathermap.org/data/2.5/weather?q={city name}&appid={API key} // 현재 날씨 API

api.openweathermap.org/data/2.5/forecast?q={city name}&appid={API key} // 5일치 일기예보 API
```


🔎 각 API의 호출법을 살펴보면 `?` 부분 부터 key와 value 형태로 데이터를 서버에 요청하는 **쿼리스트링 방식**임을 알 수 있다  
<br>
🔎 이런식으로 서버에 요청을하면 q 라는 매개변수를 통해 서버에서 데이터를 조회하고   
&emsp;조회한 결과를 다시 클라이언트에게 반환해줘서 원하는 데이터를 받을 수 있다
{:.notice--info}

💡 **fetch 함수**를 사용해서 데이터를 요청할 것인데 우선 city name과 API key를 변수로 할당받아 URL을 수정해보자
{:.notice--primary}

<br>
<br>

## fectch로 데이터 불러오기

```js
import React from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';

  const getWeather = () => {
    fetch(`https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`)
      .then(res => res.json())
      .then(data => {
        console.log(data);
      });
  };

  getWeather();

  return <div>App</div>;
}

export default App;
```
{: .language-js .line-numbers data-line=4, 5, 7, 8, 9, 10, 11, 12, 13, 15}

💡 그 다음 코드를 좀 더 간결하게 **async await**를 통해 변환해보자
{:.notice--primary}

<br>
<br>

## async await로 변환하기

```
import React from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    console.log(result);
  };

  getWeather();

  return <div>App</div>;
}

export default App;
```
{: .language-javascript .line-numbers data-line="7, 8, 11, 12"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/a299c172-d0e7-430d-aba6-836a36eff398' alt=''>

<span class='explain smallText'>데이터를 잘 받아온 모습</span>

💡 이제 받아온 데이터를 **useState Hook**을 활용해서 데이터를 화면에 그려보자
{:.notice--primary}

<br>
<br>

## useState로 데이터를 화면에 그리기

```js
import React, { useState } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log(result);
  };

  getWeather();

  return (
    <>
      <h1>{apiValue.name}</h1>
    </>
  );
}

export default App;
```
{: .language-javascript .line-numbers data-line="1, 6, 20, 21, 22"}


<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/4ae3c953-c598-4318-a9d6-6b90ac54c8c9' alt=''>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/701a58d3-e83f-4e7c-97bd-a4c134dbaa24' alt=''>

<span class='explain smallText'>화면에는 잘 그려지지만 콘솔을 확인해보면 데이터를 무한정하게 불러오는 모습</span>

⚠️ useState를 이용해서 만든 apiValue의 상태에 변화가 생길때마다 setApiValue(result)  
&emsp;다시 렌더링이 이루어지고 함수가 다시 실행되면서 상태에 다시 변화가 생기고 또 다시 렌더링이 이루어지면서 무한루프가 된다  
&emsp;즉, 함수실행 > setstate > 리렌더링 > 함수실행 > setstate > 리렌더링 ... 이 순서가 반복된다
{:.notice--warning}

💡 따라서, 이것을 해결하기 위해서는 getWeather 함수가 한 번만 호출되도록 만들어주면 되는데  
💡 **useEffect Hook**을 이용하면 **처음 렌더링될 때 딱 한번** 만 함수가 실행되도록 할 수 있다
{:.notice--primary}

<br>
<br>

## useEffect 로 반복되는 호출 막기

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log(result);
  };

  useEffect(() => {
    getWeather();
  }, []);

  return (
    <>
      <h1>{apiValue.name}</h1>
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="1, 17, 19"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/a299c172-d0e7-430d-aba6-836a36eff398' alt=''>

<span class='explain smallText'>데이터가 한 번만 호출되는 모습</span>

💡 그럼 이제 화면에 그려줄 요소를 더 추가해보자
{:.notice--primary}

<br>

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log(result);
  };

  useEffect(() => {
    getWeather();
  }, []);

	// 날씨 이름 : name
	// 날씨 그림 : weather[0].icon
	// 날씨 설명 : weather[0].description
	// 날씨 온도(최고/최저) : main.temp / main.temp_max / main.temp_min

  return (
    <>
      <div className="today-weather">
        <h1>Now</h1>
        <h1>{apiValue.name}</h1>
        <h2>
          <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
        </h2>
        <h2>{apiValue.weather[0].description}</h2>
        <h2>{apiValue.main.temp}</h2>
        <h3>
          최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
        </h3>
      </div>
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/278adaaa-67a3-48b4-b167-adb6846b8e9b' alt=''>

<span class='explain smallText'>하지만 "정의되지 않은 속성을 읽을 수 없습니다" 라며 TypeError를 뿜고 있는 모습</span>

🔎 TypeError가 나는 이유는?  
&emsp;바로 useEffect의 실행 시점 때문이다  
<br>
&emsp;내가 바랬던 실행순서는 이렇다  
&emsp;최초에 렌더링 `>` useEffect `>` setstate `>` 리렌더링  
<br>
&emsp;하지만, 사실 useEffect가 실행되기 전에 문제가 생긴다    
&emsp;최초에 렌더링 `>` **데이터없음(TypeError)** `>` useEffect `>` setstate `>` 리렌더링    
<br>
&emsp;다시말해, useEffect가 실행되고 getWeather 함수가 호출된 이후에    
&emsp;상태변화가 일어날 **변수(apiValue)의 초기값으로 화면을 그리려고 하다보니** TypeError가 발생한 것이다    
{:.notice--info}

<br>

```js
const obj = { name: 'DongSup' };
console.log(obj.name); // 'DongSup'
console.log(obj.age); // undefined
console.log(obj.age.birth); // TypeError: Cannot read property 'birth' of undefined

console.log(apiValue.name); // undefined
console.log(''.name); // undefined

console.log(apiValue.weather[0].icon) // TypeError
console.log(undefined.icon) // TypeError
```

💡 TypeError는 잘못된 데이터 유형 또는 잘못된 동작으로 인해 발생한다  
💡 메서드 체이닝이 `apiValue.name` 한 번 이루어지면  undefined가 떠서 error는 뿜지않고 화면에 그려주지 않아서 괜찮지만  
💡 `apiValue.weather[0].icon` 연속 두번 이루어지는 부분에서부터 `undefined.icon` 이기 때문에 TypeError가 난다  
💡 따라서, 받아온 데이터가 존재할 경우로 undefined가 아닐 때만 출력하도록 하면 된다
{:.notice--primary}

<br>
<br>

## && 연산자로 TypeError 해결하기

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log(result);
  };

  useEffect(() => {
    getWeather();
  }, []);

  return (
    <>
      {apiValue && 
      <div className="today-weather">
        <h1>Now</h1>
        <h1>{apiValue.name}</h1>
        <h2>
          <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
        </h2>
        <h2>{apiValue.weather[0].description}</h2>
        <h2>{apiValue.main.temp}</h2>
        <h3>
          최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
        </h3>
      </div>
      }
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="23, 36"}

💡 TypeError를 해결하는 또 다른 방법으로  
&emsp;데이터가 완전히 불러와질 때까지 데이터를 이용해 그려질 컴포넌트가 아닌  
&emsp;**다른 컴포넌트가 먼저 렌더링** 되도록 한 이후에, 데이터가 모두 준비되면  
&emsp;데이터를 이용해 그려질 컴포넌트가 렌더링 되도록 해보자 
{:.notice--primary}

<br>

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    setLoading(false);
    console.log(result);
  };

  useEffect(() => {
    getWeather();
  }, []);

  return (
    <>
      {loading ? (
        <div>Loading...</div>
      ) : (
        <div className="today-weather">
          <h1>Now</h1>
          <h1>{apiValue.name}</h1>
          <h2>
            <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
          </h2>
          <h2>{apiValue.weather[0].description}</h2>
          <h2>{apiValue.main.temp}</h2>
          <h3>
            최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
          </h3>
        </div>
      )}
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="7, 25, 26, 27, 40"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/d22c0dce-a34b-4c64-a627-d924de6b24d9' alt=''>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/afa93418-2319-41ac-9933-88ba45686e17' alt=''>

<span class='explain smallText'>로딩 컴포넌트가 먼저 보여지다가 데이터가 모두 불러와지면 원래 보여주려던 컴포넌트가 보여지는 모습</span>

💡 이제 현재 날씨 API에서 필요한 데이터를 모두 그려줬으니 일기예보 API도 연동해보자
{:.notice--primary}

<br>
<br>

## 일기예보 API 연동하기

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log('첫번재 API', result);
  };

  const getFutureWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setSecondApiValue(result);
    setLoading(false);
    console.log('두번째 API', result);
  };

  useEffect(() => {
    getWeather();
    getFutureWeather();
  }, []);

  return (
    <>
      {loading ? (
        <div>Loading...</div>
      ) : (
        <div className="today-weather">
          <h1>Now</h1>
          <h1>{apiValue.name}</h1>
          <h2>
            <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
          </h2>
          <h2>{apiValue.weather[0].description}</h2>
          <h2>{apiValue.main.temp}</h2>
          <h3>
            최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
          </h3>
        </div>
      )}
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="7,19,20,21,22,23,24,25,26,27,31"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/d9d0bb72-6fea-4549-a00b-46de9937dc3e' alt=''>

<span class='explain smallText'>두번째 API도 성공적으로 불러온 모습</span>

💡 받아온 데이터를 펼쳐보면 총 40개의 날씨정보를 가져온 것으로 보인다.  
&emsp;현재시각을 기준으로 3시간 단위로 5일치의 일기예보 데이터를 가져온 것인데  
&emsp;이 중에서 오후 6시에 해당하는 날씨만 해서 총 3일치의 날씨 데이터를 추출하려고 한다
{:.notice--primary}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/e912e5a7-a34d-4d55-8421-87781b4b36ef' alt=''>

<span class='explain smallText'>추출할 속성의 모습</span>

<br>
<br>

## filter 메서드를 통해 데이터 추출하기

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log('첫번재 API', result);
  };

  const getFutureWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();

    const oneDay = 1000 * 60 * 60 * 24;
    const offset = 1000 * 60 * 60 * 9;
    const today = new Date().getTime() + offset;
    const DesiredTime = ' 18:00:00';
    const oneDaysLater = new Date(today + oneDay).toISOString().slice(0, 10) + DesiredTime;
    const twoDaysLater = new Date(today + oneDay * 2).toISOString().slice(0, 10) + DesiredTime;
    const threeDaysLater = new Date(today + oneDay * 3).toISOString().slice(0, 10) + DesiredTime;

    const data = result.list.filter(item => {
      return item.dt_txt === oneDaysLater || item.dt_txt === twoDaysLater || item.dt_txt === threeDaysLater;
    });

    setSecondApiValue(data);
    setLoading(false);
    console.log('두번째 API', result);
    console.log('추출한 데이터', data);
  };

  useEffect(() => {
    getWeather();
    getFutureWeather();
  }, []);

  return (
    <>
      {loading ? (
        <div>Loading...</div>
      ) : (
        <div className="today-weather">
          <h1>Now</h1>
          <h1>{apiValue.name}</h1>
          <h2>
            <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
          </h2>
          <h2>{apiValue.weather[0].description}</h2>
          <h2>{apiValue.main.temp}</h2>
          <h3>
            최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
          </h3>
        </div>
      )}
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="25,26,27,28,29,30,31,32,33,34,35,36,37,39"}

💡 dt_txt 속성의 형식에 맞는 데이터를 만들기위해 Date 함수를 이용해서 1일치, 2일치, 3일치의 밀리초를 구했다  
💡 그 다음 1970년 1월 1일 부터 경과한 밀리초에 각각의 밀리초(1일치,2일치,3일치)를 더해줬다  
💡 그 다음 Date 함수의 인자로 각각의 밀리초를 넣은 뒤에 Date 객체를 생성한다음  
&emsp;toISOString(), slice() 함수로 가공해줬다 `>` '2023-05-14'  
<br>
💡 마지막으로 ' 18:00:00' 형식의 문자열을 더해준다음 각각의 날짜를 filter 메서드의 조건식에 추가해서 데이터를 추출했다
{:.notice--primary}

⚠️ 1970년 1월 1일 경과한 시간은 UTC라고 하는데 이 UTC로 한국의 현재시간을 얻으려면 9시간을 더해야한다  
&emsp;다시말해서, UTC와 한국시간이 9시간 차이가나기때문인데  
&emsp;00시부터 9시 이전에 날씨를 불러오면 UTC는 9시간이 느리기 때문에  
&emsp;오늘 기준이아닌 어제 기준으로 다음날이 계산돼서 하루씩 앞당겨지게 된다  
&emsp;그래서 1970년 1월 1일 부터 경과한 밀리초에서 9시간짜리 밀리초 더해줬다
{:.notice--warning}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/41610e3a-c455-4904-8903-61a1758f0483' alt=''>

<span class='explain smallText'>성공적으로 데이터를 추출한 모습</span>

💡 추출한 데이터를 가지고 화면에 그려보자
{:.notice--primary}

<br>

```js
import React, { useState, useEffect } from 'react';

function App() {
  const cityValue = 'seoul';
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  const getWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();
    setApiValue(result);
    console.log('첫번재 API', result);
  };

  const getFutureWeather = async () => {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
    );
    const result = await response.json();

    const oneDay = 1000 * 60 * 60 * 24;
    const offset = 1000 * 60 * 60 * 9;
    const today = new Date().getTime() + offset;
    const DesiredTime = ' 18:00:00';
    const oneDaysLater = new Date(today + oneDay).toISOString().slice(0, 10) + DesiredTime;
    const twoDaysLater = new Date(today + oneDay * 2).toISOString().slice(0, 10) + DesiredTime;
    const threeDaysLater = new Date(today + oneDay * 3).toISOString().slice(0, 10) + DesiredTime;

    const data = result.list.filter(item => {
      return item.dt_txt === oneDaysLater || item.dt_txt === twoDaysLater || item.dt_txt === threeDaysLater;
    });

    setSecondApiValue(data);
    setLoading(false);
    console.log('두번째 API', result);
    console.log('추출한 데이터', data);
  };

  useEffect(() => {
    getWeather();
    getFutureWeather();
  }, []);

  // * 첫번째 API
  // 날씨 이름 : name
  // 날씨 그림 : apiValue.weather[0].icon
  // 날씨 설명 : weather[0].description
  // 날씨 온도(최고/최저) : main.temp / main.temp_max / main.temp_min

  // * 두번째 API
  // 날씨 그림 : weather[0].icon
  // 예보 일자 : dt_txt
  // 날씨 설명 : weather[0].description
  // 날씨 온도 : main.temp

  return (
		<>
      {loading ? (
        <div>Loading...</div>
      ) : (
        <>
          <div className="today-weather">
            <h1>Now</h1>
            <h1>{apiValue.name}</h1>
            <h2>
              <img src={`https://openweathermap.org/img/wn/${apiValue.weather[0].icon}.png`} alt="" />
            </h2>
            <h2>{apiValue.weather[0].description}</h2>
            <h2>{apiValue.main.temp}</h2>
            <h3>
              최고: {apiValue.main.temp_max} | 최저: {apiValue.main.temp_min}
            </h3>
          </div>

          <div className="feature-weathers">
            {secondApiValue.map((item, index) => {
              return (
                <div className="feature-weather" key={index}>
                  <h4>
                    <img src={`https://openweathermap.org/img/wn/${item.weather[0].icon}.png`} alt="" />
                  </h4>
                  <h4>{item.dt_txt.slice(5, 10)}</h4>
                  <h5>{item.weather[0].description}</h5>
                  <h5>{item.main.temp}</h5>
                </div>
              );
            })}
          </div>
        </div>
      )}
    </>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90 91"}

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/ef51eeb1-dfc3-4a01-b4ad-2bd12b7e6aa0' alt=''>

<span class='explain smallText'>화면에 잘 그려지는 모습</span>

💡 이제 두 API 모두 성공적으로 불러왔으니 다음 챕터에서는 컴포넌트를 분리하고 예외처리를 해보자
{:.notice--primary}
