## CRA(Create React App)

리액트 프로젝트를 시작하는데 필요한 개발 환경을 세팅해주는 도구(toolchain)

- 리액트는 UI기능만 제공. 따라서 개발자가 직접 구축해야하는 것들이 많음.
- 처음 시작하는 단계에서는 직접 개발환경을 구축하기 어려움.
- CRA는 리액트로 웹 어플리케이션을 만들기 위한 환경을 제공함.
- CRA를 이용하면 하나의 명령어로 리액트 개발환경 구축이 가능함.

```javascript
npx create-react-app 프로젝트 이름
npm start //실행
```

### Node_Modules

npm으로 다운로드 받은 모든 패키지들이 저장되어 있는 폴더 <br>
용량이 많기 때문에 **gitignore**에서 관리합니다. 그렇게 되면 다른 팀원들이 clone 받았을때 Node_Modules의 내용이 없는 상태로 clone됩니다. 그렇기 때문에 **clone받은 후 npm install 명령어를 필수로 실행해서 파일들을 다운로드 받아야 합니다.** npm install 명령어를 실행하면 package.json의 dependency 항목을 자동으로 읽어서 다운로드가 됩니다.

### package.json

우리 프로젝트에 대한 정보들이 기입된 파일

- script: 프로젝트에서 실행할 수 있는 명령어들이 있는 곳
- dependency: 프로젝트에서 필요로하는 다른 패키지들에 대한 정보가 있는 곳

### .gitignore

git으로 관리하지 않을 파일 또는 폴더 등을 기입해두는 파일. 그렇기 때문에 git에 업데이트가 되지 않습니다.

### index.html

- html의 엔트리 포인트
- 사용자가 우리 프로젝트를 요청하면 최초로 보여지는 html

### index.js

- javascript의 엔트리 포인트
- html과 react 컴포넌트를 연결해주는 역할을 함, 중간다리

### app.js

- 실제 화면에 보여지고 있는 컴포넌트
