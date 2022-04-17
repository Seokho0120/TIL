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

이런 식으로 함수가 재생성이 되는 겁니다. 각각의 함수에서 count는 독립적인 것입니다. <br>
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

## 상태는 리렌더링의 조건이자 함수의 상수

![image](https://user-images.githubusercontent.com/93597794/163714252-d3fcc7d3-d454-413e-ab23-98cd0adb4a8d.png)

제목이 무슨 말인지 살펴봅시다. 위 사진이 바로 컴포넌트가 없데이트 되어서 화면에 보이기까지의 순서를 담은 것입니다. <br>
여기서 **초록색 박스** 안이 이야기하는 부분입니다. 함수에서 props가 바뀌거나 state가 바뀌면 **shouldComponentUpdate** 과정이 진행됩니다. 여기서 만약 새로 업데이트 할 필요가 없다면 하지 않겠죠. 하지만 업데이트를 해야한다면 바로 render로 넘어가는 것입니다!

## prevState를 잘 사용해보자!

useState의 핵심이자, 이 내용을 공부하려고 여기까지 정리한것입니다.
**setState 함수는 비동기 함수**입니다. 2번에서 새로운 컴포넌트를 불러오는 과정에서 원래의 함수가 계속 진행이 되었던 것도 결국에는 비동기함수이기 때문에 가능했던 것입니다. <br>
자 그럼 비동기 함수이기 때문에 발생하는 문제점가 있습니다. 아래의 코드를 봅시다.

```javascript
import React, { useState } from "react";

export default function TestAll() {
  const initialNum = 0;
  const [num, setNum] = useState(initialNum);

  const IncrementByFive = () => {
    for (let i = 0; i < 5; i++) {
      setNum(num + 1);
      console.log(num);
    }
  };

  return (
    <div>
      <p>Counter: {num}</p>
      <button onClick={() => setNum(initialNum)}> Reset </button>
      <button onClick={() => setNum(num + 1)}> Increment </button>
      <button onClick={() => setNum(num - 1)}> Decrement </button>
      <button onClick={IncrementByFive}> Increment By 5 </button>
    </div>
  );
}
```

**증가버튼**과 **감소버튼**, 그리고 **한 번에 5개를 올려주는 버튼**이 있습니다. onClick 함수의 반복문 안에서 setState를 해주고 있습니다.(setState = setNum) <br>
자 그러면 이 코드의 결과를 예측해봅시다. 과연 우리가 원하는 대로 나올까요?

![image](https://user-images.githubusercontent.com/93597794/163714527-51e286f8-b573-4849-a85a-848a5eb7723c.png)

는 전혀 아닙니다.. 그냥 1씩 증가하는 모습이 보입니다. 이는 **setState가 비동기함수이기 때문**입니다. 이 때 컴포넌트에서 어떤 상황이 일어나는지 봅시다.

![image](https://user-images.githubusercontent.com/93597794/163714559-ab32e577-ee78-4b26-b04d-d1cc055cef91.png)

기존 함수는 원래의 진행방향으로 가고 있습니다. 첫 번째로 보이는 페이지에서 num을 변화시켜 리렌더링 시켰으니, 두 번째 페이지가 나오겠죠? 그런데 위에서 적어준 코드는 setState(1 + 1)을 다섯번 해준거나 마찬가지입니다. 이는 첫 번째 보이는 페이지의 num이 1이라고 한다면, 이 때 반복문을 다섯번 돌아주기 때문입니다.

setState(num + 1)이 다섯번 돌아간다는 것은, setState(1 + 1) 이 다섯번 돌아감과 동시에(!!!) 업데이트된 페이지를 렌더링 해줍니다. **(비동기함수이기 때문에 반복문과 업데이트가 동시에 진행됩니다!)** 따라서 setState(2) 가 다섯번 반복되니 처음 setState말고는 update를 할 필요가 없기 때문에 num이 2인 상태로 렌더링이 되고 마는것이죠.

### 해결 방법

**prevState** 사용해서 해결하기! <br>
이는 setState에서 **콜백함수를 사용하면서 해결이 가능**합니다. 위의 코드에서는 아래와 같이 사용합니다.

```javascript
setNum((prevNum) => prevNum + 1);
```

변수명은 중요하지 않습니다. 이 **콜백함수에서의 파라미터는, 이전 state값을 참조**합니다. 코드를 아래와 같이 바꾸어봅시다.

```javascript
import React, { useState } from "react";

export default function TestAll() {
  const initialNum = 0;
  const [num, setNum] = useState(initialNum);

  const IncrementByFive = () => {
    for (let i = 0; i < 5; i++) {
      setNum((prevNum) => prevNum + 1); // 콜백함수로 수정
    }
  };

  return (
    <div>
      <p>Counter: {num}</p>
      <button onClick={() => setNum(initialNum)}> Reset </button>
      <button onClick={() => setNum(num + 1)}> Increment </button>
      <button onClick={() => setNum(num - 1)}> Decrement </button>
      <button onClick={IncrementByFive}> Increment By 5 </button>
    </div>
  );
}
```

크게 바뀐 것 없습니다. 다만 setState을 다르게 사용한 것이죠. 그런데 효과는 굉장했습니다.
드디어 작동합니다! 이 것은 setState의 **콜백함수 인자에서 전에 사용하던 state을 가져왔기 때문에 가능**한 것입니다.

![img (1)](https://user-images.githubusercontent.com/93597794/163715052-e1d6035b-27dc-4a66-bf9a-09481e4cac8d.gif)

이런 식으로 원래 컴포넌트에서 setState가 다섯번 돌아간다고 해도, 이전의 setState을 한 state의 값을 기억해서 계속계속 받아와줍니다. 그래서 결국 state가 잘 추가가 되는 것입니다.

![image](https://user-images.githubusercontent.com/93597794/163715094-c4def32f-cc2c-40f4-a93b-c7da4f20640d.png)

## 더 쉽게 정리해보자!

setNum(num + 1) 을 3번 호출했기 때문에 버튼 한번으로 3씩 증가할것으로 예상되지만, 여전히 1씩 증가하게 된다.

**왜 그럴까?**

```javascript
function App() {
  const [num, setNum] = useState(1);

  function plus() {
    setNum(num + 1);
    setNum(num + 1);
    setNum(num + 1);
  }

  function minus() {
    setNum(num - 1);
  }

  return (
    <div className="App">
      <h1>{num}</h1>
      <button onClick={plus}>PLUS</button>
      <button onClick={minus}>MINUS</button>
    </div>
  );
}
```

**동일한 state를 연속적으로 업데이트 하는 경우**, 리액트는 setState를 각각 동기로 수행하지 않고, **batch 처리**합니다.
**전달된 setState를 하나로 병합한 후 최종적으로 한번의 setState를 하게 된다는 얘기입니다.**

### 해결

결국 문제의 해결은 setState를 비동기로 수행할 때, **값을 전달하지말고 업데이트된 최신의 state와 함께 함수를 전달하는 방법**입니다.
즉, setState(state변경, callback) 형식으로 변경!

```javascript
function App() {
  const [num, setNum] = useState(1);

  function plus() {
    setNum((num) => num + 1);
    setNum((num) => num + 1);
    setNum((num) => num + 1);
  }

  function minus() {
    setNum(num - 1);
  }

  return (
    <div className="App">
      <h1>{num}</h1>
      <button onClick={plus}>PLUS</button>
      <button onClick={minus}>MINUS</button>
    </div>
  );
}
```

# 결론

1. setState()는 **비동기**로 처리된다.
2. setState()를 연속적으로 호출하면 **Batch 처리**를 한다.
3. **콜백함수로 해결**하면 된다!
