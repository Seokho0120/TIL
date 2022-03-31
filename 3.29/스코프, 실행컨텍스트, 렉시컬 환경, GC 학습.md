## 스코프(Scope)

범위, 영역
<br>변수를 참조할 수 있는(접근할 수 있는) 유효한 범위
<br>식별자가(변수, 함수, 클래스 이름) 유효한 범위

**선언된 위치에 따라 유효 범위가 결정됨!**

<br>

```javascript
{
블럭 안의 변수는 블럭 안에서만 유효
}
```

이름 충돌 방지, 메모리 절약

`변수는 최대한 필요한 곳에서 정의해야겠군!`

코드 블럭: {}, if() {}, for() {}, function() {} 등등

<br>

- 블럭 외부에서는 블럭 내부의 변수를 참조 X

```javascript
{
  const a = "a";
}
console.log(a);
```

<br>

- 함수 외부에서는 함수 내부의 변수를 참조 X

```javascript
function print() {
  const message = "Hello World";
  console.log(message);
}
console.log(message);
```

<br>

- 함수 외부에서는 함수의 매개변수를 참조 X

```javascript
function sum(a, b) {
  console.log(a, b);
}
console.log(a, b);
```

## 퀴즈를 통해 이해해보자!

- 밖에서는 참조 못함, 안에서는 참조 가능

```javascript
{
  const x = 1;
  {
    const y = 2;
    console.log(x); // 1
  }
  console.log(x); // 1
  console.log(y); // app crash
}
```

```javascript
const text = "global"; // 전역 변수, 전역 스코프 (글로벌 변수, 글로벌 스코프)
{
  const text = "inside block1"; // 지역 변수(로컬 변수), 지역 스코프(로컬 스코프)
  {
    console.log(text); // inside block1
  }
}
```

## GC = Garbage Collector(쓰레기 수집가)

**자바스크립트 엔진 백그라운드 프로세스**
<br> 누구도 object를 참조하고 있지 않으면 청소해준다.
<br> 글로벌 변수는 앱이 종료될때까지 계속 메모리에 유지됨!

```javascript
const global = 1;
```

<br>

- 블록 내부에서만 존재하고, 블럭이 끝나면 자동으로 소멸됨(GC가 청소함)

```javascript
{
  const local = 1;
}
```

<br>

- 함수 내부에서도 블럭안에서 필요한 경우에는 필요한 곳에서! 블럭 안에서 변수를 선언하고 사용해야함!

```javascript
function print() {
  if (true) {
    let temp = 0;
  }
}
```

## 실행 컨텍스트(Execution context)

실행 컨텍스트는 **실행 가능한 코드**가 실행되기 위한 **필요한 환경**
<br> 코드의 실행 순서오 스코프를 기억!

<br>

실행 가능한 코드
<br> 일반적으로 실행 가능한 코드는 전역 코드와 함수 내 코드이다.

- 전역 코드 : 전역 영역에 존재하는 코드
- 함수 코드 : 함수 내에 존재하는 코드
- Eval 코드 : eval 함수로 실행되는 코드

<br>

필요한 여러가지 정보 및 환경
<br> 자바스크립틍 엔진은 코드를 실행하기 위해 필요한 여러가지 정보를 알아야함. 실행에 필요한 여러가지 정보란 아래와 같다

- 변수 : 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
- 함수 선언
- 변수의 유효범위(Scope)
- this

### 실행 컨텍스트 스택(Execution context Stack)

이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 자바스크립트 엔진은 **실행 컨텍스트를 물리적으로 객체의 형태로 관리**한다.
<br> 실행 컨텍스트 스택(Execution context Stack) : 실행할 코드에 제공할 환경 정보를 모아놓은 객체

### 렉시컬 환경(Lexical Environment)

각각의 블록은 렉시컬 환경이라는 내부 오브젝트를 가지고 있다.

- 환경 레코드(Environment Record)
  - 현재 블록의 정보를 담고 있음
- 외부 환경 참조(Outer Lexical, Environment Reference)
  - 부모를 참조 하고 있음

<img width="1432" alt="스크린샷 2022-03-29 오후 4 10 28" src="https://user-images.githubusercontent.com/93597794/160556999-f2cfeb78-28b0-4aad-86cb-36ac36ea413f.png">

**스코프 체인**
<br> 스코프가 연결되어 있는 것
<br> 각각의 스코프는 내 부모가 누구인지 가리키고 있음

- 현재의 스코프안에 내용이 없으면 바로 상위에 있는 스코프체인을 통해서 부모가 누구인지 확인 후, 부모에 해당하는 내용이 있는지 확인함

```javascript
메모리 절약뿐만 아니라, 성능을 위해서라도 변수는 최대한 필요한 곳에서 정의해야겠군!
```

<img width="1415" alt="스크린샷 2022-03-29 오후 4 16 15" src="https://user-images.githubusercontent.com/93597794/160557014-ee26e941-3e0a-4525-a598-7cff160490bd.png">

## 정리

**스코프**란 식별자가 유효한 범위를 나타내는데, 스코프 밖에서는 내부에 접근할 수 없지만 내부에서는 외부에 있는 그 어떤 부모의 데이터에 접근 가능하다.
<br> 접근할 수 있는 이유는 각각의 스코프는 **렉시컬 환경**이라는 곳이 있는데, 그 안에 외부 **렉시컬 환경 참조를 통해서(스코프 체인을 통해서)** 찾아가면서 부모에 접근할 수 있기 때문이다.
