---
title: '타입스크립트'
excerpt_separator: '<!--more-->'
categories:
  - TypeScript
tags:
  -
header:
  teaser: ./assets/image/46.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 타입스크립트 (TypeScript)

타입스크립트는 미국 마이크로소프트에서 개발한 오픈소스 언어로  
타입스크립트는 자바스크립트에 타입을 부여한 언어이다.  
자바스크립트의 확장된 언어라고 볼 수 있다.

<br><br><br>

## 타입스크립트의 컴파일 과정

> TypeScript는 JavaScript의 한계점을 보완하기 위해 개발된 언어로, 정적 타입 시스템을 지원한다.  
> JavaScript는 동적 타입 시스템으로, 타입을 미리 검사하지 않기 때문에 개발자의 의도치 않은 타입 실수로 인해 치명적인 오류가 발생할 가능성이 높다.  
> 반면, 정적 타입 시스템을 가진 언어(C, Java 등)는 타입을 미리 검사하여 문제 발생 시 즉시 에러를 반환하고 실행을 중단한다.  
> 그러나 모든 변수에 타입을 지정해야 하므로 타이핑 양이 많아지는 단점이 있다.

> TypeScript는 이러한 문제를 해결하기 위해 '점진적 타입 시스템'을 채택하였다.  
> 이는 정적 타입 시스템과 동적 타입 시스템의 장점을 결합하여,  
> 실행 전 타입을 검사하면서도 변수에 초기값을 기준으로 자동으로 타입을 추론하는 유연함을 제공한다.

<br>
### TypeScript의 동작 원리
  
> 대다수의 프로그래밍 언어는 코드를 컴퓨터가 해석하기 쉬운 기계어(바이트 코드) 형태로 변환하는 과정을 거치며, 이를 컴파일이라고 한다. 컴파일러는 프로그래밍 언어를 바로 바이트 코드로 변환하는 것이 아니라, 먼저 AST(추상 문법 트리)라는 형태로 변환한다. AST는 코드의 공백이나 주석 등 실행에 관계없는 요소들을 제거하고 트리 형태로 저장한 자료구조이다.  
>
  
  
<br>
### 컴파일 과정
  
타입스크립트 코드 → AST 추상 문법 트리 변환 → 타입 검사(실패 시 컴파일 종료) → 자바스크립트 언어로 변환 → 안전한 자바스크립트 코드 → 실행.  
  
1. **타입스크립트 코드 작성**: 개발자는 TypeScript 코드를 작성한다.  
2. **AST 변환**: TypeScript 코드는 AST로 변환된다.  
3. **타입 검사**: AST를 기반으로 타입 검사가 수행된다. 이때 타입 오류가 발견되면 컴파일이 중단된다.  
4. **JavaScript로 변환**: 타입 오류가 없는 경우, AST는 JavaScript 코드로 변환된다.  
5. **실행**: 생성된 JavaScript 코드는 Node.js나 웹 브라우저에서 실행된다.  
  
이러한 과정 덕분에 TypeScript로 작성된 코드는 타입 검사를 통과한 안전한 JavaScript 코드로 변환되며,   
런타임에서 발생할 수 있는 타입 오류의 가능성이 낮아진다.  
  
<br><br><br>
## 타입스크립트를 사용하는 이유
  
  
<br>
### 1. 에러의 사전 방지
  
```jsx
function sum(a, b) {
  return a + b;
}

sum(10, '20');

````

코드를 실제로 실행하기 전에 미리 에러를 어느 정도 검출 할 수 있다는 것이다.

만약 위와 같은 코드가 있다면
결과 값으로 문자열 1020 이 나올 것이다.
하지만 만약 아래와 같이 함수에 넘길 수 있는 인자의 타입이 정해져 있었다면 문제되지 않았을 것이다.

```tsx
function sum(a: number, b: number) {
  return a + b;
}

sum(10, '20'); // Error: '20'은 number에 할당될 수 없습니다.
````

sum() 함수의 내용을 모르더라도  
sum() 함수를 호출할 때 인자의 타입이 맞지 않으면 개발 툴에서 미리 경고를 해준다.  
작성자에게 의도치 않은 결과가 나올 수 있다는 것을 알려주기 때문에  
사소한 실수를 미연에 방지할 수 있다.  
실제로 웹 서비스를 개발하다보면 사소한 실수가 모여  
애플리케이션 동작을 멈추게 하는 치명적인 버그가 되곤 하는데,  
이를 미리 막을 수 있다는 것이 큰 장점이다.

<br>
### 2. 코드 가이드 및 자동완성
  
코드를 작성할 때 개발 툴의 기능을 최대로 활용할 수 있다는 것이다.  
  
우리가 많이 사용하는 개발도구 Visual Studio Code 는  
타입스크립트와 마찬가지로 마이크로소프트에서 개발했고,  
Visual Studio Code는 Electron 이라는 프레임워크로 제작되어  
내부적으로 화면을 구성하기 위해 HTML, CSS, TypeScript 로 작성되어있다.  
이런 이유 떄문인지 현존하는 통합 개발 환경 중에서 타입스크립트를 가장 잘 지원한다.  
  
```tsx
function sum(a, b) {
  return a + b;
}
var total = sum(10, 20);
total.toLocaleString();
```
  
sum() 함수의 결과를 total 변수에 담은 후  
total.toF 까지 입력하면 비주얼 스튜디오 코드에서 toFixed() API 정보를 제공해줌과 동시에  
tab으로 키워드를 자동 완성하도록 도와주는 것을 확인 할 수 있다.  
sum() 함수는 숫자를 2개 받아 더한 후 반환해주기 때문에 반환 타입이 number로 추론되어  
Visual Studio Code는 total 변수의 타입은 number이라고 인식하면서  
.을 찍었을 때 자바스크립트 Number 타입에서 사용할 수 있는 내장 API를 모두 보여준 것이다.  
이러한 기능을 인텔리센스(IntelliSense)라고 한다.  
이러한 인텔리센스 기능을 지원받아 빠르고 정확하게 코드를 작성해 나갈 수 있어서  
개발 생산성을 향상 시킬 수 있다.  
  
<br><br><br>
## 타입스크립트의 단점?
  
  
- **적지 않은 학습 비용**: 자바스크립트에 익숙한 개발자라 하더라도,  
타입스크립트는 높은 학습 곡선이 있기 때문에 초기 학습 과정에서 어려움을 겪을 수 있다.  
- **늘어나는 코드량**: 정적 타입 시스템을 사용하기 때문에 변수 및 함수 등에 타입을 지정해야 한다. 이로 인해 코드 양이 증가할 수 있고, 초기 구현 시간이 더 걸릴 수 있다.  
- **길어지는 컴파일 시간**: 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 파일을 한번 변환해줘야하는 컴파일 과정을 거쳐야 한다. 이로 인해 개발 중 컴파일 시간이 길어질 수 있고, 특히 대규모 프로젝트에서는 컴파일 시간이 더욱 길어질 수 있다.  
- **타입 오류의 발생 가능성**: 변수나 함수 등의 타입이 맞지 않을 경우 TypeError 오류가 발생하게 되는데 모든 오류를 찾아내기 어려운 경우 생산성이 낮아질 수 있다.  
  
<br><br><br>
## 프로젝트에 TS 적용하기
  
  
1. 타입스크립트 프로젝트 환경 구성  
    - 프로젝트를 생성한 후, NPM 초기화 명령어로 package.json 파일을 생성한다.  
    - `npm i typescript -D` 명령어로 타입스크립트 라이브러리를 설치한다.  
    - tsconfig.json 파일을 생성하고 아래와 같은 기본 설정을 추가한다:  
          
        ```json
        {
        "compilerOptions": {
        "allowJs": true,
        "target": "ES5",
        "outDir": "./dist",
        "moduleResolution": "Node",
        "lib": ["ES2015", "DOM", "DOM.Iterable"]
        },
        "include": ["./src/**/*"],
        "exclude": ["node_modules", "dist"]
        }
        ```
          
    - 기존 자바스크립트 파일을 타입스크립트 파일로 변환하고, tsc 명령어로 컴파일한다.  
  
1. 엄격하지 않은 타입 환경에서 프로젝트 돌려보기  
    - 테스트 코드가 있다면 먼저 통과하는지 확인한다.  
    - 모든 자바스크립트 파일을 타입스크립트 파일로 변경하고, 컴파일 에러를 수정한다.  
    - 기능 변경 없이 테스트 코드가 성공하는지 확인한다.  
  
1. 명시적인 any 선언하기  
    - noImplicitAny: true를 설정 파일에 추가한다.  
    - 가능한 모든 곳에 적절한 타입을 적용하고, 필요한 경우 any 타입을 명시적으로 선언한다.  
    - 라이브러리를 사용할 경우 DefinitelyTyped에서 관련 타입을 설치한다.  
2. strict 모드 설정하기  
      
    ```json
    {
      "compilerOptions": {
        "strict": true, // 모든 엄격한 타입 검사를 활성화한다.
        "strictNullChecks": true, // null 및 undefined를 다른 타입과 구분하여 처리한다.
        "strictFunctionTypes": true, // 함수 타입의 매개변수와 반환값에 대한 엄격한 검사를 수행한다.
        "strictBindCallApply": true, // bind, call, apply 메서드의 매개변수에 대한 엄격한 검사를 수행한다.
        "strictPropertyInitialization": true, // 클래스의 속성이 생성자에서 초기화되었는지 확인한다.
        "noImplicitThis": true, // this의 타입이 명시적으로 지정되지 않은 경우 오류를 발생시킨다.
        "alwaysStrict": true // 모든 파일을 엄격 모드로 처리한다.
      }
    }
    
    ```
      
    - any 타입을 더 적절한 타입으로 변환하고, as 키워드 사용을 최소화한다.  
  
<br><br><br>
## 기본 타입
  
  
변수나 함수를 정의할 때 사용하는 기본 타입은 크게 다음 12가지가 있다.  
  
- string  
- number  
- boolean  
- object  
- array  
- tuple  
- enum  
- null  
- undefined  
- any  
- void  
- never  
  
{:.notice--primary}
💡 **`타입스크립트를 처음쓰면 가장 많이 하는 실수`**<br><br>
any 타입은 아무 데이터나 취급하겠다는 의미로  
타입스크립트에서 자바스크립트의 유연함을 취하고 싶을떄 사용하는 타입이다.  
<br>
<br>
```tsx
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];

````
<br>
보통 타입스크립트를 처음 시작하는 사람들에게 유용한 타입이지만
Type없이 any만 남발하고 쓸데없이 코드 수를 늘린다면
TS의 장점은 안쓰고 단점만 쓰는 꼴이 되는 것이다.


{:.notice--primary}
💡 **`unknown`**<br><br>
unknown 타입은 이름처럼 무엇이 할당될지 아직 모르는 상태의 타입을 말한다.
any 타입과 유사하게 모든 타입의 값이 할당될 수 있다. 그러나 any를 제외한 다른 타입으로 선언된 변수에는 unknown 타입 값을 할당할 수 없다.
<br>
<br>
```tsx
let unknownValue: unknown;

unknownValue = 100; // any 타입과 유사하게 숫자이든
unknownValue = "hello" // 문자열이든
unknownValue = () => console.log("this is any type"); // 함수이든 상관없이 할당 가능하지만

let anyValue: any = unknownValue // (O) any 타입으로 선언된 변수를 제외한 다른 변수는 모두 할당 불가
let stringValue: string = unknownValue // (X) Type 'unknown' is not assignable to type 'string'.ts(2322)
let numberValue: number = unknownValue // (X) Type 'unknown' is not assignable to type 'number'.ts(2322)

````

<br>
any 타입과 unknown 타입의 차이는 무엇일까?  
any 타입은 어떤 타입(never 제외)으로도 할당이 가능하며  
unknown 타입은 any 타입 외에 다른 타입으로 할당이 불가능하다  
unknown 타입은 any 타입과 유사하지만 타입 검사를 강제하고 타입이 식별된 후에 사용할 수 있기 때문에 any 타입보다 더 안전하다.  
따라서  타입을 알기 어려운 데이터를 다루는 경우, any보다 타입 안정성이 강한 unknown을 사용하는 것이 좋다.  
  
  
<br><br><br>
## 타입 (type) 과 인터페이스 (interface)
  
  
> 타입스크립트에서 타입과 인터페이스는 객체의 구조를 정의하는 두 가지 주요 방법이다.  
interface는 주로 객체의 구조를 설명하는 데 사용되며, 확장(상속)이 가능하다는 특징이 있다.  
반면, Type은 더 다양한 형태의 타입을 정의할 수 있으며, 교차 타입(&)과 유니온 타입(|)을 통해 복잡한 타입 구성을 지원한다.  
따라서 Interface는 객체 지향적인 설계에서,   
Type은 더 유연한 타입 조합이 필요할 때 사용하는 것이 좋다.  
>
  
  
<br>
### 기본 개념
  
- 타입: 변수, 함수 반환 값, 매개변수 등의 타입을 정의하는 데 사용된다.  
type 키워드를 사용하여 정의한다.  
      
    ```tsx
    type User = {
        name: string;
        age: number;
    };
    ```
      
- 인터페이스: 객체의 구조를 정의하고, 클래스가 특정 구조를 따르도록 강제하는 데 사용된다. interface 키워드를 사용하여 정의한다.  
      
    ```tsx
    interface User {
        name: string;
        age: number;
    }
    ```
      
  
<br>
### 타입과 인터페이스의 차이점
  
1. 병합 가능성  
    - 인터페이스: 동일한 이름으로 여러 번 선언할 수 있으며, 이 선언들은 자동으로 병합된다.  
          
        ```tsx
        interface User {
            name: string;
        }
        interface User {
            age: number;
        }
        // 병합된 User 인터페이스
        ```
          
    - 타입: 동일한 이름으로 여러 번 선언할 수 없으며, 병합되지 않는다.  
          
        ```tsx
        type User = {
            name: string;
        };
        type User = {
            age: number;
        }; // 오류 발생
        ```
          
2. 클래스 강제 기능  
    - 인터페이스: 클래스가 특정 구조를 따르도록 강제할 수 있다.  
          
        ```tsx
        interface User {
            name: string;
            age: number;
        }
        
        class Person implements User {
            name: string;
            age: number;
        
            constructor(name: string, age: number) {
                this.name = name;
                this.age = age;
            }
        }
        
        const person = new Person('Alice', 25);
        ```
          
    - 타입: 클래스의 구조를 정의할 수 있지만, 타입은 클래스가 특정 구조를 따르도록 강제할 수 없다.  
          
        ```tsx
        type User = {
            name: string;
            age: number;
        };
        
        class Person {
            name: string;
            age: number;
        
            constructor(name: string, age: number) {
                this.name = name;
                this.age = age;
            }
        }
        
        const person: User = {
            name: 'Bob',
            age: 30
        };
        ```
          
  
<br>
### 타입과 인터페이스의 사용 사례
  
- 인터페이스: 객체의 구조를 정의하고, 클래스가 특정 구조를 따르도록 강제하는 데 사용된다.  
      
    ```tsx
    interface User {
        name: string;
        age: number;
    }
    class Person implements User {
        name: string;
        age: number;
    
        constructor(name: string, age: number) {
            this.name = name;
            this.age = age;
        }
    }
    ```
      
- 타입: 변수, 함수 반환 값, 매개변수 등의 타입을 정의하는 데 사용된다.  
      
    ```tsx
    // 타입 정의
    type ID = string | number; // 유니온 타입
    type EmployeeInfo = {
        id: ID;
        position: string;
    };
    
    // 교차 타입
    type FullTimeEmployee = EmployeeInfo & {
        salary: number;
    };
    
    // 함수 예시
    function getEmployeeInfo(employee: FullTimeEmployee): string {
        return `직원 ID: ${employee.id}, 직책: ${employee.position}, 연봉: ${employee.salary}`;
    }
    
    // 사용 예시
    const emp = new Employee("홍길동", 30, "개발자");
    emp.greet();
    
    const fullTimeEmp: FullTimeEmployee = {
        id: 12345,
        position: "개발자",
        salary: 5000,
    };
    
    console.log(getEmployeeInfo(fullTimeEmp));
    ```
      
  
<br>
### 정리
  
  
**인터페이스**  
  
- 병합 가능성으로 인해 확장성과 재사용성이 높다.  
- 클래스 구조 강제 기능 제공한다.  
  
**타입**  
  
- 유니언 타입, 튜플, 리터럴 타입 등 다양한 타입 정의가 가능하다.  
- 복잡한 타입 시스템 구현에 유리하다.  
  
타입은 주로 변수와 함수의 타입을 정의하는 데 사용되며,  
인터페이스는 객체의 구조를 정의하고 클래스의 구조를 강제하는 데 사용된다.  
  
<br><br><br>
## 제너릭 (Generic)
  
  
> 제네릭(Generic)은 타입스크립트의 강력한 기능 중 하나로,  
다양한 타입에 대해 재사용 가능한 컴포넌트를 작성할 수 있게 해준다.  
제네릭은 타입을 매개변수로 받아서,  
코드 작성 시점(함수, 클래스, 인터페이스를 정의할 때)에 사용자가 원하는 타입을 지정할 수 있다.  
>
  
  
> 이를 통해 다양한 타입을 처리하여   
코드의 중복을 줄이고 타입 안전성을 높일 수 있다.  
또한 제네릭은 타입스크립트의 타입 추론 기능과 결합되어  
더욱 강력한 타입 검사를 제공한다.  
>
  
  
```tsx
function identity<T>(arg: T): T {
    return arg;
}
```
  
```tsx
let output = identity<string>('Hello, TypeScript!');

let output = identity('Hello, TypeScript!');

````

<br>
### 제네릭 클래스와 인터페이스

> 제네릭은 함수뿐만 아니라 클래스와 인터페이스에서도 사용할 수 있다.
>


```tsx
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = (x, y) => x + y;
````

```tsx
interface GenericIdentityFn<T> {
  (arg: T): T;
}

function identity<T>(arg: T): T {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

<br>
### 제네릭 제약 조건
  
> 제네릭은 타입 매개변수에 제약 조건을 추가할 수 있다.   
예를 들어, 특정 속성을 가진 타입만 허용할 수 있다.  
>
  
  
```tsx
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
console.log(arg.length);
return arg;
}

````

```tsx
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };
let a = getProperty(x, 'a'); // 1
````

<br>
### 제네릭의 활용 예제
  
> 제네릭을 활용한 예제로 배열을 생성하는 함수를 작성할 수도 있다.  
>
  
  
```tsx
function createArray<T>(length: number, value: T): T[] {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

let stringArray = createArray<string>(3, 'Hello');
console.log(stringArray); // ['Hello', 'Hello', 'Hello']

let numberArray = createArray<number>(3, 42);
console.log(numberArray); // [42, 42, 42]

````

<br><br><br>
## 타입 추론(Type Inference)


> 타입스크립트의 타입 추론은 코드 작성 시 타입을 명시하지 않아도 타입스크립트가 자동으로 타입을 추론해주는 기능이다. 왜냐하면 타입스크립트는 변수나 함수의 초기값을 기반으로 타입을 추론하기 때문이다.
>


```tsx
const x = 10; // 타입스크립트는 x의 타입을 number로 추론한다.
````

타입 추론은 코드의 간결성을 유지하면서도 타입 안전성을 확보할 수 있게 해준다.  
타입 추론은 함수의 반환 타입을 추론할 때도 유용하다.  
왜냐하면 함수의 반환 타입을 명시하지 않아도 타입스크립트가 자동으로 반환 타입을 추론해주기 때문이다.

```tsx
function add(a: number, b: number) {
  return a + b; // 타입스크립트는 반환 타입을 number로 추론한다.
}
```

위와 같이 함수의 반환 타입을 타입스크립트가 자동으로 추론한다.  
이와 같이 타입 추론을 통해 타입 안전성을 확보할 수 있어서  
코드에서 발생할 수 있는 오류를 사전에 방지할 수 있다.

<br><br><br>

## 타입 호환(Type Compatibility)

> 타입스크립트의 타입 호환은 서로 다른 타입 간의 호환성을 판단하는 기능이다.  
> 이는 주로 변수에 값을 할당하거나 함수의 인자를 전달할 때 발생한다.  
> 타입스크립트는 구조적 타이핑을 사용하여 타입 호환성을 결정하기 때문에,  
> 타입의 이름이 아닌 구조를 기반으로 호환성을 판단한다.

```tsx
interface Person {
  name: string;
  age: number;
}

// Person 타입의 객체
const person: Person = { name: 'Alice', age: 30 };

// 타입 호환성: 객체 리터럴이 Person 인터페이스와 호환됨
const anotherPerson = { name: 'Bob', age: 25 }; // Person 타입과 호환된다.

// 추가 속성을 가진 객체도 호환됨
const extraPerson = { name: 'Charlie', age: 28, city: 'Seoul' }; // Person 타입과 호환된다.

// 함수에 타입을 전달할 때
function greet(person: Person) {
  console.log(`Hello, ${person.name}`);
}

greet(person); // 정상 작동
greet(anotherPerson); // 정상 작동
greet(extraPerson); // 정상 작동, 추가 속성은 무시됨
```

타입 호환은 코드의 유연성을 높이고, 다양한 타입을 쉽게 사용할 수 있게 해준다.  
또한, 타입 호환성을 통해 객체의 속성을 재사용할 수 있어 코드의 재사용성을 높인다.  
이와 같이 타입 호환성을 활용하면, 다양한 타입의 객체를 효과적으로 다룰 수 있다.

<br><br><br>

## 타입 별칭(Type Alias)

> 타입스크립트의 타입 별칭은 특정 타입에 이름을 부여하여 재사용할 수 있게 해주는 기능이다. 이를 통해 복잡한 타입을 간단하게 표현할 수 있으며, 코드의 가독성을 높일 수 있다.

```tsx
type Point = {
  x: number;
  y: number;
};

const point: Point = { x: 10, y: 20 };
```

타입 별칭은 특히 복잡한 객체 타입이나 유니온 타입을 정의할 때 유용하다.  
타입 별칭을 사용하면 코드의 중복을 줄이고, 타입을 명확하게 정의할 수 있다.  
이와 같이 타입 별칭을 활용하면, 코드의 유지보수성을 높일 수 있다.

<br><br><br>

## 타입 단언(Type Assertion)

> 타입스크립트의 타입 단언은 개발자가 특정 값의 타입을 명시적으로 지정할 수 있게 해주는 기능이다.  
> 이는 타입스크립트가 자동으로 추론한 타입이 아닌,  
> 개발자가 의도한 타입으로 강제할 수 있다.

```tsx
const someValue: any = 'Hello, TypeScript!';
const strLength: number = (someValue as string).length; // 타입 단언을 사용하여 string으로 간주한다.
```

타입 단언은 주로 외부 라이브러리나 API에서 반환된 값의 타입이 불확실할 때 유용하다.  
이를 통해 개발자는 타입스크립트의 타입 시스템을 보다 유연하게 활용할 수 있다.  
이와 같이 타입 단언을 사용하면, 코드의 타입 안전성을 유지하면서도 필요한 경우 타입을 조정할 수 있다.

<br><br><br>

## 타입 가드(Type Guard)

> 타입스크립트의 타입 가드는 특정 값의 타입을 좁히거나 확인할 수 있게 해주는 기능이다. 이를 통해 조건문을 사용하여 변수의 타입을 확인하고, 해당 타입에 맞는 로직을 실행할 수 있다.

```tsx
function isString(value: any): value is string {
  return typeof value === 'string';
}

const value: any = 'Hello, TypeScript!';
if (isString(value)) {
  console.log(value.length); // value는 string 타입으로 간주된다.
}
```

타입 가드는 코드의 안전성을 높이고, 런타임에서 발생할 수 있는 오류를 줄여준다.  
또한, 타입 가드를 사용하면 코드의 가독성을 높이고, 다양한 타입을 효과적으로 처리할 수 있다.  
이와 같이 타입 가드를 활용하면, 코드의 안정성을 확보할 수 있다.

<br><br><br>
**reference**

- <a href='https://joshua1988.github.io/ts/' target='_blank'>https://joshua1988.github.io/ts/</a>
- <a href='https://www.daleseo.com/tsc/' target='_blank'>https://www.daleseo.com/tsc/</a>
- <a href='https://velog.io/@lhj5924/any-%EC%99%80-unknown-type-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80' target='_blank'>https://velog.io/@lhj5924/any-%EC%99%80-unknown-type-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80</a>
- <a href='https://www.youtube.com/watch?v=VJ8rvsw2j5w&list=PLJf6aFJoJtbUXW6T4lPUk7C66yEneX7MN' target='_blank'>https://www.youtube.com/watch?v=VJ8rvsw2j5w&list=PLJf6aFJoJtbUXW6T4lPUk7C66yEneX7MN</a>
