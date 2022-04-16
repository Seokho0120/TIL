# useMemo & useCallback

이제 useState와 useEffect에 완전히 익숙해졌다고 느꼈는데, 컴포넌트 내에서 저 두 개의 hook 만으로도 props나 state를 다루는 로직에 관련된 기본적인 기능을 모두 구현할 수 있기 때문에 굳이 useMemo나 useCallback 을 사용할 필요를 느끼지 못했습니다. 물론 useMemo와 useCallback 은 리액트의 렌더링 성능 최적화를 위한 hook 이기 때문에 필수적으로 사용할 필요는 없습니다.

그럼에도 불구하고 사용해 볼 필요를 못 느낀다는 것이 그 기술의 장단점을 파악해서 결정한 것이 아니라 “단지 익숙하지 않은 기술을 배우지 않으려고 하는 나의 좁은 마음 때문” 이라는 생각이 스쳐지나갔습니다.

## 메모이제이션(memoization)이란?

알고리즘 시간에 자주 나오는 **메모이제이션(memoization)** 개념에 대해서 알아보자!

memoization이란 **기존에 수행한 연산의 결과값을 어딘가에 저장해두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법**을 말합니다. memoization을 절적히 적용하면 **중복 연산을 피할 수 있기 때문에** 메모리를 조금 더 쓰더라도 **애플리케이션의 성능을 최적화**할 수 있습니다.

## useMemo와 useCallback을 배우기 전에 알아야 하는 것

- 함수형 컴포넌트는 그냥 함수입니다. 함수형 컴포넌트는 단지 jsx를 반환하는 함수입니다.
- 컴포넌트가 렌더링 된다는 것은 누군가가 그 함수(컴포넌트)를 호출하여서 실행되는 것을 말합니다. 함수가 실행될 때마다 내부에 선언되어 있던 표현식(변수, 또다른 함수 등)도 매번 다시 선언되어 사용됩니다.
- 컴포넌트는 자신의 state가 변경되거나, 부모에게서 받는 props가 변경되었을 때마다 리렌더링 됩니다. (심지어 하위 컴포넌트에 최적화 설정을 해주지 않으면 부모에게서 받는 props가 변경되지 않았더라도 리렌더링 되는게 기본입니다.)

# useMemo

메모이제이션된 **'값'** 을 반환합니다.

## 용법

```javascript
useMemo(() => fn, deps);
```

- useMemo는 **deps**가 변한다면, **() => fn 이라는 함수를 실행**하고 그 값을 반환합니다. deps는 dependency이며, useMemo가 이 deps라는 것에 **'의존'**한다는 뜻.

## 예시

```javascript
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  // useMemo 사용하기
  useMemo(() => {
    console.log(ex);
  }, [ex]);

  // 두 개의 버튼을 설정했습니다. X버튼만이 ex를 변화시킵니다.
  return (
    <>
      <button onClick={() => setEx((curr) => curr + 1)}> X </button>
      <button onClick={() => setWhy((curr2) => curr2 + 1)}> Y </button>
    </>
  );
}
```

먼저 위 코드는 X라는 버튼을 누를 때에 **'ex'** 라는 상태값이 변화하는 코드입니다.

```javascript
useMemo(() => {
  console.log(ex);
}, [ex]);
```

deps 는 `[ex]` 입니다.
**'ex' 가 변할 때에만 () => {console.log(ex)} 이 실행됩니다.**

따라서 **X 버튼을 누를 때에만 콘솔창에 ex 값이 출력됩니다.**
Y 버튼을 누르더라도 APP 이라는 함수 컴포넌트가 전부 재실행 되지만, ex 라는 값은 변하지 않았기 때문에 useMemo는 아무런 변화가 없습니다.

# useCallback

메모이제이션된 **'함수'** 을 반환합니다.

## 용법

```javascript
useCallback(fn, deps);
```

- useCallback 는 **deps**가 변한다면, **fn** 이라는 **새로운 함수**를 반환합니다.

## 예시

```javascript
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [ex, setEx] = useState(0);
  const [why, setWhy] = useState(0);

  // useCallback 이 () => {console.log(why)} 라는 함수를 반환합니다.
  const useCallbackReturn = useCallback(() => {
    console.log(why);
  }, [ex]);

  // useCallback 이 담겨있는 함수를 실행합니다.
  useCallbackReturn();

  return (
    <>
      <button onClick={() => setEx((curr) => curr + 1)}>X</button>
      <button onClick={() => setWhy((curr2) => curr2 + 1)}>Y</button>
    </>
  );
}
```

useMemo 코드와 같이 버튼을 보여주는 코드입니다.
위의 **useCallback** 은 **() => {console.log(why)} 라는 함수를 반환**해주고 있습니다.

위의 useCallback 은 다음의 순서로 진행됩니다.

1. 처음 컴포넌트가 시작될 때 실행 **() => {console.log(0)}**
2. ex 가 변할 때까지 함수는 **() => {console.log(0)}**
3. **ex 가 변한다면** 그제서야 why 의 값을 가져와서 **() => {console.log(새로운 값)}**

예를 들어서 **Y 버튼을 다섯번** 누른다고 해보면,
Y 버튼이 눌리면 'why'라는 상태의 값이 1씩 증가할 것입니다.
![img](https://user-images.githubusercontent.com/93597794/163574435-a09b35ce-ce09-4469-8f69-bf2b8ac2f5ac.gif)

위의 움짤에서 보듯이, Y를 다섯번 누를 때 동안에는 계속 함수가 **() => {console.log(0)}** 입니다. (물론 이 때 why라는 상태값은 계속 값이 증가합니다.)

그러다가 **X 버튼**을 누르면서 ex 라는 변수(deps) 가 변하자 마자, **() => {console.log(5)}** 를 반환합니다.
**deps가 변해야 함수 컴포넌트와 상태값(why) 를 공유하는 것입니다!!**

따라서 **useCallback은 함수와는 상관 없는 상태값이 변할 때, 함수 컴포넌트에서 불필요하게 함수를 업데이트 하는 것을 방지**해줍니다.

- 다만, deps 를 잘못 설정하면 아무리 함수 컴포넌트를 재실행해도 함수가 변하지 않으면서 내가 원치 않는 상황이 올 수도 있으므로 섬세한 컨트롤이 필요합니다..

## useCallback 의 새로고침

useCallback 은 deps 가 변하면서 함수를 반환할 때, **형태가 같더라도 아예 새로운 함수를 반환합니다.**

```javascript
const add1 = () => {};
const add2 = () => {};
```

위의 두 함수는 같지 않습니다.
![img](https://user-images.githubusercontent.com/93597794/163575029-e2dda4c1-e363-4f27-ac21-9c02720587c6.png)

각 변수는 같은 함수를 바라볼 뿐 전혀 다른 변수입니다. 바라보는 값만 같을 뿐, 전혀 다른 메모리를 가진 변수입니다.

### useCallback은 새로운 함수를 반환합니다.

```javascript
const useCallbackReturn = useCallback(() => {}, [ex]);
```

여기서 ex 가 변할 때, useCallback 은 새로운 함수를 반환합니다.
그 말은 즉, **ex == 0 일 때와 ex == 1 일 떄의 () => {} 은 다른 함수**입니다.
새로운 무기명 함수를 반환했기 때문입니다. 이는 앞서 기술했듯이, 값이 같을 뿐 다른 메모리입니다.

# 두 가지의 차이점

**useMemo**는 **복잡한 함수의 return값을 기억해야 할 때** 사용합니다.

**useCallback**은 **함수 자체를 기억해야 할 때** 사용합니다.
