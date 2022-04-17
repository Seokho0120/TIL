# useState

핵심!! useState는 리렌더링이 될 때 새로운 함수를 만들어주며, 각각의 함수는 고유의 state와 props를 기억하고 있습니다.

## 사용법

```javascript
const [상태 값 저장 변수, 상태 값 갱신 함수] = useState(상태 초기 값);

const [counter, setCounter] = useState(0);
```

이런 식으로 사용합니다.

리액트 컴포넌트에서 **동적인 값을 상태(state)** 라고 부릅니다. 사용자가 자신의 웹을 사용하면 저희의 코드에서 특정 값이 바뀌어야 할 때가 있습니다. 만약 사용자가 어떤 버튼을 누르면 값이 증가하는 것처럼 말이지요. 이 때 값을 **'상태값'** 이라고 하며, 컴포넌트의 상태값이 동적으로 바뀔 경우에는 상태를 관리하는 것이 필요합니다.

**React Hooks** 가 나오기 이전에는 상태값을 관리하기 위해 **class 기반의 클래스 컴포넌트**를 작성해야했습니다. class 안에서 this.state 를 써주는 코드를 길게 작성해야 했지요. 클래스 컴포넌트는 간단한 상태 관리 조차도 함수형 컴포넌트에 비해 복잡하여 유지 보수가 힘들었습니다.

하지만 **리액트 16.8 버전부터 Hooks** 라는 기능이 도입되면서 **함수형 컴포넌트에서도 상태를 관리**할 수 있게 됐습니다. Hooks 중에 **useState()** 함수가 있는데, 이를 통해 **함수형 컴포넌트에서도 상태를 관리**할 수 있습니다.

## setState는 새로운 값을 가지고 컴포넌트를 다시 불러줍니다.

```javascript
import React, { useState } from "react";

export default function TestAll() {
  const [count, setCount] = useState(0);

  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + count);
    }, 3000);
  }

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  );
}
```

위의 코드는 간단하게 버튼이 두 개 있습니다. 하나의 버튼은 **count** 라는 상태값을 증가시켜주며, 또 다른 버튼은 count라는 상태값을 **3초 뒤에 alert**해줍니다.

![111](https://user-images.githubusercontent.com/93597794/163713850-d43d106f-9fe7-4110-a1c7-91f23ab13a88.gif)

여기서 다음과 같이 행동을 해봅니다.

- 카운터를 3으로 증가시킵니다. -> "Show alert"버튼을 누릅니다. -> 타임아웃(3초)가 지나기 전에 카운터를 5로 증가시킵니다.

이 때 alert에서는 어떤 값이 나올까요? 결과는 바로 3이 나옵니다!
왜냐하면 **state는 리렌더링이 될 때 아예 새로운 함수를 반환**해주기 때문입니다. 각각의 렌더링에서 함수 안의 count(state)는 상수이자 독립적인 값으로 존재합니다.

```javascript
// 처음 랜더링 시
function Counter() {
  const count = 0; // useState() 로부터 리턴
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + count);
    }, 3000);
  }
  // ...
}

// 클릭하면 함수가 다시 호출된다
function Counter() {
  const count = 1; // useState() 로부터 리턴
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + count);
    }, 3000);
  }
  // ...
}

// 또 한번 클릭하면, 다시 함수가 호출된다
function Counter() {
  const count = 2; // useState() 로부터 리턴
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert("You clicked on: " + count);
    }, 3000);
  }
  // ...
}
```

이런 식으로 함수가 재생성이 되는 겁니다. 각각의 함수에서 count는 독립적인 것입니다.
따라서 count가 3일때 버튼을 눌렀다면, alert를 해주는 함수는 **이 때의 상태값은 count == 3을 기억**하고 있을 것이며, 그 것을 alert해주는 겁니다. 이 것은 실제의 함수에서도 발생하는 상황입니다.

## 실제 함수라고 다를 것 없습니다.

```javascript
function sayHi(person) {
  const name = person.name;
  setTimeout(() => {
    alert("Hello, " + name);
  }, 3000);
}

let someone = { name: "Dan" };
sayHi(someone);

someone = { name: "Yuzhi" };
sayHi(someone);

someone = { name: "Dominic" };
sayHi(someone);
```

이 코드도 순서대로 Dan, Yuzhi, Dominic 이 나옵니다. someone이라는 변수에 객체를 계속 재할당 하고 있기 때문이지요. 이런 식으로 함수가 매개변수로 쓰인 **person이라는 값을 기억**하고 있는 것입니다.

```javascript

```

```javascript

```

```javascript

```

```javascript

```
