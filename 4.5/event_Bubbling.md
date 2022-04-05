## 이벤트 버블링(Event bubbling) & 이벤트 캡쳐(Event Capture)

![image](https://user-images.githubusercontent.com/93597794/161520557-42db5060-21b7-4c02-b99e-7b1bae7c7b84.png)

### 이벤트 버블링(Event bubbling)

이벤트를 처리할 때 여러 요소에서 이벤트를 처리할 수 있는데, 이때 **이벤트가 일어난 요소에서 부터 최상단 document 까지 거슬러 올라가는 방법**을 **이벤트 버블링**이라고 합니다.

<br>

아래의 코드는 세 개의 div 태그에 모두 클릭 이벤트를 등록하고 클릭 했을 때 logEvent 함수를 실행시키는 코드입니다. 여기서 최하위 div 태그 `<div class="three"></div>` 를 클릭하면 아래와 같은 결과가 실행됩니다.

```javascript
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>;

var divs = document.querySelectorAll("div");
divs.forEach(function (div) {
  div.addEventListener("click", logEvent);
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

![image](https://user-images.githubusercontent.com/93597794/161520952-ae8940db-4206-4c19-bb35-08a4af67fe90.png)

div 태그 한 개만 클릭했을 뿐인데 왜 3개의 이벤트가 발생되는 걸까요?
<br> 그 이유는 브라우저가 이벤트를 감지하는 방식 때문입니다. 브라우저는 특정 화면 요소에서 이벤트가 발생했을 때 그 이벤트를 최상위에 있는 화면 요소까지 이벤트를 전파시킵니다. 따라서, 클래스 명 three -> two -> one 순서로 div 태그에 등록된 이벤트들이 실행됩니다. 마찬가지로 two 클래스를 갖는 두 번째 태그를 클릭했다면 two -> one 순으로 클릭 이벤트가 동작하겠죠.

<br> 
여기서 주의해야 할 점은 각 태그마다 이벤트가 등록되어 있기 때문에 상위 요소로 이벤트가 전달되는 것을 확인할 수 있습니다. 만약 이벤트가 특정 div 태그에만 달려 있다면 위와 같은 동작 결과는 확인할 수 없습니다.

<br>

이와 같은 하위에서 상위 요소로의 이벤트 전파 방식을 **이벤트 버블링(Event Bubbling)**이라고 합니다.

### 이벤트 캡쳐(Event Capture)

**이벤트 버블링과 반대 방향, 가장 먼 조상부터 시작하는 방법**을 **이벤트 캡쳐링**이라고 합니다.

<br>

특정 이벤트가 발생했을 때 최상위 요소인 body 태그에서 해당 태그를 찾아 내려갑니다. 그럼 이벤트 캡쳐는 코드로 어떻게 구현할 수 있을까요? `addEventListener()` API에서 옵션 객체에 `capture:true` 를 설정해주면 됩니다. 그러면 해당 이벤트를 감지하기 위해 이벤트 버블링과 반대 방향으로 탐색합니다.

```javascript
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>;

var divs = document.querySelectorAll("div");
divs.forEach(function (div) {
  div.addEventListener("click", logEvent, {
    capture: true, // default 값은 false입니다.
  });
});

function logEvent(event) {
  console.log(event.currentTarget.className);
}
```

따라서, 아까와 동일하게 `<div class="three"></div>` 를 클릭해도 아래와 같은 결과가 나타납니다.

![image](https://user-images.githubusercontent.com/93597794/161521402-32facc16-d72e-4e7b-b58c-918a85abe63f.png)
