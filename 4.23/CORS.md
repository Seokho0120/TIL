# CORS

CORS란, (= Cross Origin Resource Sharing ) <br>
서로 다른 도메인간에 자원을 공유하는 것을 의미하며 기본적으로 차단되어있습니다.

Origin이란 출처를 의미하며 Protocol + Host + Port 를 합친 것을 말합니다.
Origin이 같으면 CORS 에러는 발생하지 않습니다.

## CORS 에러 해결 방법

서버에서 응답 헤더에 특정 헤더를 포함하는 방식으로 해결할 수 있습니다.

- Access-Control-Allow-Origin: 특정 브라우저가 리소스에 접근이 가능하도록 허용합니다.

- Access-Control-Allow-Method: 특정 HTTP Method만 리소스에 접근이 가능하도록 허용합니다.

- Access-Control-Expose-Headers: 자바스크립트에서 헤더에 접근할 수 있도록 허용합니다.

- credentials: 쿠키 등의 인증 정보를 전달할 수 있습니다.

## 정리

CORS란 서로 다른 Origin간에 자원을 공유하는 것을 말하며 기본적으로 브라우저간에 차단되어있습니다.

Origin이란 출처를 말하며, Protocol + Host + Port 를 합친 것을 의미합니다.

CORS 에러는 서버에서 응답 헤더에 특정 헤더를 포함하는 방식으로 해결할 수 있습니다.

예를 들어 Access-Control-Allow-Origin을 통해 특정 브라우저가 리소스에 접근이 가능하도록 허용할 수 있습니다.
