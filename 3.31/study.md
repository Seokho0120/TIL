## 클로저(Closure)

함수와 그 함수를 감싸고 있는 렉시컬환경에 대한 조합
<br> 내부함수가 외부함수의 스코프에 접근할 수 있는 것을 가르킨다.

### 이전에 배웠던 내용 vs 클로저

- 이전(inner 함수가 내부에서 호출)

![스크린샷 2022-03-31 오후 5 29 01](https://user-images.githubusercontent.com/93597794/161012318-357c9bca-5c31-49bc-abbf-db2b7024a040.png)

- 클로저(inner 함수를 선언하고, return 해줌)

![스크린샷 2022-03-31 오후 5 28 37](https://user-images.githubusercontent.com/93597794/161012538-73e781f2-e0bc-4702-b9fa-41e6009ea54a.png)

<br>

- 클로저는 반환된 내부 함수가 자신이 선언됐을때의 렉시컬 환경인 스코프를 기억해서 자신이 선언됐을때의 환경(스코프)밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수이다.

```javascript
function outer() {
  function inner() {
    let title = "hello";
    alert(title);
  }
  inner();
}
outer();

// outer : 외부함수
// inner : 내부함수
```

- 내부함수에서 외부함수의 지역변수에 접근할 수 있다.

```javascript
function outer() {
  let title = "hello"; // 외부함수에 선언되어있는 지역변수
  function inner() {
    alert(title); // 내부함수에서 외부함수의 지역변수에 접근할 수 있다.
  }
  inner();
}
outer();
```

- 외부함수가 더이상 사용되지 않아도, 내부함수가 외부함수에 접근할 수 있다.
- 함수는 return을 하면 종료되었다고 한다. outer는 내부함수를 return하고 종료되었지만, outer에서 선언된 title변수에 접근하여 사용할 수 있다.

```javascript
function outer() {
  let title = "hello";
  return function () {
    alert(title);
  };
}
let inner = outer();
inner();
```

### 언제 사용하나?

내부 정보를 은닉하고, 공개 함수(public, 외부)를 통한 데이터 조작을 위해 사용한다.
<br> 캡슐화와 정보은닉
<br> 클래스 private 필드 또는 매소드를 사용하는 효과와 동일!

```javascript
function makeCounter() {
  let count = 0; // 숨기고 싶은 정보
  function increase() {
    count++;
    console.log(count);
  }
  return increase;
}

const increase = makeCounter();
increase();
increase();
increase();
```
