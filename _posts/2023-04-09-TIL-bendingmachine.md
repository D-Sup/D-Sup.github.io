---
title: '벤딩머신 기능구현하기'
excerpt_separator: '<!--more-->'
categories:
  - ToyProject
tags:
  - JavaScript
header:
  teaser: ./assets/image/18.png
toc: true
toc_sticky: true
toc_label: 'CONTENTS'
---

<br>

<!-- content -->
# 벤딩머신에 필요한 기능 🎰

> 동작해야할 순서대로 넘버링을 해보자면

<img class='img' src='https://user-images.githubusercontent.com/96939334/230779673-9f1480df-3157-4e0d-9fea-05dc4532ffac.png' alt=''>

<br>
&emsp;1. 소지금과 같거나 적은 금액을 자판기에 입금한다  
<br>
&emsp;2. 거스름돈 반환 버튼을 누를 경우 잔액을 소지금에 반환해줘야 한다  
<br>
&emsp;3. 음료의 재고가 존재해야하고 음료를 선택했을 때 재고가 깎여야한다  
<br>
&emsp;4. 음료를 선택하면 그것에 맞게 장바구니에 추가되어야 한다  
<br>
&emsp;5. 획득 버튼을 누르면 구매할 음료의 총액이 잔액에서 차감되어야 한다    
<br>
&emsp;6. 획득이 정상적으로 이루어지면 장바구니에 있는 요소들이 그대로 추가되고 음료의 총액이 떠야한다  
<br>
{:.notice--primary}

<br>

## 구현

<br>

### 1. 소지금과 같거나 적은 금액을 자판기에 입금한다

⚠️ 입금할 금액이 소지금보다 많거나 아무것도 입력하지 않고 입금을 누를 경우 다르게 동작해야한다
{:.notice--warning}

```js
amount_btn.addEventListener('click', () => {
  if (Number(inputNumber_input.value) > PossessionAmount) {
    alert('가진 금액보다 더 충전할 수 없습니다');
    inputNumber_input.value = '';
  } else if (Number(inputNumber_input.value) <= PossessionAmount && inputNumber_input.value !== '') {
    total += parseInt(inputNumber_input.value);
    totalAmount += parseInt(inputNumber_input.value);
    balanceCount.textContent = total;
    inputNumber_input.value = '';
    PossessionAmount -= totalAmount;
    Possession.textContent = PossessionAmount;
    totalAmount = 0;
  }
});
```

✔️ 가진 금액 내에서 입금을 누를 경우 입금액을 잔액에 해당하는 변수를 재할당 하도록 했다  
✔️ 소지금에서 입금액을 뺀 만큼 소지금에 해당하는 변수에 재할당 하도록 했다  
✔️ 가진 금액 밖으로 입금을 누를 경우 '가진 금액보다 더 충전할 수 없습니다' 라는 알림이 뜨도록 했다
{:.notice--success}

<br>

### 2. 거스름돈 반환 버튼을 누를 경우 잔액을 소지금에 더해줘야 한다

⚠️ 잔액이 없는 상태에서 거스름돈 반환을 누를 경우 다르게 동작해야한다
{:.notice--warning}

```js
return_btn.addEventListener('click', () => {
  if (total > 0) {
    PossessionAmount += total;
    Possession.textContent = PossessionAmount;
    balanceCount.textContent = '0';
    total = 0;
  } else {
    alert('잔액이 없습니다');
  }
});
```

✔️ 잔액이 있는 상태라면 소지금에서 잔액을 더하여 재할당해주고 잔액은 0으로 재할당되도록 했다  
✔️ 잔액이 없는 상태라면 '잔액이 없습니다' 라는 알림이 뜨도록 했다
{:.notice--success}

<br>

### 3. 음료의 재고가 존재해야하고 음료를 선택했을 때 재고가 깎여야한다

⚠️ 재고가 매번 다르게 존재하도록 랜덤하게 재고를 채우고 재고가 다 떨어졌을 때 품절 표시가 떠야한다
{:.notice--warning}

```js
let soldOut = [];
let stock = [];

function randomNumber(i) {
  const random = Math.floor(Math.random() * 10);
  stock.push(random);
  if (random === 0) {
    soldOut.push(i);
  }
  return random;
}

function choose() {
  for (let i = 0; i < 6; i++) {
    items[i].children[3].innerHTML = randomNumber(i);
  }
  for (let i = 0; i < soldOut.length; i++) {
    items[soldOut[i]].lastElementChild.classList.add('out');
  }
}
```

✔️ 0부터 10까지 랜덤한 수를 생성하고 stock이라는 배열에 순서대로 원소(재고)를 추가하도록 했다  
✔️ 랜덤하게 재고가 0이 나올 경우 soldOut이라는 배열에 원소의 index를 원소로 추가하도록 했다  
✔️ soldOut을 순회하면서 index에 해당하는 요소에 'out' 이라는 클래스를 추가해주도록 했다
{:.notice--success}

<br>

### 4. 음료를 선택하면 그것에 맞게 장바구니에 추가되어야 한다

⚠️ 음료를 선택할 때마다 장바구니에 추가가 되고, 재고가 다 떨어질 경우 품절표시가 뜨고 더 이상 선택이 되면 안된다  
⚠️ 음료를 선택한 순서대로 요소가 추가되어야하고 같은 음료를 여러번 선택했을 경우엔 이미 추가된 요소의 갯수가 카운트 되어야한다
{:.notice--warning}

```
let selectCount = [0, 0, 0, 0, 0, 0];

function choose() {
  for (let i = 0; i < 6; i++) {
    items[i].children[3].innerHTML = randomNumber(i);
  }
  for (let i = 0; i < soldOut.length; i++) {
    items[soldOut[i]].lastElementChild.classList.add('out');
  }
  items.forEach(items => {
    items.addEventListener('click', e => {
      const selectItem = e.currentTarget.dataset.id;
      e.currentTarget.classList.add('active');
      calcChoose(selectItem, e);
      if (stock[selectItem] > 0) {
        appendBasket(selectItem, e);
      }
      console.log(stock);
    });
  });
}

function calcChoose(selectItem, e) {
  stock[selectItem] += -1;
  if (stock[selectItem] > 0) {
  } else if (stock[selectItem] === 0) {
    items[selectItem].lastElementChild.classList.add('out');
    appendBasket(selectItem, e);
  }
}

function appendBasket(selectItem, e) {
  let num = selectCount[selectItem] + 1;
  if (selectCount[selectItem] === 0) {
    const element = document.createElement('div');
    element.classList.add('selectItem');
    element.classList.add('translateX-minus');
    const attr = document.createAttribute('data-id');
    attr.value = selectItem;
    element.setAttributeNode(attr);
    element.innerHTML = `
            <img src="${e.currentTarget.children[0].src}" alt="">
            ${e.currentTarget.children[1].textContent}
            <div class="countSelectItem">${num}</div>
            `;
    basket.append(element);
    delay();
  } else {
    const addItems = basket.querySelectorAll('.selectItem');
    addItems.forEach(e => {
      if (e.dataset.id === selectItem) {
        e.lastElementChild.textContent = num;
      }
    });
  }
  selectCount[selectItem] += 1;
}
```
{: .language-js .line-numbers}
✔️ 클릭이 이루어질 떄마다 appendBasket() 함수를 호출하여 장바구니에   
&emsp;선택한 음료의 요소를 추가해주도록하고, selectCount 라는 배열의 선택한 음료에 해당하는 원소가 카운트되도록 했다  
<br>
✔️ 선택한 음료에 해당하는 selectCount 의 원소가 0일 경우 장바구니에 새롭게 추가하도록하고  
&emsp;0이 아닐경우 이미 추가되어있는 요소의 갯수만 재할당해주도록 했다  
<br>
✔️ choose() 함수에 addEventListener() 메서드를 추가해주고 클릭이 이루어질 때마다  
&emsp;calcChoose() 함수를 호출하여 클릭된 요소에 해당하는 stock의 원소의 값을 1씩 감소시키고,  
&emsp;원소의 값이 0에 달했을 때 해당 요소에 'out' 이라는 클래스를 추가해주도록 했다
{:.notice--success}

<br>

### 5. 획득 버튼을 누르면 구매할 음료의 총액이 잔액에서 차감되어야 한다

⚠️ 총액이 잔액보다 많을 경우 다르게 동작해야한다
{:.notice--warning}

```js
submit_btn.addEventListener('click', () => {
  const paymentAmount = selectCount.reduce((a, b) => a + b) * 1000;
  if (paymentAmount <= total) {
    changeMoney = total - paymentAmount;
    PossessionAmount += changeMoney;
    Possession.textContent = PossessionAmount;
    total = 0;
    inputNumber_input.value = '';
    balanceCount.textContent = '0';
  } else {
    alert('잔액이 부족합니다');
  }
});
```

✔️ 총액이 잔액보다 적은 경우 잔액에서 총액을 빼고 소지금에서도 총액을 빼도록 했다  
✔️ 남은 잔액은 그대로 소지금에 더해주고 잔액을 0으로 재할당하도록 했다  
✔️ reduce() 함수를 통해 총액을 구하고 총액이 잔액보다 많은경우 '잔액이 부족합니다' 라는 알림이 뜨도록 했다
{:.notice--success}

<br>

### 6. 획득이 정상적으로 이루어지면 장바구니에 있는 요소들이 그대로 추가되고 음료의 총액이 떠야한다

⚠️ 또 다시 획득을 누를 경우를 대비해서 총금액끼리 더해지도록 해야한다
{:.notice--warning}

```
let finalAmount = 0;

submit_btn.addEventListener('click', () => {
  const paymentAmount = selectCount.reduce((a, b) => a + b) * 1000;
  if (paymentAmount <= total) {
    changeMoney = total - paymentAmount;
    PossessionAmount += changeMoney;
    Possession.textContent = PossessionAmount;
    total = 0;
    inputNumber_input.value = '';
    balanceCount.textContent = '0';
    finalAmount += paymentAmount;
    totalPrice.children[0].textContent = `총금액 : ${finalAmount}`;
    selectCount = [0, 0, 0, 0, 0, 0];
    items.forEach(e => {
      e.classList.remove('active');
    });
    appendEarnedItems();
  } else {
    alert('잔액이 부족합니다');
  }
});

function appendEarnedItems() {
  const selectItem = basket.querySelectorAll('.selectItem');
  selectItem.forEach(e => {
    const element = document.createElement('div');
    element.classList.add('buyItem');
    element.classList.add('translateXX-minus');
    element.innerHTML = e.innerHTML;
    EarnedItems.append(element);
    switchOnOff = false;
    delay();
  });
}
```
{: .language-js .data-line="10,11,12,13,14,15,16"}

✔️ finalAmount 변수를 추가하여 획득을 누를 때마다 총액에 총액이 더해지도록 했다  
✔️ 획득이 이루어진 경우 appendEarnedItems() 함수를 호출하여 장바구니에 있던 음료들을  
&emsp;forEach() 함수로 EarnedItems 요소 안에도 추가하도록 했다
{:.notice--success}

<span class='explain'>이제 구현은 끝..!</span>

<br>

## CSS

> 이제 CSS로 효과를 줘보자

```css
.translateXX-minus {
  transform: translateX(-290px);
}
.translateX-minus {
  transform: translateX(-180px);
}
.translateX-plus {
  transform: translateX(200px);
}
```

✔️ transform을 통해 장바구니에 음료가 추가될 때 왼쪽에서 오른쪽으로 자연스럽게 움직이면서 생성되도록 했다
{:.notice--success}

```js
function delay() {
  const selectItem = basket.querySelectorAll('.selectItem');
  const buyItem = EarnedItems.querySelectorAll('.buyItem');
  if (switchOnOff) {
    setTimeout(() => {
      selectItem.forEach(e => {
        e.classList.remove('translateX-minus');
      });
    }, 100);
  } else {
    setTimeout(() => {
      selectItem.forEach(e => {
        e.classList.add('translateX-plus');
        setTimeout(() => {
          e.classList.remove('translateX-plus');
          basket.innerHTML = '';
          buyItem.forEach(e => {
            e.classList.remove('translateXX-minus');
            switchOnOff = true;
          });
        }, 100);
      });
    }, 100);
  }
}
```

✔️ 획득을 누르면 자연스럽게 화면밖으로 빠져나가면서 장바구니에 있는 음료들은 제거되도록 했다
{:.notice--success}

<span class='explain'>그리하여... 완성!</span>

[URL](https://d-sup.github.io/BendingmachineApp/)

<img class='img' src='https://user-images.githubusercontent.com/96939334/226775062-61613b40-04d8-4eb7-b052-66a8a712a2e2.gif' alt=''>

<details>
  <summary>전체코드</summary><br>
    <img src='https://user-images.githubusercontent.com/96939334/230788711-32924b5a-de74-458e-83c9-03ee65cc06ed.png' alt=''>
</details>

<br>

## 추가해볼 기능

💡 입금을 했을 때의 금액이 음료의 최저가 보다 낮을 경우 소지금에 다시 반환하도록 구현해보기  
💡 음료를 고르다가 거스름돈을 반환했을 때 장바구니에 있은 음료들이 사라지고 재고도 원상태로 돌아오도록 구현해보기
{:.notice--primary}