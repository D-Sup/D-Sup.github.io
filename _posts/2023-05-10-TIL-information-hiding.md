---
title: '[코어자바스크립트] 정보은닉(information hiding)'
excerpt_separator: '<!--more-->'
categories:
  - Core JavaScript
tags:
  -
header:
  teaser: ./assets/image/28.png
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

<!-- content -->

## 접근 권한 제어(정보은닉)

<span class='explain'>아래는 외부 함수의 변수를 참조하는 내부함수 대해 다룬 코드이다</span>

```js
var outer = function () {
  var a = 1;

  var inner = function () {
    return ++a;
  };

  return inner();
};

var outer2 = outer();
console.log(outer2);
```

> outer 함수가 반환한 inner 함수는 outer 함수 내부의 a 변수에 직접적이 아닌 간접적으로 접근할 수 있는데  
> outer 함수 내부에서 선언된 a 변수는 outer 함수의 스코프 안에서만 유효한 지역 변수이기 때문에,   
> outer 함수가 return 한 정보에만 접근이 가능하고 **outer 함수 외부에서는 직접적으로 접근할 수 없다**

💡 이를 통해 알 수 있는 것은 **return을 활용**해서 일부 변수에 대한 **접근 권한을 부여**할 수 있다는 것이다
{:.notice--primary}

<br>
<br>

### 접근 제한자 public / private
---

> 자바, C++, C# 과 같은 언어에서는 접근 제한자라는 키워드가 존재하는데,  
> public, private, protected 라는 접근 제한자를 통해 접근을 제어할 수 있다  
> 여기서 public은 외부에서 접근가능한 것을 의미하고  
> private는 외부에 노출되지 않고 내부에서만 접근가능한 것을 의미한다

🔎 반면, 자바스크립트에서는 이 public과 private라는 접근 제한자를 지원하진 않지만  
&emsp;이러한 접근 제한자 없이도 **클로저로 public한 값과 private한 값을 구분**해서 접근권한을 제어할 수 있다
{:.notice--info}

<br>

```js
var outer = function () {
  var a = 1;

  var inner = function () {
    return ++a;
  };

  return inner();
};

var outer2 = outer();
console.log(outer2);
```

> 다시 코드를 살펴보자면,  
> a 변수는 outer 함수 내부에서만 사용되는 private한 값이 된다  
> 하지만 outer 함수에서 반환하고 있는 inner 함수는   
> 스코프 외부에서도 접근이 가능하기 때문에 public한 값이 된다

💡 그래서 정리하자면,  
&emsp;외부에 제공하고자하는 정보들은 **return하고**,  
&emsp;내부에서만 사용할 정보들은 **return하지 않는 것**으로 **접근 권한 제어가 가능**하다
{:.notice--primary}

<br>
<br>

### 접근 권한에 더 익숙해지기
---

<span class='explain'>아래는 간단하게 가위바위보 게임을 구현해본 코드이다</span>

```js
var 가위바위보게임 = {
  options : ['가위', '바위', '보'],
  moved : 0,
  choice : 1,

  run: function () {
    var computerChoice = this.options[Math.floor(Math.random() * 3)];
    var playerChoice = this.options[this.choice];

    if (computerChoice === playerChoice) {
      console.log('비겼습니다');
      console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
    } else if (
      (computerChoice === '가위' && playerChoice === '보') ||
      (computerChoice === '바위' && playerChoice === '가위') ||
      (computerChoice === '보' && playerChoice === '바위') 
      ) {
      console.log('인간패배');
      this.moved -= 1;
      console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
      console.log(`계단 한칸 내려가기 (현재 ${this.moved}번째 계단)`)
    } else {
      console.log('인간승리');
      this.moved += 1;
      console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
      console.log(`계단 한칸 내려가기 (현재 ${this.moved}번째 계단)`)
    }
  }
}
```

> 가위바위보게임 이라는 변수에 객체를 직접 할당하고  
> run() 이라는 메서드를 실행할 때 사용할 options, moved라는 프로퍼티에 값을 부여했다  
> 
> 그래서 외부에서 호출이 이루어져서 run() 메서드가 실행되면  
> Math.random 함수를 이용해서 0부터 3까지  
> 랜덤한 숫자의 인덱스에 해당하는 options 의 값을  
> 컴퓨터가 낼 선택지인  computerChoice라는 변수에 할당하고   
> playerChoice라는 변수에는 무조건 주먹이 들어가도록 할당했다  
> 
> 그 다음 if문으로 비교해서   
> 각각의 값이 같을경우 `||` 엔드연산자를 이용해서 내가 컴퓨터에게 졌을 경우 `||` 내가 이겼을경우   
> 각각의 경우에 따라 해당 메세지를 출력하고  
> 이기면 위로 한칸 지면 아래로 한칸 내려가도록 만들었다

<br>

```js
가위바위보게임.run()

// 인간승리
// 플레이어:바위 | 컴퓨터:가위
// 계단 한칸 내려가기 (현재 1번째 계단)
```

💡 run() 메서드를 통해 실행해보면 잘 작동되지만 컴퓨터에게 계속 가위바위보를 지는바람에 분한나머지 코드를 수정해봤다 😎
{:.notice--primary}

<br>

```js
가위바위보게임.options = ['가위', '바위', '가위']

가위바위보게임.run()
가위바위보게임.run()
가위바위보게임.run()

// 인간승리
// 플레이어:바위 | 컴퓨터:가위
// 계단 한칸 내려가기 (현재 1번째 계단)
// 비겼습니다
// 플레이어:바위 | 컴퓨터:바위
// 인간승리
// 플레이어:바위 | 컴퓨터:가위
// 계단 한칸 내려가기 (현재 2번째 계단)
```

> options를 [가위, 바위, 가위]로 수정해서   
> 컴퓨터는 3분에 2확률로 가위를 내고 3분에 1확률로 바위를 내서  
> 내가 지는 일 없이 이기거나 비기도록 만들었다

💡 하지만 이런식으로 마음대로 객체안의 값을 수정해버린다면 이기기만하는 일방적인 게임이 될 수 있다  
💡 여기서 바로 클로저를 활용하면 값을 변경하지못하도록 할 수 있다
{:.notice--primary}

<br>
<br>

#### 클로저 활용하기
---

> 이번에는 객체로 이루어진 기존의 코드에서 함수로 만들어 보았다   
> 그리고 외부에 제공할 정보들만 return 해서 변수를 보호하도록 했다

```js
var 가위바위보게임 = function () {
  var options = ['가위', '바위', '보'];
  var moved = 0;
  var choice = 1;

  return {
    get moved () {
      return moved
    },
    get choice () {
      return choice
    },
    set choice (value) {
      if (value === 0 || value === 1 || value === 2) {
        choice = value;
      } else if (value === 'random') {
        choice = Math.floor(Math.random() * 3);
      }
    },
    run: function () {
      var computerChoice = options[Math.floor(Math.random() * 3)];
      var playerChoice = options[choice];
  
      if (computerChoice === playerChoice) {
        console.log('비겼습니다');
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
      } else if (
        (computerChoice === '가위' && playerChoice === '보') ||
        (computerChoice === '바위' && playerChoice === '가위') ||
        (computerChoice === '보' && playerChoice === '바위') 
        ) {
        console.log('인간패배');
        moved -= 1;
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
        console.log(`계단 한칸 내려가기 (현재 ${moved}번째 계단)`)
      } else {
        console.log('인간승리');
        moved += 1;
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
        console.log(`계단 한칸 내려가기 (현재 ${moved}번째 계단)`)
      }
    }
  }
}
```
{: .language-js .line-numbers .data-line="6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 43"}

> 이번에는 가위바위보라는 함수를 실행함으로써   
> options, choice, moved 라는 변수들은 private로 구분해서 외부에서의 접근을 제한했고,  
> return 문 안에서 객체를 생성하고 반환해줌으로써 run() 메서드를 외부에서 접근 가능하도록 했다

여기 있는 get 과 set 은 getter 와 setter를 의미한다   
Getter는 객체의 속성 값을 반환하는 메서드이고   
Setter는 객체의 속성 값을 설정하거나 변경하는 메서드이다  
<br>
이 getter 와 setter 를 이용해서  
변수 값을 외부에서 접근하지 못하도록 보호(캡슐화)할 수 있는데요  
함수 앞에 get 또는 set 키워드를 붙여서 정의하고  
return 키워드를 사용해서 읽어오거나 수정할 값을 설정할 수 있다
{:.notice}


> moved는 getter로 값을 읽어올 수 있도록 했고  
> choice는 값을 수정하거나 불러올 수 있도록   
> getter 와 setter 로 두가지 모두 부여했다  
> choice의 값을 수정했을 때 0 또는1 또는 2를 입력하면 입력한 값을 choice에 재할당해주고  
> 만약 random이란 문자열을 입력하면 0부터 3까지의 랜덤한 숫자가 choice에 재할당되도록 했다

💡 따라서 이제 외부에서는 run() 메서드를 실행하거나,  
&emsp;moved 값을 확인하거나 choice 값을 변경하거나 확인하는 동작만이 가능하게 되었다
{:.notice--primary}

<br>

```js
var 플레이 = 가위바위보게임();
플레이.run();
// 비겼습니다
// 플레이어:바위 | 컴퓨터:바위

console.log(플레이.moved);
플레이.choice = 0;
플레이.run();
// 0
// 비겼습니다
// 플레이어:가위 | 컴퓨터:가위

플레이.choice = 'random';
플레이.run();
// 인간패배
// 플레이어:가위 | 컴퓨터:바위
// 계단 한칸 내려가기 (현재 -1번째 계단)

console.log(플레이.options);
// undefined
플레이.options = ['바위', '바위', '보'];
console.log(플레이.options);
// [ '바위', '바위', '보' ]
```

> 이제 가위바위보게임 함수를 호출해서 생성된 객체를  
> play 변수에 할당해서 run() 메서드로 호출해보면   
> 마찬가지로 잘 작동하고 setter 를 통해 값을 변경해줘도 잘 작동된다

⚠️ 그런데, 플레이.options 으로 외부에서 접근하면  
&emsp;내부 변수를 접근하는 것이기 때문에 undefined가 되지만   
&emsp;`플레이.options = ['바위', '바위', '보'];`  이 부분의 코드가 실행되면 options 의 값이 변경되어버린다
{:.notice--warning}

<br>

<img class='img' src='https://github.com/D-Sup/TECHIT_CODELION/assets/96939334/3d019a52-b4e7-47f9-8a25-c477526cc116' alt=''>

🔎 이것은 함수 내부에 정의 되어있는 options 변수와는 별개로  
&emsp;가위바위보게임 함수의 전역변수로 들어가는게 아니라 새로운 options 라는 프로퍼티가  
&emsp;run() 메서드 내부에서생성되어 그 값을 할당하면서 **options값이 덮어씌워진 것이다**
{:.notice--info}

✔️ 따라서 이것을 해결하기 위해서는,  
&emsp;**Object.freeze() 메서드**를 사용하면 되는데,  
&emsp;Object.freeze() 메서드는 객체를 **"동결"**하여 수정할 수 없도록 만들 수 있다
{:.notice--success}

<br>
<br>

#### Object.freeze()
---

```js
var 가위바위보게임 = function () {
  var options = ['가위', '바위', '보'];
  var moved = 0;
  var choice = 1;

  var publicMembers = {
    get moved() {
      return moved;
    },
    get choice() {
      return choice;
    },
    set choice(value) {
      if (value === 0 || value === 1 || value === 2) {
        choice = value;
      } else if (value === 'random') {
        choice = Math.floor(Math.random() * 3);
      }
    },
    run: function () {
      var computerChoice = options[Math.floor(Math.random() * 3)];
      var playerChoice = options[choice];

      if (computerChoice === playerChoice) {
        console.log('비겼습니다');
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
      } else if (
        (computerChoice === '가위' && playerChoice === '보') ||
        (computerChoice === '바위' && playerChoice === '가위') ||
        (computerChoice === '보' && playerChoice === '바위')
      ) {
        console.log('인간패배');
        moved -= 1;
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
        console.log(`계단 한칸 내려가기 (현재 ${moved}번째 계단)`);
      } else {
        console.log('인간승리');
        moved += 1;
        console.log(`플레이어:${playerChoice} | 컴퓨터:${computerChoice}`);
        console.log(`계단 한칸 내려가기 (현재 ${moved}번째 계단)`);
      }
    },
  };
  Object.freeze(publicMembers);
  return publicMembers;
};
```
{: .language-js .line-numbers .data-line="6, 43, 44, 45"}

> 위와 같이, 객체의 모든 속성들이 불변 상태로 유지되도록 할 수 있다  
> 따라서 publicMebers라는 객체를 따로 만들어서   
> return 안에 있던 내용들을 그 안으로 옮겨주고  
>   
> Object.freeze() 라는 메서드를 통해  객체의 속성들을 변경할 수 없도록 만들어 주게되고  
> publicMembers 객체 내부의 options 변수는 Object.freeze로 불변화되어 있으므로,  
> publicMembers 객체 외부에서는 fuel 변수의 값을 변경할 수 없다

<br>

💡 이러한 정보은닉은 객체 내부의 상태나 동작을 외부에서 직접 접근하는 것을 제한할 수 있다  
💡 정보은닉은 객체지향 프로그래밍에서 중요한 개념이라 더 공부할 필요성이 있다고 느꼈다
{:.notice--primary}





<!-- content -->
