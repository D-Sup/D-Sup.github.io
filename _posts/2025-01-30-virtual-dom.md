---
title: 'Virtual DOM'
excerpt_separator: '<!--more-->'
categories:
  - Frontend
tags:
  -
header:
  teaser: ./assets/image/59.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>
<!-- https://kr.freepik.com/
https://ye-yo.github.io/thumbnail-maker/ -->
<!--  source ~/.bash_profile   -->
<!-- bundle exec jekyll serve -->

# 가상(Virtual) DOM  
  
  
> 가상 DOM은  
실제 브라우저의 DOM이 아닌  
리액트가 관리하는 가상의 DOM으로  
React는  
실제 DOM을 직접 조작하지않고  
가상 DOM을 활용해서 렌더링을 수행한다.  
>
  
  
<br>
### 실제 DOM을 직접 조작하면 어떤 단점이 있을까?
  
> 브라우저 렌더링 과정을 떠올려보자.  
특정 요소에 노출 여부나 사이즈 변경이 일어나면  
Reflow와 Repaint가 일어나는데  
예를 들어, 변경이 일어나는 요소가  
많은 자식 요소를 가지고 있으면  
자식 요소도 변경해야하기 때문에  
Reflow와 Repaint에 대한 비용이 증가한다.  
즉, 브라우저가 매번 DOM을 다시 계산하고 렌더링해야한다.  
특히 SPA(Single Page Application)에서는 한 페이지 내에서 계속 위치를 재계산해야 하므로 성능 비용이 커진다.  
>
  
  
<br>
### 가상 DOM을 활용해서 렌더링하면 어떤 장점이 있을까?
  
- 가상 DOM은 웹페이지가 표시해야 할 DOM을 일단 메모리에 저장하고 리액트가 실제 변경에 대한 준비가 완료됐을 때 실제 브라우저의 DOM에 반영한다.  
- 가상 DOM에서 변경 사항을 미리 계산하고 변경된 부분만 실제 DOM에 반영하면 여러 번의 렌더링 과정을 압축해 최소한의 렌더링을 할 수 있다.  
  
{:.notice--primary}
💡 **`가상 DOM에 대한 오해`**<br><br>
가상 DOM은 실제 DOM을 관리하는 브라우저보다 빠르다고 생각할 수 있는데  
가상 DOM은 정상적인 애플리케이션을  
개발 할 수 있을 정도로 충분한 속도를 제공하는 것이라고 생각하는게 올바르다.  
렌더링 방식에 있어서 브라우저와 개발자들에게 도움을 줄 수 있는 것이 가상 DOM이라는 것을 기억하자.  
  
  
<br><br><br>
## 가상 DOM의 탄생 배경
  
  
> 앞서 브라우저가 매번 DOM을 다시 계산하고 렌더링해야하는 것이  
가상 DOM 탄생의 이유이기도 하지만  
더 궁극적인 이유가 있다.  
React가 없는 Vanilla JavaScript를 떠올려보자.  
>
  
  
> 개발자는,  
사용자의 인터랙션이 발생했을 때를 대비해서  
인터랙션마다 변경할 요소들에 대해  
DOM 선택자를 사용해서 요소를 찾고  
해당 요소를 업데이트 하는 코드를 직접 작성해야한다.  
>
  
  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vanilla JS Counter</title>
</head>
<body>
    <h1 id="counter">0</h1>
    <button id="increase">+</button>
    <button id="decrease">-</button>

    <script>
        const counterElement = document.getElementById("counter");
        const increaseButton = document.getElementById("increase");
        const decreaseButton = document.getElementById("decrease");

        let count = 0;

        function updateCounter() {
            counterElement.textContent = count; // 직접 DOM을 업데이트해야 함
        }

        increaseButton.addEventListener("click", () => {
            count++; // 상태 변경
            updateCounter(); // 변경된 값으로 UI 갱신
        });

        decreaseButton.addEventListener("click", () => {
            count--; // 상태 변경
            updateCounter(); // 변경된 값으로 UI 갱신
        });
    </script>
</body>
</html>
```
  
> 즉, 어떤 요소를 업데이트하기 위해서  
요소의 변경 사항을 추적해야한다는 것이다.  
이 과정이 많아질 수록 코드는 복잡해지고 유지보수가 어려워지는데  
만약, 요소의 변경 사항을 추적할 필요없이  
변경되어야할 결과값에만 신경쓸 수 있다면  
개발자가 더 쉽게 UI를 관리할 수 있을 것이다.  
>
  
  
```jsx
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0); // 상태(State) 관리

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
    </div>
  );
}
```
  
> 이를 위해 도입된 것이 가상 DOM이다.  
리액트는  
"변경되어야할 결과값"을 상태(State) 개념을 통해 관리하고  
가상 DOM을 통해 DOM 선택자 없이도 자동으로 UI를 업데이트할 수 있는 매커니즘을 가졌다.  
>
  
  
<br><br><br>
## 재조정 (Reconciliation)
  
  
> React는  
실제 DOM과 가상 DOM을 비교해서  
변경된 부분을 식별하고 업데이트하는 과정을 수행한다.  
이 과정을 재조정이라고 한다.  
이 재조정을 수행하기 위해선  
재조정을 수행하는 React의 내부 알고리즘인  
`Reconciler(재조정기)`가 필요하고  
React 버전에 따라 사용되는 재조정기가 다르다.  
>
  
  
<br>
### Stack Reconciler (React 15 이전)
  
  
> 이전 리액트에서는 스택 조정자로,  
하나의 스택에 렌더링에 필요한 작업이 쌓이면 이 스택에서 꺼내 동기적으로 작업이 이루어졌는데 몇가지 문제점이 존재했다.  
>
  
  
<img class='img-full' src='https://github.com/user-attachments/assets/4387e493-2732-4dca-bbc6-d547d5013142' alt=''>
  
- **렌더링 작업 중단 불가**: 재조정 프로세스가 시작되면 전체 컴포넌트 트리를 순회해야했고, 각 컴포넌트 처리를 콜 스택에 추가해서 하위 컴포넌트가 처리될 때까지 이 프로세스는 계속 되었다.  
- **우선순위 처리 불가능**: 모든 업데이트가 동일한 우선순위로 처리되어, 긴급한 업데이트와 중요한 업데이트를 구분할 수 없었다.  
- **비동기 렌더링 지원 X**: 렌더링 작업을 여러 프레임에 걸쳐 분산시킬 수 없어 복잡한 업데이트 시 성능 문제가 발생했다.  
  
<br>
### Fiber Reconciler: Diff + Fiber 알고리즘 (React 16 이후)
  
  
> Fiber는 기존 React의 렌더링 동작을 개선하기 위해 도입된 새로운 아키텍처이다.  
>
  
- 자체적인 스케줄러를 사용한다.  
- 렌더링 작업을 작은 단위로 나눌 수 있다.  
- 작업을 일시 중지하고 나중에 다시 시작하는 것이 가능하다.  
- 다양한 우선순위 레벨을 지원한다.  
- 렌더링이 비동기로 수행된다.  
  
**React의 동작 흐름**  
  
1. **Render 단계** → 가상 DOM을 생성  
2. **Reconcile 단계** → 가상 DOM과 실제DOM을 비교해서 변경 사항을 감지  
3. **Commit 단계** → 실제 DOM 업데이트  
4. **Update 단계** → state나 props가 변경 시 다시 렌더링  
  
<br><br>

1) **`Render 단계`**  
  
- JSX로 작성되어있는 코드는 내부적으로 Babel로 인해 React.createElement()로 자동변환되고  
React.createElement()부분은 객체인 React 엘리먼트로 변환하는데   
이 React 엘리먼트는 가상 DOM 역할을 수행하게 된다.  
      
    ```jsx
    // before
    const App = () => {
      return <h1>Hello, World!</h1>;
    };
    
    // after
    const App = () => {
      return React.createElement("h1", null, "Hello, World!");
    };
    ```
      
    ```jsx
    const element = {
      type: "h1",
      props: {
        children: "Hello, World!"
      }
    };
    ```
      
- `createRoot 함수`가 호출되면 최상위 노드인 root를 기반으로 `FiberRoot` 및 `HostRoot Fiber` 를 생성한다.  
    - **FiberRoot**:  
        - React 애플리케이션을 감싸는 최상위 루트 객체이다.  
        - 렌더링 스케줄링과 우선순위를 관리한다.  
        - 현재 화면에 보이는 트리와 작업 중인 트리인 current 트리와 workInProgress 트리를 추적한다.  
        - 전역적인 상태 관리를 한다.  
    - **HostRootFiber**:  
        - 현재 화면에 렌더링된 트리이다.  
        - 첫 번째 실제 Fiber 노드이다.  
        - 컴포넌트의 부모 역할을 한다.  
        - 업데이트의 시작점이다.  
      
    ```json
    [DOM 레벨]
    div#root (containerInfo)
        │
    [React 레벨]
    FiberRoot
        │
        ├─ current → HostRootFiber
        │                  │
        │                  ├─ stateNode → FiberRoot 참조
        │                  │
        │                  └─ child → AppFiber
        │                                 │
        └─ finishedWork                   └─ 하위 컴포넌트
    ```
      
- `render 함수`가 호출되면 렌더링 프로세스가 시작되는데 이때 리액트 렌더링 모드를 동기 모드(Sync)로 실행할지 비동기 모드(Concurrent)로 실행할지 결정하고 `workInProgress` 트리를 생성한다.  
      
    → React 18 부터 ReactDOM.createRoot()를 사용하면 기본적으로 비동기모드가 활성화되고   
    React 18 이전 버전에서는 항상 동기 모드가 활성화된다.  
      
- `render 함수`가 호출되면 전달받은 App 컴포넌트를 기반으로 `Fiber 노드`를 생성한다.   
그리고 생성된 Fiber 노드는 `HostRootFiber`의 child 속성으로 연결된다.   
이때 `beginWork` 단계로 전환된다.  
- `beginWork` 단계에서 리액트 트리를 위에서 아래로 순회하면서 각 컴포넌트를 기반으로 Fiber 노드를 생성해서 생성된 `workInProgress` 트리의 노드를 처리한다. 더 이상 순회할 자식이없다면 `completeWork` 단계로 전환해서 상위노드로 올라가고 `HostRootFiber`까지 도달하면 `workInProgress` 트리가 완성되는데 완성된 `workInProgress` 트리를 `FiberRoot`의 `finishedWork` 속성에 할당한다.  
새로운 Fiber 트리가 커밋될 준비가 되었으니 `commitRoot` 단계로 전환된다.  
  
- **Fiber 노드**  
      
    > Fiber는 단순한 자바스크립트 객체로  
    React의 각 엘리먼트와 1:1 매칭이된다.  
    즉 엘리먼트 하나당 Fiber 노드가 매칭된다.  
    이런식으로 Fiber는 UI를 값으로 다룰 수 있도록 설계되어 있고  
    이를 통해 React는 비동기적으로 UI 업데이트를 최적화할 수 있다.  
>
  
      
    **주요 속성**  
      
    | 속성 | 설명 |  
    | --- | --- |  
    | `tag` | 해당 Fiber가 Function Component, Class Component, Host Component(DOM 요소)인지 나타냄 |  
    | `stateNode` | 해당 Fiber가 가리키는 실제 DOM 요소 또는 컴포넌트 인스턴스를 의미 |  
    | `child`, `sibling`, `return` | Fiber 간의 관계(부모, 자식, 형제)를 정의할때 사용 |  
    | `pendingProps`, `memoizedProps` | 업데이트 대기 중인 props와 최종적으로 적용된 props를 의미 |  
    | `updateQueue` | 상태 업데이트, 콜백 함수, DOM 업데이트 등의 작업을 관리하는 큐 |  
    | `alternate` | 현재 트리와 작업 중인 트리를 연결하는 속성 (더블 버퍼링을 위한 것) |  
    | `flags` | 현재 Fiber 노드의 효과(Effects)를 나타냄 |  
      
    ```jsx
    function FiberNode(
      this: $FlowFixMe, // Flow 타입 시스템에서 any와 유사한 역할을 하는 타입 (정확한 타입을 명시하지 않음)
      tag: WorkTag, // 해당 Fiber 노드의 유형을 나타내는 태그 (Function Component, Class Component, Host Component 등)
      pendingProps: mixed, // 컴포넌트가 아직 렌더링되지 않은 상태에서 적용될 예정인 props
      key: null | string, // 리액트에서 리스트를 렌더링할 때 요소를 고유하게 식별하는 키
      mode: TypeOfMode, // 해당 Fiber 노드의 모드 (예: 동시 모드, 블록 모드 등)
    ) {
      // === 인스턴스 관련 속성 ===
      this.tag = tag; // Fiber 노드의 유형을 나타냄
      this.key = key; // 이 노드가 속한 리스트의 고유 식별자
      this.elementType = null; // Fiber가 나타내는 요소의 타입 (예: Function Component, Class Component 등)
      this.type = null; // Fiber 노드가 표현하는 실제 타입 (Function, Class, Host 등)
      this.stateNode = null; // DOM 노드 또는 클래스 컴포넌트의 인스턴스를 참조하는 필드
    
      // === Fiber 구조 관련 속성 ===
      this.return = null; // 부모 Fiber 노드를 가리킴
      this.child = null; // 첫 번째 자식 Fiber 노드
      this.sibling = null; // 다음 형제 Fiber 노드
      this.index = 0; // 부모의 children 배열에서의 인덱스
    
      this.ref = null; // ref 속성 (DOM 요소 또는 클래스 인스턴스 참조)
      this.refCleanup = null; // ref가 정리될 때 실행할 함수
    
      this.pendingProps = pendingProps; // 아직 렌더링되지 않은 props
      this.memoizedProps = null; // 마지막으로 렌더링된 props
      this.updateQueue = null; // 이 Fiber에 대한 업데이트가 저장되는 큐
      this.memoizedState = null; // 마지막으로 커밋된 state
      this.dependencies = null; // Context 또는 다른 종속성 관련 정보 저장
    
      this.mode = mode; // Fiber가 실행되는 모드 (예: 동시 모드, 렌더 차단 등)
    
      // === Side Effect 및 렌더링 관련 속성 ===
      this.flags = NoFlags; // 현재 Fiber 노드의 효과(Effects)를 나타냄
      this.subtreeFlags = NoFlags; // 서브트리에서 발생한 효과(Effects)를 나타냄
      this.deletions = null; // 삭제할 Fiber 노드 목록
    
      this.lanes = NoLanes; // 현재 작업을 수행할 우선순위 Lane (스케줄링을 위한 개념)
      this.childLanes = NoLanes; // 자식 노드에서 발생한 업데이트의 우선순위
    
      this.alternate = null; // 현재 Fiber의 이전(대체) Fiber (즉, 이전 렌더 트리와 연결됨)
    }
    ```
      
- **Fiber 트리 생성 및 실행 흐름**  
      
    Fiber 트리의 작동 흐름 → Depth-First Search(깊이 우선 탐색)  
      
    - `beginWork()` → Fiber 트리의 노드를 탐색하면서 새로운 작업을 수행한다.  
    - `completeWork()` → 현재 노드의 작업이 완료되었는지 확인하고, 형제 노드로 이동한다.  
      
    ```jsx
    export default function App() {
      const [count, setCount] = useState(0);
    
      return (
        <div className="App">
          <Title />
          <Content count={count} />
          <LikeButton setCount={setCount} />
        </div>
      );
    }
    ```
      
    <img class='img-full' src='https://github.com/user-attachments/assets/385ff090-0531-4ac2-9d1f-63e3145dc1ab' alt=''>
        
    1. `beginWork(App)` → `App` 컴포넌트의 Fiber 노드가 생성된다.  
        - `useState(0)`을 실행하여 `count`와 `setCount`가 설정된다.  
        - `App` 컴포넌트의 첫 번째 자식 `div.App`으로 이동한다.  
    2. `beginWork(div.App)` → `div`의 Fiber 노드가 생성된다.  
        - 자식이 있으므로 첫 번째 자식 `Title`로 이동한다.  
    3. `beginWork(Title)` → `Title`의 Fiber 노드가 생성된다.  
        - `Title` 에서 자식 들의 Fiber 노드를 생성하고 `completeWork(Title)` 를 실행한다.  
        - 형제 노드인 `Content`로 이동한다.  
    4. `beginWork(Content)` → `Content`의 Fiber 노드가 생성된다.  
        - `count` 값이 props로 전달된다.  
        - `Content` 에서 자식 들의 Fiber 노드를 생성하고 `completeWork(Content)` 를 실행한다.  
        - 형제 노드인 `LikeButton`으로 이동한다.  
    5. `beginWork(LikeButton)` → `LikeButton`의 Fiber 노드가 생성된다.  
        - `setCount` 값이 props로 전달된다.  
        - `LikeButton` 에서 자식 들의 Fiber 노드를 생성하고 `completeWork(LikeButton)` 를 실행한다.  
    6. `completeWork(div.App)` → `div.App`의 모든 자식이 완료되었으므로 완료 처리한다.  
        - 부모인 `App`으로 이동한다.  
    7. `completeWork(App)` → `App`이 완료되어 `HostRootFiber` 까지 도달하면 루트 노드가 완성된다.  
  
<br><br>

2) **`Reconcile 단계`**  
  
  
- `completeWork` 단계에서는 Commit 단계에서 DOM 업데이트를 위해 필요한 정보를 수집한다.   
각 Fiber 노드에 대응하는 DOM 노드가 생성되고 DOM 노드에 필요한 속성인 className, style, 이벤트 리스너 등을 계산한다.  
각 Fiber 노드에 대해 필요한 DOM 업데이트를 나타내는 `flags`가 설정된다.  
모든 설정이 끝나면 `commitRoot`를 실행한다.  
  
<br><br>

3) **`Commit 단계`**  
  
  
> `commitRoot` 단계에서 `finishedWork` 트리를 기반으로 변경사항이 실제 DOM에 적용된다.  
이 단계는 동기적으로 처리된다.  
UI 업데이트가 부분적으로 진행되는 일이 없도록 하고  
DOM과 연관된 부수 효과의 실행 시점을 보장하기 위함 때문이다.  
>
  
- 이때 flags를 기반으로 필요한 DOM업데이트만 수행된다.  
- 모든 준비된 변경사항이 실제 DOM에 적용된다.  
- 준비된 DOM 노드들이 실제 문서에 삽입된다.  
- ref가 설정된다.  
- useLayoutEffect 훅이 실행된다.  
- useEffect 등의 부수 효과가 실행된다.  
      
    커밋이 완료되면 finishedWork는 새로운 current트리가 되고  
    current 트리는 다음 업데이트를 위해 보관된다.  
      
  
<br><br>

4) **`Update 단계`**  
  
  
- state 혹은 props가 변경하거나 상위 컴포넌트가 리렌더링 되는 경우 리렌더링이 트리거 된다.  
- 리액트는 발생한 업데이트를 바로 처리하지않고 `업데이트 큐`에 추가해서 각 업데이트에 대해 우선순위를 설정하고 렌더링 스케줄링을 한다.  
- `current` 트리를 복제해서 새로운 `workInProgress` 트리를 생성한다.   
이때 `workInProgress` 노드의 `alternate` 속성은 기존 `current` 노드를 참조한다.  
- `beginWork` 단계에서 리액트 트리를 순회해서 각 컴포넌트의 변경사항을 확인한다. 이때 `Diffing` 알고리즘을 사용해서 최소한의 업데이트만을 계산한다.  
- `completeWork` 단계에서는 각 Fiber 노드에 대해 flags가 설정된다.  
- `commitRoot` 단계에서 모든 준비된 변경사항이 실제 DOM에 적용된다. 이때 이중 버퍼링이 적용된다.  
  
- **이중 버퍼링 기법**  
      
    Fiber 구조에서는 두 개의 트리가 존재한다.  
      
    1. Current Tree (현재 UI 트리) → 현재 화면에 렌더링된 상태  
    2. Work In Progress Tree (작업 중인 트리) → 새로운 업데이트를 적용하는 중  
      
<img class='img-full' src='https://github.com/user-attachments/assets/b5d1dd15-99ed-44b4-8700-3f3b995eff3f' alt=''>
      
- 새로운 업데이트가 발생하면, 현재 트리(Current Tree)를 복사하여 새로운 트리(Work In Progress Tree)를 생성한다.  
- 모든 변경 작업은 Work In Progress Tree에서 수행되므로, UI가 중간에 깨지는 일이 없다.  
- 업데이트가 완료되면 두 트리를 한 번에 교체(Swap) 하여 빠르게 UI를 갱신한다.  
- 이를 통해 사용자는 불완전한 화면을 보지 않고, 최적화된 렌더링이 가능해진다.  
- **비교 알고리즘 (Diffing Algorithm)**  
      
    > ****두 개의 가상 DOM 트리를 비교해서 변경된 부분만 찾아내는 방식  
>
  
1. 엘리먼트 타입 비교  
    - 다른 타입의 요소는 새로 생성한다  
    비교할 두 개의 요소가 서로 다른 타입이면, 이전 요소를 제거하고 새 요소를 생성한다.  
    - 예를 들어 `<div>`가 `<span>`으로 바뀌면, 기존 `<div>`를 완전히 제거하고 새로운 `<span>`을 만든다.  
            
        ```html
        <div>Hello</div>
        ```
            
        ```html
        <span>Hello</span>
        ```
            
    - 태그가 동일하다면, 기존 요소를 유지한 채 변경된 속성만 업데이트한다.  
    - 예를 들어, `<button>` 요소의 className이 변경되면, 기존 `<button>`을 유지한 채 속성만 바뀐다.  
            
        ```html
        <button className="btn-primary">Click</button>
        ```
            
        ```html
        <button className="btn-secondary">Click</button>
        ```
            
    
<br>

2. key prop 비교  
    - react는 리스트를 비교할 때, 요소의 순서가 변경되었는지, 추가/삭제되었는지를 추적하기 위해 key 값을 사용한다.  
    - key가 없으면, React는 리스트의 모든 요소를 새로 렌더링할 수도 있다.  
    - key를 사용하면 변경된 항목만 찾아서 업데이트할 수 있다.  
  
<br><br><br>
**reference**
  
  
- <a href='https://velog.io/@yiseungyun/%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%9D%98-Fiber%EB%A5%BC-%EB%AA%A8%EB%A5%B4%EB%8A%94-Chill-guy%EC%9D%BC-%EB%95%8C#%EC%B4%88%EA%B8%B0-%EB%A0%8C%EB%8D%94%EB%A7%81' target='_blank'>https://velog.io/@yiseungyun/%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%9D%98-Fiber%EB%A5%BC-%EB%AA%A8%EB%A5%B4%EB%8A%94-Chill-guy%EC%9D%BC-%EB%95%8C#%EC%B4%88%EA%B8%B0-%EB%A0%8C%EB%8D%94%EB%A7%81</a>
- <a href='https://d2.naver.com/helloworld/2690975' target='_blank'>https://d2.naver.com/helloworld/2690975</a>