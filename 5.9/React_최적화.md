# 리액트 렌더링 최적화

면접 뿐 아니라 현업에서 리액트 최적화에 대한 이슈가 많다고 하기에, 학습을 진행했습니다.

## state 선언은 어디서 하는게 좋을까?

리액트는 특정 state가 변경되면 그 state가 선언된 컴포넌트와 그 하위 컴포넌트들을 모두 리렌더링 시킵니다. 따라서 state가 선언되는 위치를 잘 설계하는 것은 리렌더링 횟수에 엄청난 영향을 끼칩니다.

기본적으로 해당 state를 사용하는 컴포넌트들을 잘 구분해놓은 후, 그 컴포넌트들 중 **가장 최상위 컴포넌트에 선언** 합니다.

- 만약 그 state를 사용하는 최상위 컴포넌트보다 **더 상위 컴포넌트에 state를 선언하면**, state를 사용하지 않는 더 많은 컴포넌트들이 state변경에 의해 **불필요한 리렌더링**을 겪게 됩니다.

## 객체 타입의 state는 최대한 분할하여 선언합니다!

객체가 크고 복잡한 구조인 경우 분할할 수 있는 만큼 최대한 분할하는 것이 좋습니다.

해당 state에서 일부의 프로퍼티만 사용하는 하위 컴포넌트가 있다면, 그 컴포넌트는 해당 프로퍼티가 변경될 때에만 리렌더링 되는 것이 바람직합니다.

- 만약 복잡한 객체로 선언된 state를 분할하지 않으면, 하위 컴포넌트가 사용하지 않는 다른 프로퍼티의 값이 업데이트될 때에도 리렌더링이 발생하므로 렌더링 최적화의 대상이 됩니다.

```javascript
function Example3() {
  const [state, setState] = useState({
    group: {
      name: "coco",
      description: "rendering optimization pracitice",
    },
    users: [
      {
        id: 0,
        name: "Kim",
        age: 27,
      },
      {
        id: 1,
        name: "Jo",
        age: 25,
      },
    ],
  });

  return (
    <div>
      <Group group={state.group} />
      <UserList
        users={state.users}
        setUsers={(newUsers) => {
          setState({ ...state, users: newUsers });
        }}
      />
    </div>
  );
}
```

여기서 만약 users 배열에 **원소가 하나 추가**되면 어떻게 될까요?

**users데이터를 이용하는 UserList는 리렌더링**되어야 합니다. 그런데 굳이 users데이터를 이용하지 않는 Group 컴포넌트까지도 state변경으로 인해 리렌더링 될 수 있습니다.

이번엔 group state와 users state를 나눠서 선언해보겠습니다.

```javascript
function Example4() {
  const [group] = useState({
    name: "coco",
    description: "rendering optimization pracitice",
  });
  const [users, setUsers] = useState([
    {
      id: 0,
      name: "Kim",
      age: 27,
    },
    {
      id: 1,
      name: "Jo",
      age: 25,
    },
  ]);

  return (
    <div>
      <Group group={group} />
      <UserList users={users} setUsers={setUsers} />
    </div>
  );
}
```

이렇게 **나눈 후** 다시 users 배열에 **원소를 하나추가**하는 경우 어떻게 될까요?

이전과 마찬가지로 users state변화로 인해 index 컴포넌트가 리렌더링되고 이에 따라 하위 컴포넌트들이 리렌더링 되면서 Group컴포넌트까지도 리렌더링이 됩니다.

그러면 굳이 이렇게 state를 분할해야하는 이유는 무엇일까요?

그건 바로 이렇게 분할함으로써, 구조적으로 group state는 Group 컴포넌트에서만 사용하고, users state는 UserList 컴포넌트에서만 사용한다는 것이 명확하게 보이게 되고, 더 하위컴포넌트에 내려서 선언해야 할 필요성을 알게 되는 데에 의의가 있습니다. **우리는 state객체를 두개로 분할함으로써, 더 나은 설계를 할 수 있게 되었습니다.**

## React.Memo 를 이용한 컴포넌트 메모이제이션

React.memo는 컴포넌트를 래핑하여 props를 비교하여 리렌더링을 막을 수 있는 메모이제이션 기법을 제공하는 함수입니다.

React.memo는 Hook이 아니기 떄문에 클래스형 컴포넌트에서도 사용할 수 있습니다.

함수형 컴포넌트에서는 shouldComponentUpdate를 사용할 수 없는데 리액트 공식 문서에서는 그 대안으로 React.memo를 제시하고 있습니다.

React.memo는 콜백함수를 이용해 메모이제이션을 적용할지 여부를 판단할 수도 있습니다.

## 컴포넌트를 매핑할 때 ket값으로 Index를 사용하지 않습니다.

리액트에서 컴포넌트를 매핑할 때에는 반드시 고유 key를 부여하도록 강제하고 있습니다.

중간에 어떤 배열에 어떤 요소가 삽입되면 그 중간보다 이후에 위치한 요소들은 전부 인덱스가 변경됩니다.

이로 인해 **key값이 변경**되고 **리마운트**가 일어나게 되죠. 또한, 데이터가 key와 매치가 안되어 서로 꼬이는 **부작용도 발생**합니다.

결론적으로 key값에 **고유 id**를 넣어주면, 배열의 중간에 어떤 요소가 삽입되더라도 기존에 있는 원소들이 가지고 있는 key가 끊어질 위험이 없습니다.

## useMemo

만약 컴포넌트 내에 어떤 함수가 값을 리턴하는데 많은 시간을 소요한다면, 이 컴포넌트가 리렌더링 될 때마다 **함수가 호출되면서 많은 시간을 소요**하게 될 것입니다.

그리고 그 함수가 반환하는 값을 하위 컴포넌트가 사용한다면, 그 하위 컴포넌트는 **매 함수호출마다 새로운 값을 받아 리렌더링**할 것입니다.

useMemo는 **종속 변수**들이 변하지 않으면 함수를 굳이 다시 호출하지 않고 이전에 반환한 참조값을 **재사용**합니다.

즉, **함수 호출 시간**도 절약할 수 있고, 같은 값을 props로 받는 하위 컴포넌트의 **리렌더링도 방지**할 수 있습니다.

- 작성중
  https://cocoder16.tistory.com/36
