# Routing

다른 경로(url 주소)에 따라 다른 View(화면)을 보여주는 것

## SPA(Single Page Application)

- 사용자가 다른 뷰로 이동할 때 애플리케이션은 **뷰를 동적으로 다시 그림**.
- SPA는 MPA(Multi Page Application) 대비 페이지 간 이동 시 사용자가 느낄 수 있는 딜레이를 제거해 일반적으로 더 나은 UX를 제공. **(페이지 전체를 새로고침 하지 않기 때문!)**

React는 기본적으로 라우팅 시스템을 갖추고 있지 않으므로(라이브러리!), `react-router-dom` 과 같은 부가적인 라이브러리를 설치해서 라우팅 기능을 추가할 수 있습니다.

```jsx
// index.html
<!DOCTYPE html>
  <head>
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

```jsx
// index.js
ReactDOM.render(<Router />, document.getElementById("root"));
```

```jsx
// Router.js
const Router = () => {
  return (
    <BrowserRouter>
      <Nav />
      <Routes>
        <Route path="/" element={<App />} />
        <Route path="/users" element={<Users />} />
        <Route path="/products" element={<Products />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
      <Footer />
    </BrowserRouter>
  );
};
```

- `index.html` : public/index.html에 위치하며 React 페이지 로드 시 가장 먼저 호출되는 영역
- `index.js` : React 앱을 렌더하고 index.html의 `div#root` 이하에 끼워넣는 역할
- `Router.js` : React 앱이 경로에 따라 어떤 컴포넌트를 보여줄지 결정하는 역할 (화면 바꿔 끼우기)

# 정적 라우팅 & 동적 라우팅

지금까지 해온 라우팅 방법으로는 완전히 정해진 경우(정적, static)에 대해서만 경로를 표현 할 수 있었습니다.

```jsx
"/"         => <App />
"/users"    => <Users />
"/products" => <Products />
```

- 하지만 다음과 같은 경우라면?

<img width="812" alt="Screen_Shot_2021-05-04_at_6 10 40_PM" src="https://user-images.githubusercontent.com/93597794/165733054-93534182-17ec-458e-9816-292562c95fe0.png">

<img width="761" alt="Screen_Shot_2021-05-04_at_6 14 43_PM" src="https://user-images.githubusercontent.com/93597794/165733040-57ead85b-bba7-48d3-833a-4f9629a303e6.png">

- url 을 살펴보면 url 마지막에 특정 id 값이 들어가고(`/32692`, `/53424`), 해당 id 값에 따라 서로 다른 상세 페이지 정보가 화면에 그려지는 것을 볼 수 있습니다. **id 값에 따라 무수히 많은 url 이 나타날 것이고, 각각의 모든 url 에 대해 미리 경로의 형태와 갯수를 결정할 수 없게 됩니다.**
- 즉, URL에 들어갈 **id를 변수처럼 다뤄야 할 필요성**이 생긴 것입니다.
- 이처럼 정적이지 않은, **동적일 수 있는 경로에 대하여 라우팅**을 하는 것을 **동적 라우팅(Dynamic Routing)**이라고 부릅니다.
- 이는 다음 두 가지 개념(`Path Parameter`, `Query Parameter`)을 통해 적용해볼 수 있습니다.

## 동적 라우팅

동적인 경로를 처리할 수 있는 방법으로 Path Parameter 와 Query Parameter 이 있습니다.

### Path Parameter

```jsx
// Bad
"/users/1" => <Users id={1} />
"/users/2" => <Users id={2} />
"/users/3" => <Users id={3} />
"/users/4" => <Users id={4} />
"/users/5" => <Users id={5} />
```

```jsx
// Good
"/users/:id" => <Users /> // useParams().id
```

### Query Parameter

```jsx
// Bad
"/search?keyword=위코드"    : <Search keyword="위코드" />
"/search?keyword=리액트"    : <Search keyword="리액트" />
"/search?keyword=라우팅"    : <Search keyword="라우팅" />
"/search?keyword=쿼리스트링" : <Search keyword="쿼리스트링" />
"/search?keyword=SPA"     : <Search keyword="SPA" />
```

```jsx
// Good
"/search?keyword=something" : <Search /> // useLocation().search
```
