---
title: '[동료특강] React로 날씨 Open API 다루기 2'
excerpt_separator: '<!--more-->'
categories:
  - React
tags:
  -
header:
  teaser: ./assets/image/30.png
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

> 전 [포스팅](https://d-sup.github.io/react/TIL-weather-api/) 에서 API를 모두 불러와서 화면에 그리는 것 까지 모두 완료했다  

💡 이제 **컴포넌트를 분리**해서 **추가기능**을 넣고 **예외처리**까지 해보자!
{:.notice--primary}

<br>

## 컴포넌트 분리

<span class='explain smallText'>App.jsx</span>

```js
import React, { useState } from 'react';
import './index.css'
import DisplayWeather from './Components/DisplayWeather/DisplayWeather';

function App() {
  const [inptLocation, setInptLocation] = useState('');
  const [searchPage, setSearchPage] = useState(false);

  function submitForm(event) {
    event.preventDefault();
    setSearchPage(true);
  }
  return (
    <div className="container">
      <div className="wrapper">
        {searchPage ? (
          <DisplayWeather />
        ) : (
          <form onSubmit={submitForm}>
              <input type="text" placeholder="Seoul" value={inptLocation} onChange={event => setInptLocation(event.target.value)} />
              <button type="submit">🔍</button>
            </form>
        )}
      </div>
</div>
    
  );
}

export default App;
```

<span class='explain smallText'>components/DisplayWeather.jsx</span>

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather() {
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
        </>
      )}
    </>
  );
}
```

💡 입력값을 받는 초기화면 따로 데이터를 보여주는 화면따로 컴포넌트를 분리했다  
💡 입력값을 받으면 원하는 지역의 날씨를 보여주고 입력값을 받지않으면 현재 위치의 날씨를 보여주도록 구현해 볼 것이다  
💡 우선 입력 값을 받으면 요청받는 URL의 매개변수가 변경되도록 해보자
{:.notice--primary}

<br>
<br>

## 입력 값에 따라 검색하기

<span class='explain smallText'>App.jsx</span>

```js
import React, { useState } from 'react';
import './index.css';
import DisplayWeather from './Components/DisplayWeather/DisplayWeather';

function App() {
  const [inptLocation, setInptLocation] = useState('');
  const [searchPage, setSearchPage] = useState(false);
  const [preInput, setPreInput] = useState('');

  function submitForm(event) {
    event.preventDefault();
    setSearchPage(true);
    setInptLocation(preInput);
    setPreInput('');
  }

  function turnBack() {
    setSearchPage(false);
  }

  return (
    <div className="container">
      <div className="wrapper">
        {searchPage ? (
          <DisplayWeather inptLocation={inptLocation} turnBack={turnBack} />
        ) : (
          <form onSubmit={submitForm}>
            <input
              type="text"
              placeholder="Seoul"
              value={preInput}
              onChange={event => setPreInput(event.target.value)}
            />
            <button type="submit">🔍</button>
          </form>
        )}
      </div>
    </div>
  );
}

export default App;
```
{: .language-js .line-numbers data-line="8, 13, 14, 25"}

<span class='explain smallText'>components/DisplayWeather.jsx</span>

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather(props) {
  const cityValue = props.inptLocation;
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
        <div onClick={props.turnBack}>
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
        </>
      )}
    </>
  );
}
```
{: .language-js .line-numbers data-line="3,4,65"}

💡 App.jsx 에서 inptLocation변수와 turnBack함수를 props로 넘겨준 다음에,  
💡 DisplayWeather.jsx 에서 URL에 들어갈 변수값을 inptLocation값으로 재할당을,  
💡 결과를 보여주는 요소에서 onClick하면 turnBack함수가 실행되도록 했다  
💡 이제 잘못된 값을 넣었을 때의 예외처리를 해보자
{:.notice--primary}

<br>
<br>

## try-catch로 예외처리하기

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather(props) {
  const cityValue = props.inptLocation;
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  const getWeather = async () => {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${cityValue}&appid=${key}&units=metric&lang=KR`,
      );
      const result = await response.json();
      setApiValue(result);
      console.log('첫번재 API', result);
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
  };

  const getFutureWeather = async () => {
    try {
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
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
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
        </>
      )}
    </br>
  );
}
```
{: .language-js .line-numbers data-line="11, 18, 19, 20, 21, 25, 47, 48, 49, 50"}

🔎 async/await를 사용하는 경우, 예외 처리를 위해 `try/catch` 를 사용할 수 있다  
🔎 try 에 예외가 발생할 수 있는 부분을 넣어주고, catch에 예외가 발생했을 때 예외 처리될 부분을 넣어주면 된다
{:.notice--info}

💡 catch 의 인자로 error를 받아서 error를 출력할 수 있도록 한 다음   
&emsp;error가 나면 그냥 초기화면으로 넘어가도록 props.turnBack() 함수가 실행되도록 했다  
<br>
💡 이제 현재위치기반으로 날씨를 불러오는 기능을 추가하기 위해 현위치 정보를 구할 수 있는 API를 이용해보자
{:.notice--primary}

<br>
<br>

## Geolocation API 이용하기

> `Geolocation API` 는 사용자의 동의 하에 웹 애플리케이션에서 위치 정보에 접근할 수 있는 API이다  
> `navigator.geolocation` 이라는 객체를 통해 `Geolocation API`를 사용하고  
> `getCurrentPosition()` 메서드를 호출해서 현재 위치를 받아올 수 있다
    

```js
navigator.geolocation.getCurrentPosition(success, error, [options])
```

🔎 getCurrentPosition의 첫 번째 매개변수에는 위치 정보를 성공적으로 가져왔을 때 실행되는 콜백 함수 이다  
🔎 두 번째 매개변수는 위치 정보를 가져오는 데 실패했을 때 실행되는 콜백 함수 이다
{:.notice--info}

<br>

```js
navigator.geolocation.getCurrentPosition((position) => {
	console.log(position);
});
```

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/056b66a9-c9cd-401e-bbc0-2139431e60df' alt=''>

<span class='explain smallText'>콘솔로 GeolocationPosition 객체를 확인한 모습</span>

🔎 `GeolocationPosition` 이라는 객체를 첫번재 매개변수로 받을 수 있는데  
&emsp;위 코드에서는 `position` 이 `GeolocationPosition` 이라는 객체를 나타내게 된다  
<br>
🔎 그 안에는 latitude 와 longitude 로 경도와 위도에 대한 정보가 들어있다
{:.notice--info}

🔎 Geolocation API는 비동기 함수이기 때문에 비동기로 작업이 이루어진다   
<br>
🔎 이제 fetch 함수의 인자로 들어갈 URL의 매개변수로 위도와 경도가 들어가도록 수정할텐데   
&emsp;이 위도와 경도를 먼저 동기적으로 가져온 다음에 fetch 함수가 실행 되어야 한다  
<br>
🔎 따라서, 동기적인 처리를 하기 위해서는 await 를 사용해야하고 await 를 사용하려면  
&emsp;Promise 객체를 사용해서 비동기함수를 감싼 후 await를 사용가능하다  
{:.notice--info}

💡 그럼 URL을 수정하고 Promise 객체로 함수를 감싸보자
{:.notice--primary}


<br>

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather(props) {
  const cityValue = props.inptLocation;
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  function currentLocation() {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        position => {
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;
          resolve({ latitude, longitude });
        },
        error => {
          reject('error');
        },
      );
    });
  }

  const getWeather = async () => {
    try {
      const { latitude, longitude } = await currentLocation();
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${key}&units=metric&lang=KR`,
      );
      const result = await response.json();
      setApiValue(result);
      console.log('첫번재 API', result);
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
  };

  const getFutureWeather = async () => {
    try {
      const { latitude, longitude } = await currentLocation();
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/forecast?lat=${latitude}&lon=${longitude}&appid=${key}&units=metric&lang=KR`,
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
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
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
        </>
      )}
    </br>
  );
}
```
{: .language-js .line-numbers data-line="10,11,12,13,14,15,16,17,18,19,20,21,22,23,27"}

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/987520d3-5608-4446-a6c7-a238395dfd45' alt=''>

<span class='explain smallText'>현재위치 기반으로 불러온 모습</span>

💡 이제 입력값을 받으면 원하는 지역의 날씨를 보여주고 입력값을 받지않으면 현재 위치의 날씨를 보여주도록 구현해보자
{:.notice--primary}

<br>

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather(props) {
  const cityValue = props.inptLocation;
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  function currentLocation() {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        position => {
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;
          resolve({ latitude, longitude });
        },
        error => {
          reject('error');
        },
      );
    });
  }

  const getWeather = async () => {
    try {
      const { latitude, longitude } = await currentLocation();
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?${
          cityValue ? 'q=' + cityValue : 'lat=' + latitude + '&lon=' + longitude
        }&appid=${key}&units=metric&lang=KR`
      );
      const result = await response.json();
      setApiValue(result);
      console.log('첫번재 API', result);
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
  };

  const getFutureWeather = async () => {
    try {
      const { latitude, longitude } = await currentLocation();
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/forecast?${
          cityValue ? 'q=' + cityValue : 'lat=' + latitude + '&lon=' + longitude
        }&appid=${key}&units=metric&lang=KR`,
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
    } catch (error) {
      console.error('Error:', error);
      props.turnBack();
    }
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
        <div onClick={props.turnBack}>
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
```
{: .language-js .line-numbers data-line="30, 47"}

💡 이제 구현은 모두 끝이 났다  
💡 하지만, 두 개의 API를 연동하다보니 불필요하게 currentLocation 함수를 호출해서 구조분해할당하는 부분이 불필요하게 느껴진다  
💡 두 개의 API를 하나의 함수에서 연동할 수 있도록 합쳐주고 병렬처리할 필요성이 있다
{:.notice--primary}

<br>
<br>

## Promise.all로 병렬처리

> `Promise.all()` 메서드사용하면 Promise.all() 의 인자로 전달된 Promise 들이 모두 완료될 때까지 기다린다  
> 모든 Promise가 완료되면 모든 Promise들의 결과 값이 배열 형태로 반환되는데,  
> 이때 배열 구조 분해 할당을 사용하면 각각의 결과값을 개별 변수로 받을 수 있다  
> 이렇게 개별 변수로 받은 Promise들을 가지고, 추가적인 작업을 수행할 수 있다

```js
const fisrtResponse = fetch(
        'url(1)...',
      );
const secondResponse = fetch(
        'url(2)...',
      );
const thirdResponse = fetch(
        'url(2)...',
      );

Promise.all([fisrtResponse , secondResponse , thirdResponse])
  .then(([data1,data2,data3]) => {
    console.log(data1); 
		console.log(data2); 
		console.log(data3); 
  })
  .catch((error) => {
    console.error(error);
  });
```

✔️ `Promise.all()`은 fisrtResponse , secondResponse , thirdResponse 세 개의 프로미스가 모두 이행될 때까지 기다린다  
✔️ 그 후, 세 개의 프로미스의 이행 값들이 배열로 반환되고, 콘솔로 각각의 값을 출력한다  
✔️ 만약 세 개의 프로미스 중 하나라도 거부된다면, 반환된 프로미스도 즉시 거부된다
{:.notice--success}

<br>

```js

async function getApi() {

	const fisrtResponse = fetch(
	        'url(1)...',
	      );
	const secondResponse = fetch(
	        'url(2)...',
	      );
	const thirdResponse = fetch(
	        'url(3)...',
	      );
	
	const [data1,data2,data3] = await Promise.all([fisrtResponse , secondResponse , thirdResponse])

}
```

🔎 이도 마찬가지로 then을 생략하고 함수로 감싸서 async await로 처리하는 것이 가능하다  
🔎 전달된 프로미스 배열에 포함된 프로미스들을 동시에 실행한다   
&emsp;그 과정은 비동기적으로 이루어지며, 각 프로미스는 병렬로 처리된다
{:.notice--info}

💡 이제 Promise.all 을 통해서 병렬처리를 해주자
{:.notice--primary}

<br>

```js
import React, { useState, useEffect } from 'react';

export default function DisplayWeather(props) {
  const cityValue = props.inptLocation;
  const key = '34bc9f0e13f7fdd3adac28b4f182d8ad';
  const [apiValue, setApiValue] = useState('');
  const [secondApiValue, setSecondApiValue] = useState('');
  const [loading, setLoading] = useState(true);

  function currentLocation() {
    return new Promise((resolve, reject) => {
      navigator.geolocation.getCurrentPosition(
        position => {
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;
          resolve({ latitude, longitude });
        },
        error => {
          reject('error');
        },
      );
    });
  }

  const getWeather = async () => {
    try {
      const { latitude, longitude } = await currentLocation();
      const responseFirst = fetch(
        `https://api.openweathermap.org/data/2.5/weather?${
          cityValue ? 'q=' + cityValue : 'lat=' + latitude + '&lon=' + longitude
        }&appid=${key}&units=metric&lang=KR`,
      );

      const responseSecond = fetch(
        `https://api.openweathermap.org/data/2.5/forecast?${
          cityValue ? 'q=' + cityValue : 'lat=' + latitude + '&lon=' + longitude
        }&appid=${key}&units=metric&lang=KR`,
      );
  
      const [data1, data2] = await Promise.all([responseFirst, responseSecond])
  
      const result1 = await data1.json();
      const result2 = await data2.json();
  
      console.log('a:', result1);
      console.log('b', result2);
  
      const oneDay = 1000 * 60 * 60 * 24;
      const offset = 1000 * 60 * 60 * 9;
      const today = new Date().getTime() + offset;
      const DesiredTime = ' 18:00:00';
      const oneDaysLater = new Date(today + oneDay).toISOString().slice(0, 10) + DesiredTime;
      const twoDaysLater = new Date(today + oneDay * 2).toISOString().slice(0, 10) + DesiredTime;
      const threeDaysLater = new Date(today + oneDay * 3).toISOString().slice(0, 10) + DesiredTime;
  
      const data = result2.list.filter(item => {
        return item.dt_txt === oneDaysLater || item.dt_txt === twoDaysLater || item.dt_txt === threeDaysLater;
      });
  
      console.log('첫번재 API', result1);
      setApiValue(result1);
  
      console.log('두번째 API', result2);
      console.log('추출한 데이터', data);
      setSecondApiValue(data);
  
      setLoading(false);
    } catch (error) {
      console.error('Error: ', error);
      props.turnBack();
    }
  };

  useEffect(() => {
    getWeather();
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
        <div onClick={props.turnBack}>
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
```
{: .language-js .line-numbers data-line="28, 34, 40, 42, 43, 60, 61, 63, 64, 65"}

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/829ea266-5ead-4524-a037-e8d13d393760' alt=''>

<span class='explain smallText'>네트워크 탭으로 병렬처리가 되는지 확인한 모습</span>

<br>
<br>

## 최종결과물

[URL](https://d-sup.github.io/API-connection/)

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/84cbfdb6-69cc-4eeb-b576-b3237b0467c4' alt=''>

<img class='img' src='https://github.com/D-Sup/D-Sup.github.io/assets/96939334/318b230e-9891-4269-b2b2-5579067ac55c' alt=''>
