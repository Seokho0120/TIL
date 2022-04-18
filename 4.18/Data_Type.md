# 컴퓨터의 가장 중요한 3가지 구성요소

- 하드디스크(파일, 어플리케이션을 저장)
- CPU(저장 장치에서 가져와서 연산)
- 메모리(데이터를 임시적으로 메모리에 올려놓음)

## 메모리(Memory)

메모리셀이라고 불리는 각각의 칸 혹은 저장장치들로 이루어진 것

![스크린샷 2022-04-18 오후 8 40 33](https://user-images.githubusercontent.com/93597794/163803216-b9ca9080-4fab-4693-b83f-a75911ab7764.png)

### 어플리케이션이 메모리에 올라왔을때

![스크린샷 2022-04-18 오후 8 40 05](https://user-images.githubusercontent.com/93597794/163803239-33164b43-b192-46dc-b1b7-f79b04bb7754.png)

# 데이터 타입 (Data Type)

![스크린샷 2022-04-18 오후 8 30 40](https://user-images.githubusercontent.com/93597794/163802261-452214b2-0bc0-41ec-baf9-dc51aa146a0e.png)

## 원시(primitive)

**단일 데이터**

- number
- string
- boolean
- null
- undefined
- symbol

## 객체(object)

**복합 데이터**

```javascript
{
  key: value; // value에는 원시, 객체 담을 수 있음
}

{
  id: 1234, // key: value
  key: 'secret-key', // key: value
}
```

![스크린샷 2022-04-18 오후 9 10 37](https://user-images.githubusercontent.com/93597794/163806429-f580c664-b139-4bd9-a812-b08a6a160ecb.png)

- **원시타입**은 어디에 변수가 선언되냐에 따라서 **Data, Stack**에 저장됩니다. (전역 선언 -> Data / 로컬 선언 -> Stack)

- **객체타입**는 key와 value형태로 여러개가 묶일 수 있개 때문에 **사이즈가 정해져 있지 않습니다.** 즉, 메모리셀 안에 한번에 들어갈 수 없기 때문에 **Heap**에 저장됩니다.

![스크린샷 2022-04-18 오후 9 16 43](https://user-images.githubusercontent.com/93597794/163807031-029a6b0a-e7cd-4cee-ad8f-ef9849d052d1.png)

# 깊은 복사 or 얕은 복사

![스크린샷 2022-04-18 오후 8 59 55](https://user-images.githubusercontent.com/93597794/163805227-38fda1ee-b50e-4a6c-9ffe-b88a91c3c184.png)

## 깊은 복사

원시 타입은 **값**이 복사되어 전달됩니다.

```javascript
let a = 1;
let b = a; // 1
b = 2;

console.log(a); // 1
console.log(b); // 2
```

## 얕은 복사

객체 타입은 **참조값(메모리주소, 레퍼런스)** 이 복사되어 전달됩니다.

```javascript
let apple = {
  // 메모리 주소 0x1234
  name: "사과",
};
let orange = apple; // 메모리 주소 0x1234

apple.name = "오렌지";
console.log(apple); // 오렌지
console.log(orange); // 오렌지

// 한곳에서 수정되어도 동일한 오브젝트를 바라보고 있기 때문에 오브젝트 자체가 수정됨
```
