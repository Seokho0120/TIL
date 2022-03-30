## async & await

콜백의 복잡성을 해결하기 위한 프로미스를 보다 더 깔끔하게 작성할 수 있다!

- 비동기 코드를 동기적으로!
- async를 사용하면 함수 내부의 비동기적인 코드를 동기적으로 사용할 수 있다.
- 단, Promise를 리턴하는 함수를 호출할때는, await를 작성해서 기다렸다가 Promise값이 resolve가 되면, 그 값을 변수에 할당한 후 return 한다.

```javascript
// 바나나와 사과를 같이 가져오기

function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("🍌");
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("🍎");
    }, 3000);
  });
}

function getOrange() {
  return new Promise.reject(new Error("no Orange"));
}

// async를 사용하면 함수 내부의 비동기적인 코드를 동기적으로 사용할 수 있다.
// 단, Promise를 리턴하는 함수를 호출할때는, await를 작성해서 기다렸다가 Promise값이 resolve가 되면, 그 값을 변수에 할당한 후 return

async function fetchFruits() {
  const banana = await getBanana();
  const apple = await getApple();
  return [banana, apple];
}

fetchFruits().then((fruits) => console.log(fruits));

// 이전 코드 -> 콜백 지옥이 펼쳐질 수 있음 -> async, await로 정리 가능함.
function fetchFruits() {
  return getBanana() //
    .then((banana) =>
      getApple() //
        .then((apple) => [banana, apple])
    );
}
fetchFruits().then((fruits) => console.log(fruits));
```

```javascript
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 계란`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 후라이펜`);
}

function getChicken() {
  return Promise.reject(new Error("치킨을 가지고 올 수 없음ㅠㅠ"));
}

async function makeFriedEgg() {
  let chicken;
  try {
    chicken = await getChicken();
  } catch {
    chicken = "다른 치킨";
  }
  const egg = await fetchEgg(chicken);
  return fryEgg(egg);
}

// 이전 코드
function makeFriedEgg() {
  return getChicken()
    .catch(() => "다른 치킨")
    .then(fetchEgg)
    .then(fryEgg);
}

makeFriedEgg().then(console.log);
```
