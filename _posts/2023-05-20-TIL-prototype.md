---
title: '[코어자바스크립트] prototype'
excerpt_separator: '<!--more-->'
categories:
  - Core JavaScript
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

<!-- content -->

## 프로토타입

> 자바스크립트는 **프로토타입 기반 언어**이다  
> 클래스 기반 언어에서는 '상속'을 사용하지만 프로토타입 기반 언어에서는  
> 어떤 객체를 원형으로 삼고 이를 복제(참조)함으로써 상속과 비슷한 효과를 얻는다

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/23d92c46-d097-4a9f-9685-3683ad4673fc' alt=''>

🔎 어떤 생성자 함수를 new 연산자와 함께 호출하면  
&emsp;Constructor에서 정의된 내용을 바탕으로 **새로운 인스턴스**가 생성된다  
<br>
🔎 이때 instance에는 **__proto__** 라는 프로퍼티가 자동으로 부여되는데  
&emsp;이 프로퍼티는 Constructor의 **prototype이라는 프로퍼티를 참조**한다
{:.notice--info}

> prototype와 이를 참조하는 **__proto__** 는 모두 객체이다  
> prototype 객체 내부에는 인스턴스가 사용할 메서드를 저장한다

<br>
<br>

### 코드로 살펴보기
---

```js
var Person = function (name) {
	this._name = name;
};
Person.prototype.getName = function() {
	return this._name; 
};

var suzi = new Person('Suzi');
```

> 그럼 실제 코드로 한 번 살펴보자  
> Person 함수는 인자로 받은 name 값을 _name 속성값으로 가진다   
> 
> Person.prototype 프로퍼티에 getName 함수를 추가하고    
> getName 함수에는 this._name으로 해당 인스턴스의 _name 속성값을 반환하도록 작성되어있다  
> 
> suzi 변수에 new Person('Suzi')을 할당해서,    
> Person 함수를 생성자로 사용해서 suzi 라는 인스턴스를 생성한다    
> 여기서 생성자 함수를 호출할 때 this 는 새로 생성된 인스턴스 suzi를 가리키는데   
> suzi._name 속성에는 'Suzi'가 할당된다   
>  
> suzi 인스턴스에는 **__proto__**라는 프로퍼티가 자동으로 부여되고  
> Person.prototype 이라는 프로퍼티를 suzi.**__proto__** 이라는 프로퍼티가 참조하고있다

<br>
<br>

### 개발자 도구로 디렉터리 구조 보기
---

```js
console.dir(Person)
console.dir(suzi)
```
<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/77a22a2e-5543-4f6b-a65b-778474e0a6e8' alt=''>

> 그럼 Person과 suzi가 각각의 디렉터리 구조가 어떠한지 살펴보자   
> 출력결과, 첫 줄에는 함수라는 의미의 f 와 함수 이름인 Person이 보인다   
> 그리고 두 번째 줄에는 suzi가 아닌 Person이라고 나와있다
> 

💡 왜 Person이라고 나와있을까?  
<br>
&emsp;바로, 어떤 인스턴스가 있을때 그 **인스턴스가 어떤 생성자 함수에 속해있는지**  
&emsp;이름을 표기함으로써 해당 함수의 인스턴스임을 알려주기 위함이다 따라서, Person이 찍히게 되는 것이다
{:.notice--primary}

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/4f5767ff-c1af-4c40-9dae-d76050908f0e' alt=''>

> 우선 Person 함수의 디렉터리부터 열어보면,  
> 그 안에는 여러가지 속성들이 존재하는걸로 보여지는데   
> 그 중에서 name 속성과 prototype 속성을 살펴보면  
> name 이라는 속성에는 Person 이라고 나와있다

🔎 이 name 이라는 속성은 함수를 정의할때 자동으로 생성되는데  
🔎 설명하자면, 함수를 익명 함수로 정의하면 name의 값은 빈 문자열이 되고  
🔎 함수의 이름을 지정해서 정의하면 그 이름이 name의 값이 된다
{:.notice--info}

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/259294f3-1845-4d3c-a261-c68feebbfeea' alt=''>

> 그 다음 prototype을 열어보면   
> 아까 Person.prototype 속성에 추가했던 함수인 getName 이 있는 것을 확인할 수 있다

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/5188e466-c2a7-4ce3-bd38-36f579de5587' alt=''>

> 이제 이어서 suzi 인스턴스 의 디렉터리를 열어보면,  
> _name의 속성이 'Suzi'라고 보여지는데   
> 이것은, 아까 코드를 통해 suzi 라는 인스턴스를 생성할때   
> suzi._name 속성에는 'Suzi'라고 할당되었기 때문이다

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/f2f5ff3d-4f40-4c38-b61d-da62e7131237' alt=''>

> 이제 `[[prototype]]` 라는 내부슬롯도 한번 열어보자

🔎 `[[prototype]]` (내부슬롯) 에는 상위 객체가 저장하고 있는 prototype 속성에서 상속받은정보가 존재한다
{:.notice--info}

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/d6fd83a9-a4ec-4946-947e-68cb8f6e0232' alt=''>

🔎 Person 함수가 가지고있던 prototype 속성의 내부와 같은 형태인것을 확인 할 수 있다  
🔎 이것을 통해서 알 수 있는 것은 **상위 객체의 prototype 속성에서 상속 받은 정보를 참조하고 사용**할 수 있다는 것이다
{:.notice--info}

<br>

```js
var suzi = new Person('Suzi');
suzi.__proto__._name = 'SUZI__PROTO__'
suzi.__proto__.getName(); // SUZI__PROTO__
```

🔎 따라서 **__proto__** 을 통해 상속 받은 정보를 사용할 수 있다 
{:.notice--info}

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/79596b35-a831-421a-981f-a5e22de32868' alt=''>

> 각 속성의 색상을 살펴보면 색이 짙은색과 옅은색으로 나뉘어져있는데  
> 이것은 enumerable 이라는 속성 때문이다

🔎 객체의 프로퍼티에 대한 정보를 제공하는 **디스크립터(descriptor)에는 enumerable 속성도 포함**되어있다  
🔎 해당 속성이 **열거가 가능한지 불가능한지의 여부** 에따라 구분을 짓고자 enumerable(이넴뭐러블) 속성이 존재한다  
🔎 여기서 말한 열거가 가능하다는 것은 객체의 속성를 순회하거나 조작하는 작업을 얘기한다  
🔎 따라서, 이 속성값을 통해 방금과 같이 개발자도구에서 직관적으로 확인이 가능하다
{:.notice--info}

<br>

#### 요약
---

💡 생성자 함수의 prototype에 어떤 메서드나 프로퍼티가 있다면  
&emsp;인스턴스에서도 **마치 자신의 것처럼 해당 메서드나 프로퍼티에 접근** 할 수 있다
{:.notice--primary}


<br>
<br>

### Array 생성자 함수
---

```js
var literalArray = [1, 2];
// var literalArray2 = new Array;
console.dir(literalArray);
console.dir(Array);
// console.dir(literalArray2);
```

> 이번에는 내장 생성자 함수인 Array와 배열 리터럴로 생성된 배열을 비교하며 살펴보자  
> 1과 2를 원소로 가지고있는 literalArray와  생성자 함수인 Array가 있다

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/3435c210-250f-431e-abc2-05fc8da88711' alt=''>

🔎 각각의 디렉터리를 콘솔로 확인해보면 마찬가지로 literalArray의 상속받은 정보들이  
&emsp;Array 생성자 함수의 prototype 속성에서 상속을 받아 생성되었다는 것을 확인할 수 있다  
<br>
🔎 Array를 new 연산자와 함께 호출해서 인스턴스를 생성했을 때도 마찬가지의 결과를 보여줄것이다
{:.notice--info}

<br>

```js
literalArray.push(...);
literalArray.pop(...);
literalArray.map(...);
literalArray.filter(...);
```

🔎 따라서, 인스턴스가 push, pop, map 등의 메서드를 **마치 자신의 것 처럼 호출** 할 수 있다  
<br>
🔎 이러한 Array 객체도 사용자 정의 객체와 동일하게 **__proto__**를 생략해서 사용이 가능하다  
&emsp;Array 생성자 함수의 prototype을 상속받은 인스턴스를 이용해서 호출할경우  
&emsp;**__proto__**가 생략 가능하도록 설계되어있기 때문에 **__proto__**를 생략할 수 있다
{:.notice--info}

<br>

<img class='img' src='https://github.com/D-Sup/BlogStorage/assets/96939334/d92c2c33-21ed-41e0-8b2a-79ce7e495410' alt=''>

🔎 하지만 예외적으로 Array 생성자 함수의 prototype 내부에 존재하지 않는 from이나 isArray 등의 메서드들은   
&emsp;인스턴스를 통해 호출하는 것이 아니라 Array 생성자 함수에서 직접 접근해야 실행이 가능하다
{:.notice--info}

<br>

```js
var arr = [1, 2];
arr.forEach(function (){});
Array.isArray(arr);
arr.isArray(); // TypeError: arr.isArray is not a function
```

> 이런식으로 객체에 **직접적으로 접근**해서 사용할 수 있는 메서드들을 **정적 메서드** 라고한다


💡 왜 이런식으로 메서드를 다른방식으로 접근하도록 만들었을까?  
<br>
&emsp;그 이유 중 하나는 인스턴스와 구분하기 위함인데  
&emsp;정적 메서드들은 생성자 함수 자체에 속하는데, 인스턴스와는 직접적인 연관이 없기 때문에  
&emsp;객체의 인스턴스에서 호출되는 **인스턴스 메서드와 구분하기 위한 목적**이 있기 때문이라고 할 수 있다
{:.notice--primary}
<!-- content -->
