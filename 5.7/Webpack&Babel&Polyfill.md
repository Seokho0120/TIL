# Webpack & Babel & Polyfill

## Babel

![image](https://user-images.githubusercontent.com/93597794/167130141-3f132da5-9088-4af0-9aae-5a011119fd3e.png)

**바벨(Babel)**은 자바스크립트 **컴파일러**로 새로운 방식의 자바스크립트로 개발 후, 배포할 때에는 예전 방식의 자바스크립트로 변환해서 **배포**하려고 사용합니다.

- 컴파일러 : 특정 프로그래밍 언어로 쓰여 있는 문서를 다른 프로그래밍 언어 혹은 컴퓨터 언어로 옮기는, 일종의 **번역 프로그램**

### Babel 사용 이유

최신 버젼의 자바스크립트가 실행이 안되는 **구버젼 웹브라우저**를 대응하기 위해서 입니다.

ES6 코드를 ES5 코드로 **변환**해주는 일에서 리액트의 JSX문법, 타입스크립트, 코드 압축, Proposal 까지 처리해줍니다.

## Webpack

**웹팩(Webpack)**은 모듈을 번들 시켜주는 역할을 합니다. **빌드** 라는 과정을 통해 하나의 파일로 만들어 주는데, 빌드란 소스코드 파일을 실행가능한 소프트웨어 산출물로 만드는 과정으로 컴파일, 배포 등의 과정이 있습니다.

바벨을 사용하려면 **Node.js**가 되어있어야 하고 터미널에서 웹팩 관련된 것들을 설치해줍니다.

- 모듈(module) : 분리된 파일
- 번들링(Bundling) : 모듈들의 의존성 관계를 파악하여 그룹화시켜주는 작업 (분리된 녀석들을 하나로 합쳐주는 구나)

설치방법 : `npm install webpack webpack-cli path --save-dev`

## Polyfill

**폴리필(Polyfill)**은 **최신 ECMAScript 환경**을 만들어 줍니다. 바벨은 ES6 => ES5로 변환할 수 있는 것들만 변환을 하는데, ES6에서 비동기 처리를 위해 등장한 Promise와 같이 ES5에서 변환할 수 있는 대상이 없는 경우 에러가 발생합니다. 이러한 경우 우리는 Polyfill 을 이용해서 이슈를 해결할 수 있습니다.

설치방법 : `npm install @babel/core @babel/polyfill @babel/preset-env @babel/preset-react babel-loader --save-dev`
