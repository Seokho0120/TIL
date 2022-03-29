## 호이스팅 (Hoisting)

끌어올리다!!
<br> 자바스크립트 엔진(번역기, Interpreter)이 **코드를 실행하기 전**, **변수와 함수, 클래스의 선언문을 끌어올리는 것**을 말한다.
<br> 변수의 선언과 초기화를 분리한 후(선언되었다는 사실만), **선언만 코드의 최상단**으로 옮긴다. **선언만!**

**변수**: 자료를 저장할 수 있는 **이름이 주어진 기억장소**

let 재할당이 필수로 필요한 경우
const 가능한 const를 사용!

- **함수**의 호이스팅은 함수의 선언문 전에 호출이 가능하게 해줌
- 함수의 선언문은 선언 이전에도 호출이 가능함

```javascript
print(); // 함수 호출 가능 -> 자바스크립트 엔진이 코드를 실행하기 전에 함수의 선언을 제일 위로 끌어올려주기 때문에

function print() {
  console.log("hollo");
}

print(); // 함수 호출 가능 -> 자바스크립트 엔진이 코드를 실행하기 전에 함수의 선언을 제일 위로 끌어올려주기 때문에
```

- **변수**(let, const)와 **클래스**는 **선언**만 호이스팅 되고(변수 이름이 있다는 사실만 호이스팅), 초기화는 안됨, 값 자체는 코드의 순서가 왔을떄 실행됨
- 초기화 전, 변수에 접근하면 컴파일(빌드) 에러가 발생

```javascript
console.log(hi);
let hi = "hi";

let func1 = function () {}; // 표현식 위에서 접근x

const cat = new Cat(); // 에러 발생
class Cat {}
```

```javascript
let x = 1;
{
  console.log(x);
  let x = 2; // 에러 발생
}
```

## var

일반 코딩 방식과 어긋나서 개발하면서도 멘붕이 옴
코드의 가독성과 유지보수성에 좋지 않음

- 변수 선언하는 키워드 없이 선언 & 할당이 가능함
- 선언인지, 재할당인지 구분하기 어려움

```javascript
something = "악";
console.log(something);
```

- **중복 선언**이 가능

```javascript
var poo  '악!'
var poo  '음?'
console.log(poo);
```

- **블록 레벨 스코프** 안됨 X

```javascript
var apple = "사과";
{
  var apple = "배";
  {
    var apple = "오렌지";
  }
}
console.log(apple); // 오렌지
```

- **함수 레벨 스코프**만 지원 됨

```javascript
function example() {
  var dog = "개";
}
console.log(dog);
```

## 엄격모드

strick mode

- 리액트와 같은 프레임워크 사용 시 기본적으로 엄격 모드
- 하지만 제약이 분명 생김(예전의 자바스크립트의 조용히 무시되던 에러들을 던져준다.)

```javascript
"use struck";
var x = 1;
delete x; // 엄격모드 사용하면 키워듯 삭제 불가능
```

```javascript
function add() {
  var a = 2;
  b = a + x;
  console.log(this); // 에러
}
add(1);
```

```javascript

```

```javascript

```

```javascript

```
