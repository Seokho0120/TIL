# setTimeout & setInterval 함수

**자바스크립트의 타이머를 사용하는 내장 함수 2가지 setTimeout(), setInterval() 함수**에 대해서 학습했습니다. <br>
이후 React Hooks에서 setInterval() 함수의 오작동에 대해서 정리했습니다.

## setTimeout

코드를 바로 실행하지 않고 **일정 시간 기다린 후 실행**해야하는 경우, 자바스크립트의 setTimeout() 함수를 사용하면 됩니다. <br>
setTimeout() 함수는 **첫번째 인자로 실행할 코드를 담고 있는 함수**를 받고, **두번째 인자로 지연 시간을 밀리초(ms) 단위**로 받습니다.

- 간단한 예로, 2초를 기다린 후에 어떤 문자열을 콘솔에 출력해보았습니다.

```javascript
setTimeout(() => console.log("2초 후에 실행됨"), 2000);

2초 후에 실행됨
```

setTimeout() 함수는 **세번째 인자부터 가변인자**를 받습니다. 첫번째 인자로 넘어온 함수가 인자를 받는 경우, 이 함수에 넘길 인자를 명시하기 위해서 사용합니다.

- 예를 들어, 두 개의 수를 인자로 받아 더한 값을 출력해주는 add()라는 함수에 인자로 3과 4를 넘겨 2초를 기다린 후 호출해겠습니다.

```javascript
function add(x, y) {
  console.log(x + y);
}

setTimeout(add, 2000, 3, 4);

7;
```

**setTimeout() 함수는 타임아웃 아이디(Timeout ID)라고 불리는 숫자를 반환**합니다. <br>
타임아웃 아이디는 setTimeout() 함수를 호출할 때 마다 내부적으로 생성되는 타이머 객체를 가리키고 있습니다. <br>

- **이 값을 인자로 clearTimeout() 함수를 호출하면 기다렸다가 실행될 코드를 취소**할 수 있습니다.

```javascript
const timeoutId = setTimeout(() => console.log("5초 후에 실행됨"), 5000);
clearTimeout(timeoutId);

// 아무 것도 출력 안 됨
```

## setInterval

웹 페이지에서 특정 부분을 주기적으로 업데이트 하거나, 어떤 API로 부터 변경된 데이터를 주기적으로 받아와야 하는 경우가 있습니다. <br>
이럴 때는 자바스크립트의 setInterval() 함수를 사용하면 됩니다.
<br>

setInterval() 함수는 **어떤 코드를 일정한 시간 간격을 두고 반복해서 실행하고 싶을 때 사용**합니다. 함수 API는 setInterval() 함수와 대동소이 합니다. **첫번째 인자로 실행할 코드를 담고 있는 함수**를 받고, **두번째 인자로 반복 주기를 밀리초(ms) 단위**로 받습니다.

- 간단한 예로, setInterval() 함수를 사용하여 콘솔에 현재 시간을 2초마다 출력해보았습니다.

```javascript
setInterval(() => console.log(new Date()), 2000);

Sun Dec 12 2021 12:29:06 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:08 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:10 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:12 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:14 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:16 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:18 GMT-0500 (Eastern Standard Time)
```

- 다른 예로, 0과 9 사이의 수를 무작위로 생성하여 2초마다 출력해보겠습니다.

```javascript
setInterval(() => console.log(Math.floor(Math.random() * 10)), 2000);

3;
2;
8;
3;
1;
9;
```

**setInterval() 함수는 인터벌 아이디(Interval ID)라고 불리는 숫자를 반환**합니다. <br>
인터벌 아이디는 setInterval() 함수를 호출할 때 마다 내부적으로 생성되는 타이머 객체를 가리키고 있습니다. <br>
**이 값을 인자로 clearInterval() 함수를 호출하면 코드가 주기적으로 실행되는 것을 중단**시킬 수 있습니다.

## 정리

setTimeout(), setInterval() 함수는 **자바스크립트의 타이머 기능을 하는 내장 함수**입니다. <br>
setTimeout(), setInterval() 함수를 사용한 후에는 반드시 **clearTimeout() 함수와 clearInterval() 함수를 사용해서 타이머를 청소해주는 습관**을 들여야합니다. 특히, SPA(Single Page Application)을 개발할 때는 이 부분이 **메모리 누수(Memory leak)로 이어질 수 있기 때문에 주의**해야 합니다.

# React Hooks에서 setInterval() 사용 문제

Hooks에서 setInterval() 함수가 제대로 작동하지 않는 경험을 할 수 있습니다. setInterval() 함수를 사용했을 때 **리액트 내부에서 일어나는 현상**과 그 현상이 발생했을때 **어떻게 대처하는 것이 좋은지**에 대해 예시를 통해 학습했습니다. 결론적으로 **React와 setInterval() 함수는 어울리지 않습니다.**

- 간단한 예로, setInterval 을 이용해서 1초마다 글씨 크기 키워봤습니다.

```javascript
export default function TestAll() {
  const [fontState, setFontState] = useState(5);

  setInterval(() => {
    setFontState(fontState + 1);
  }, 1000);

  return (
    <div>
      <div style={{ fontSize: fontState + "px" }}> 하이루 </div>
    </div>
  );
}
```

아래는 결과 영상입니다.. 자바스크립트에서는 위와 같은 코드를 사용했을지 몰라도, **React에서는 state가 변하면 App이 리렌더링이 되기 때문에 setInterval() 함수는 무한히 실행**되고 맙니다. (정신차려....)

![ezgif com-gif-maker (10)](https://user-images.githubusercontent.com/93597794/163938645-96e7ba59-358a-4e00-9130-eb29b9a86b13.gif)

- useEffect 훅을 이용해서 무한의 렌더링 방지해보겠습니다.

```javascript
export default function TestAll() {
  const [fontState, setFontState] = useState(5);

  useEffect(() => {
    setInterval(() => {
      setFontState(fontState + 1);
    }, 1000);
  }, []);

  return (
    <div>
      <div style={{ fontSize: fontState + "px" }}> 하이루 </div>
    </div>
  );
}
```

아래는 결과 캡쳐입니다. 위와 같이 사용하면 **첫 렌더링때 한번만 적용이 되고 그 이후에는 렌더링이 되지 않습니다.** <br>
그래서 첫번째 예시처럼 무한히 렌더링 되는 것은 막을 수 있지만, fontState 는 첫 렌더링때의 값인 5가 유지됩니다. 그러므로 폰트 사이즈는 6px로 고정이 됩니다.

![스크린샷 2022-04-19 오후 3 22 47](https://user-images.githubusercontent.com/93597794/163939060-beac0aa0-901f-4f44-a117-0c0c7cd0a2da.png)

- 해결 방법 예시, **useRef 훅을 이용하면 위와 같은 현상을 방지**할 수 있습니다.

```javascript
import React, { useState, useRef } from "react";
import { useEffect } from "react";

export default function TestAll() {
  const [fontState, setFontState] = useState(5);
  const fontRef = useRef(5);

  useEffect(() => {
    setInterval(() => {
      setFontState((fontRef.current += 1));
    }, 1000);
  }, []);

  return (
    <div>
      <div style={{ fontSize: fontState + "px" }}> 하이루 </div>
    </div>
  );
}
```

**useRef를 이용해서 상태를 기억하고, useRef의 current 값을 이용해 계속 증가된 값을 적용시킬 수 있습니다.** <br>
딜레이가 위의 케이스처럼 1000으로 고정되도 되는 상황이라면 이렇게만 해도 문제는 없습니다. 그러나 만약 다른 페이지로 이동을 하거나 모드에 따라서 화면 렌더링을 변경해 주는 등의 작업을 해 줘야 할 때에는 상황이 조금 달라질 수도 있습니다.

![ezgif com-gif-maker (11)](https://user-images.githubusercontent.com/93597794/163940304-38045aa6-2eeb-4c60-90a3-9bc5540f23af.gif)
