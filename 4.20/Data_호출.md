# JSON

JSON = Javascript Object Notation <br>
서버와 클라이언트(브라우저, 모바일) 간의 HTTP 통신을 위한 오브젝트 형태의 텍스트 포멧

**stringify**

- object > JSON
- 객체(object)를 문자열(JSON형태)로 변환
- 직렬화 Serializing: 객체를 문자열로 변환

**parse**

- JSON > object
- 문자열(JSON형태)을 객체(object)로 변환
- 역직렬화 Deserializing: 문자열 데이터를 자바스크립트 객체로 변환

<br>

**객체를 JSON으로 만들어서 서버에 보내고, 서버로부터 받은 JSON이라는 문자열을 다시 객체로 변환하여 서버와 네트워크 통신을 합니다.**

```javascript
const lsh = {
  name: "seokho",
  age: 20,
  eat: () => {
    console.log("eat");
  },
};

// 직렬화 Serializing: 객체를 문자열로 변환
const json = JSON.stringify(lsh);
console.log(json);
console.log(lsh);

// 역직렬화 Deserializing: 문자열 데이터를 자바스크립트 객체로 변환
const obj = JSON.parse(json);
console.log(obj);
```

# fetch() 함수

백앤드로부터 데이터를 받아오려면 api를 호출하고 데이터를 응답받습니다. 이 때 자바스크립트 Web API **fetch() 함수**를 쓰거나 **axios 라이브러리**를 사용할 수 있습니다.

## 사용법

fetch() 함수는 **첫번째 인자로 URL**, **두번째 인자로 옵션 객체(HTTP 방식(method), HTTP 요청 헤더(headers), HTTP 요청 전문(body))**를 받고, **Promise 타입의 객체를 반환**합니다. <br>
반환된 객체는, API 호출이 성공했을 경우에는 응답(response) 객체를 resolve하고, 실패했을 경우에는 예외(error) 객체를 reject합니다.

```javascript
fetch(url, options)
  .then((response) => console.log("response:", response))
  .catch((error) => console.log("error:", error));
```

```javascript
fetch("api 주소", {})
  .then((res) => res.json())
  .then((res) => {
    // data를 응답 받은 후의 로직
  });
```

```javascript
fetch("api 주소", {})
  .then(function (res) {
    return res.json;
  })
  .then(function (res) {
    // data를 응답 받은 후의 로직
  });
```

### GET

fetch() 함수에서 **default method**는 **get**입니다. <br>
아래와 같은 api 명세를 보고 fetch() 함수를 작성했습니다.

```javascript
설명: 유저 정보를 가져온다.
base url: https://api.google.com
endpoint: /user/3
method: get
응답형태:
    {
        "success": boolean,
        "user": {
            "name": string,
            "batch": number
        }
    }
```

```javascript
fetch('https://api.google.com/user/3', { method: 'GET' })
  .then(res => res.json())
  .then(data => {
    if (data.success) {
        console.log(`${data.user.name}` 님 환영합니다);
    }
  });
```

api 주소를 보면 user 뒤에 있는 3이 user id 인것 같습니다. **고정된 api라면 그냥 자바스크립트 코드에서도 고정해서 사용**하면 되는데, 위와 같이 **api 주소를 상황에 맞게 유동적으로 바꿔줘야 할 때**가 정말 많습니다. 아래와 같이 작성할 수 있습니다.

```javascript
import React, { useEffect } from 'react';

function User(props) {
  useEffect(() => {
      const { userId } = props;
	    fetch(`https://api.google.com/user/${userId}`)
	      .then(res => res.json())
	      .then(data => {
	        if (data.success) {
	            console.log(`${data.user.name}` 님 환영합니다);
	        }
	    });
    }, []);
  ...
}
```

### POST

post는 fetch() 함수에 method 정보를 인자로 넘겨주어야 합니다. <br>
호출해야할 api가 get인지, post인지 모를때, 당연히 **api를 개발한 백앤드 개발자**에게 물어봐야 합니다. api 정보를 아는 것은 오로지 api를 만든 개발자 뿐입니다.

아래와 같은 api 명세를 보고 fetch() 함수를 작성했습니다.

```javascript
설명: 유저를 저장한다.
base url: https://api.google.com
endpoint: /user
method: post
요청 body:
    {
        "name": string,
        "batch": number
    }

응답 body:
    {
        "success": boolean
    }
```

```javascript
fetch("https://api.google.com/user", {
  method: "post",
  body: JSON.stringify({
    name: "seokho",
    batch: 1,
  }),
})
  .then((res) => res.json())
  .then((data) => {
    if (data.success) {
      alert("저장 완료!");
    }
  });
```

POST는 GET보다 복잡기 때문에 정리 해봅니다.

1. **두번째 인자**에 **method**와 **body**를 보내주어야 합니다.
2. method는 **post**
3. body는 **JSON형태**로 보내기 위해 **JSON,stringify() 함수에 객체를 인자로 전달**하여 JSON형태로 변환했습니다.

post로 데이터를 보낼 때 JSON.stringify를 해주어야 하는 반면, **axios**는 **객체를 그대로 작성해도 되는 편리한 점**이 있습니다. <br>
이렇듯 **axios**는 소소한 편의성을 제공해주고, 요청과 응답에 대한 확장성 있는 기능을 만들 수도 있습니다.

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```
