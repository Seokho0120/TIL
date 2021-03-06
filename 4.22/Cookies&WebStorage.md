# 쿠키와 웹 스토리지

기본적으로 쿠키와 웹 스토리지는 모두 브라우저에서 데이터 저장소의 역할을 하는 것들입니다. 웹에서 로그인을 하기 위해서는 토큰을 발급받아 API를 호출해야하 합니다.
<br>

하지만 반복되는 작업을 계속 하게 되는것이 비효율적이고, 이를 보완하기 위해 쿠키를 서버와 클라이언트에 생성해서 토큰 발급 없이 쿠키만 가지고 서버에 요청을 할 수 있게 됩니다

## 쿠키(Cookies)

- 쿠키는 서버와 로컬에 정보를 저장합니다.
- 사이트에서 방문한 페이지를 저장하거나 유저의 로그인 정보를 저장 등 다양한 방법으로 사용됩니다.
- 많은 보안 웹사이트들은 로그인을 한 후 쿠키를 사용해 유저의 신원을 확인하여 모든 페이지에서 재인증을 거치지지 않아도 되며 사이트에서 제한된 인터넷 사용 기록을 기반으로 사용자 경험을 개선합니다.
- **단점**은 저장 공간이 4KB로 작은편이며 사용자 정보 도난의 위험이 있는 편입니다. 그리고 매 HTTP 요청마다 포함되어 API 호출로 서버에 부담이 됩니다.

### Session cookies (세션 쿠키)

- 만료일을 포함하지 않습니다.
- 대신에 브라우저나 탭이 열려있는 동안에만 저장됩니다.
- 세션 쿠키는 메모리에 저장되며 디스크에 기록되지 않습니다.

### Persistent cookies (영구적 쿠키)

- 만료일을 가집니다.
- 만료일까지 유저의 디스크에 저장되고 만료일이 지나면 삭제됩니다.

## 웹 스토리지 (WebStorage)

- 쿠키의 단점을 보완하여 등장한 것입니다.
- 웹 스토리지는 서버에 클라이언트 데이터를 저장하지 않습니다.
- 로컬에만 저장하며 Key와 Value 형태로 이루어져 있습니다.
- 쿠키와는 달리 모든 HTTP 요청에서 데이터를 주고받을 필요가 없습니다.
- 저장공간은 데스크탑 기준 5~10MB이며 쿠키보다 훨씬 큽니다.
- **단점**은 HTML5부터 지원하기 때문에 이전 브라우저에서는 지원이 되지 않습니다.

# 로컬 스토리지

- 데이터가 유저의 로컬 디스크에 저장되어 있으면
- 인터넷이 끊어져도 데이터가 삭제되거나 지워지지 않습니다 (영구적 보관)

# 세션 스토리지

- 로컬 스토리지와 다르게 세션이 종료되면 (웹 브라우저를 닫으면)
- 클라이언트에 대한 정보를 삭제합니다
- 그러므로 보안이 많이 필요한 사이트일수록 세션 스토리지를 사용하는 것이 좋습니다
