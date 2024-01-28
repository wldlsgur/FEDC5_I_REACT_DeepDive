# 리액트의 모든 훅 파헤치기

### useState

useState는 함수형 컴포넌트 내부에서 상태를 정의하고, 이 상태를 관리할 수 있게 해주는 훅.

리액트의 렌더링은 함수형 컴포넌트에서 반환한 결과물은 return의 값을 비교해 실행하기 때문에 렌더링이 발생될 때마다 함수는 다시 새롭게 실행되고, 실행되는 함수에서 변수는 매번 초기화된다.

그런데 useState 훅의 결괏값은 어떻게 리렌더링돼도 그 값을 유지할 수 있을까?

리액트엣는 클로저를 이용한다.

클로저를 사용함으로써 외부에 해당 값을 노출시키지 않고 오직 리액트에서만 쓸 수 있었고, 함수형 컴포넌트가 매번 실행되더라도 useState에서 이전의 값을 정확하게 꺼내 쓸 수 있게 됐다.

#### 게으른 초기화

useState의 인수로 특정한 값을 넘기는 함수를 인수로 넣어줄 수도 있다. useState에 변수 대신 함수를 넘기는 것을 게으른 초기화(lazy initialization)이라고 한다.

```jsx
// 바로 변수를 넣는 방법
const [count, setCount] = useState(
    Number.parseInt(window.localStorage.getItem(cacheKey))
);

// 함수를 넣는 게으른 초기화 방법
const [count, setCount] = useState(() =>
    Number.parseInt(window.localStorage.getItem(cacheKey))
);
```

리액트에서 게으른 초기화는 useState의 초기값이 복잡하거나 무거운 연산을 포함하고 있을 때 사용하라고 돼 있다.

이 게으른 초기화 함수는 오로지 state가 처음 만들어질 때만 사용된다. 만약 이후에 리렌더링이 발생된다면 이 함수의 실행은 무시된다.

렌더링이 실행될 때마다 함수형 컴포넌트의 함수가 다시 실행된다는 점을 생각해보면 useState의 값도 재실행된다.

내부 클로저가 존재하기 때문에 초기값은 최초에만 사용되는 것이다.

만약 useState 내부에 함수를 넣으면 이는 최초 렌더링 이후에는 실행되지 않고 최초의 state 값을 넣을 때만 실행된다.

리액트에서는 무거운 연산이 요구될 떄 사용하라고 한다. localStorage나 sessionStorage에 대한 접근, map, filter, find 같은 배열에 대한 접근, 초기값 계산을 위한 함수 호출이 필요할 때 무거운 연산을 포함해 실행 비용이 많이 드느 ㄴ경우에 게으른 초기화를 사용하는 것이 좋다.
