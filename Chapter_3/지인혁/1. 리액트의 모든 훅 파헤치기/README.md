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

리액트에서는 무거운 연산이 요구될 떄 사용하라고 한다. localStorage나 sessionStorage에 대한 접근, map, filter, find 같은 배열에 대한 접근, 초기값 계산을 위한 함수 호출이 필요할 때 무거운 연산을 포함해 실행 비용이 많이 드는 경우에 게으른 초기화를 사용하는 것이 좋다.

<hr>

### useEffect

useEffect는 자주 쓰짐나 사용하기 쉬운 훅은 아니다. 생명주기 메서드를 대체하기 위해 만들어진 훅도 아니다. useEffect의 정의를 정확하게 내리자면 useEffect는 애플리케이션 내 컴포넌트의 여러 값들을 활용해 동기적으로 부수 효과를 만드는 메커니즘이다.

useeEffect는 state와 props의 변화 속에서 일어나는 렌더링 과정에서 실행되는 부수 효과 함수라고 볼 수 있다.

#### 클린업 함수

클린업 함수는 비록 새로운 값을 기반으로 렌더링 뒤에 실행되지만 이 변경된 값을 읽는 것이 아니라 함수가 정의됐을 당시에 선언됐던 **이전 값**을 보고 실행되는 것이다.

#### 의존성 배열

빈 배열을 둔다면 최초 렌더링 직후에 실행된 다음부터는 더 이상 실행되지 않는다.

아무런 값을 넘겨주지 않는다면 렌더링이 발생할 때마다 실행된다. 보통 컴포넌트가 렌더링됐는지 확인하기 윟나 방법으로 사용된다.

만약 의존성 배열이 없는 useEffect가 매 렌더링마다 실행된다면 그냥 useEffect 없이 써도 되는 게 아닐까?? 의문이 든다.

```JSX
function Component() {
    console.log('렌더링 됨')
}

function Component() {
    useEffect(() => {
        console.log("렌더링 됨")
    })
}
```

두 코드는 명확한 차이를 두고 있다.

1. useEffect는 클라이언트 사이드에서 실행되는 것을 보장해준다.

2. useEffect는 컴포넌트의 렌더링이 완료된 이후에 실행된다. 반면 직접 실행은 컴포넌트가 렌더링되는 도중에 실행된다.

useEffect는 컴포넌트가 렌더링된 후에 어떠한 부수 효과를 일으키고 싶을 때 사용하는 훅이다.

#### useEffect를 사용할 때 주의할 점

[] 정말로 의존성으로 빈 배열이 필요하다면 최초에 함수형 컴포넌트가 마운트됐을 시점에만 콜백 함수 실행이 필요한지를 다시 한번 되물어 보자. 만약 정말 "그렇다"하면 useEffect 내 부수 효과가 실행될 위치가 잘못됐을 가능성이 크다.

빈 배열을 넘기는 것은 컴포넌트 상태와 별개로 작동해야하는지, 혹은 여기서 호출하는게 최선인지 한 번 더 검토해야 한다.

빈 배열이 아니라도 만약 특정 값을 사용하지만 해당 값의 변경 시점을 피할 목적이라면 메모이제이션을 적절히 사용해 해당 값의 변활르 막거나 적당한 실행 위치를 다시 한번 고민해 보는 것이 좋다.

> useEffect의 첫 번째 인수에 함수명을 부여하라

보통 useEffect의 첫 번째 인수로 익명 함수를 넣어준다. 하지만 코드가 복잡하고 많아질수록 무슨 일을 하는 useEffect 코드인지 파악하기 어려워진다.

이때 익명함수가 아닌 적절한 이름을 사용한 기명 함수로 바꾸는 것이 좋다. 우리가 변수에 적절한 이름을 붙이는 이유는 해당 변수가 왜 만들어졌는지 파악하기 위함이다.

```JSX
useEffect(function logActiverUser() {
    logging(user.id)
}, [user.id])
```

어색해 보일 수 있지만 useEffect의 목적을 명확히 하고 그 책임을 최소한으로 좁힌다는 점에서 유용하다.

> 거대한 useEffect를 만들지 마라

부수 효과의 크기가 커질수록 애플리케이션 성능에 악영향을 미친다.

가능한 한 useEffect는 간결하고 가볍게 유지한느 것이 좋다. 만약 부득이하게 큰 useEffect를 만들어야 한다면 적은 의존성 배열을 사용하는 여러 개의 useEffect로 분리하는 것이 좋다.

> 불필요한 외부 함수를 만들지 마라

useEffect 내부에서만 실행되는 함수라면 외부에 작성하고 내부에서 실행하는 것 보다 useEffect 내부에서 함수를 정의하고 사용하는 것이 좋다.

> useEffect 콜백 인수로 비동기 함수를 바로 넣을 수 없을까?

이전 state 기반의 응답이 10초가 걸리고, 이후 바뀐 state 기반의 응답이 1초 뒤에 왔다면 이전 state 기반으로 결과가 나와버리는 불상사가 생길 수 있다. 이러한 문제를 경쟁 상태(race condition)라고 한다.

### useMemo

useMemo는 비용이 큰 연산에 댛나 결과를 저장(메모이제이션)해 두고, 이 저장된 값을 반환하는 훅이다.

최적화를 떠올릴 때 가장 먼저 언급되는 훅이 바로 useMemo다.

useMemo는 렌더링 발생 시 의존성 배열의 값이 변경되지 않았으면 함수를 재실행하지 않고 이전에 기억해 둔 해당 값을 반환하고, 의존성 배열의 값이 변경됐다면 첫 번째 인수의 함수를 실행한 후에 그 값을 반환하고 그 값을 다시 기억한다.

메모이제이션은 단순히 값뿐만 아니라 컴포넌트도 가능하다.

```jsx
function App() {
  const [value, setValue] = useState(10);

  // 컴포넌트의 props를 기준으로 컴포넌트 자체를 메모이제이션 했다.
  const MemoizedComponent = useMemo(
    () => <ExpensiveComponent value={value} />,
    [value]
  );
}
```

useMemo로 컴포넌트도 감쌀 수 있다. 하지만 React.memo를 쓰는 것이 더 현명하다.

### useCallback

useCallback은 인수로 넘겨받은 콜백 자체를 기억한다. 특정 함수를 새로 만들지 않고 다시 재사용한다는 의미다.

```jsx
function App() {
  const [status1, setStatus1] = useState(10);

  const toggle = useCallback(
    function toggle1() {
      setStatus1(!status1);
    },
    [status1]
  );
}
```

함수의 재생성을 막아 불필요한 리소스 또는 리렌더링을 방지하고 싶을 때 useCallback을 사용해 볼 수 있다.

예제에선느 기명 함수를 넘겨줬는데 이는 디버깅에 있어 추적하기 용이하게 하기 위함이다. 익명 함수는 함수를 추적하기 어렵기 때문이다.

useCallback은 useMemo를 사용해서 구현할 수 있다.

```jsx
const handleClick1 = useCallback(() => {
  // 함수
}, []);

const handleClick2 = useMemo(() => {
  return; // 함수
});
```

useMemo와 useCallback의 유일한 차이는 메모이제이션을 하는 대상이 변수냐 함수냐일 뿐이다.

useMemo는 값 자체를 메모이제이션하는 용도이기 때문에 반환문으로 함수 선언문을 반환해야 한다.

하지만 혼란을 불러올 수 있으므로 함수를 메모이제이션하는 경우 useCallback을 사용하자.

기억해야 할 사실은 useCallback이나 useMemo는 모두 동일한 역할을 한다.

### useRef

useRef는 useState와 동일하게 컴포넌트 렌더링이 일어나도 변경 가능한 상태값을 저장한다는 공통점이 있다.

하지만 useRef는 current로 값에 접근 또는 변경할 수 있다. 그리고 **그 값이 변하더라도 렌더링을 발생시키지 않는다.**

useRef는 컴포넌트가 렌더링될 때만 생성되며, 컴포넌트 인스턴스가 여러 개라도 각각 별개의 값을 바라본다.

가장 일반적인 사용 예는 바로 DOM에 접근하고 싶을 때다.

```jsx
function RefComponent() {
  const inputRef = useRef();

  console.log(inputRef.current); // undefined

  useEffect(() => {
    console.log(inputRef.current); // <input type="text"></input>
  });

  return (
    <input
      ref={inputRef}
      type="text"
    />
  );
}
```

useRef가 선언된 당시에는 아직 컴포넌트가 렌더링되기 전이라 return으로 컴포넌트의 DOM이 반환되기 전이므로 undefined다.

### useContext

Context란? 부모가 가지고 있는 데이터를 자식에서도 사용하고 싶다면 props로 데이터를 넘겨주는 것이 일반적이다. 하지만 거리가 멀어질수록 코드는 복잡해진다.

이러한 기법을 prop 내려주기(props drilling)이라고 한다.

해당 값을 사용하지 않는 컴포넌트에서도 단순히 값을 전달하기 위해 props가 열려 있어야 하고, 사용하는 쪽도 이렇게 prop 내려주기가 적용돼 있는지 확인해야 하는 등 매우 번거로운 작업이다.

#### useContext를 사용할 때 주의할 점

useContext를 사용하는 컴포넌트를 최대한 작게, 재사용되지 않을 만한 컴포넌트에서 사용해야한다.

루트 컴포넌트에 적용하게되면 더 많은 콘텍스트로 둘러싸일 것이고 해당 props를 다수의 컴포넌트에서 사용할 수 있겎므 해야 하므로 불필요하게 리소스가 낭비된다. 따라서 컨택스트가 미치는 범위는 필요한 환경에서 최대한 좁게 만들어야 한다.

마지막으로 useContext를 상태 관리를 위한 리액트의 API로 오해하고 있는데 콘텍스트는 상태를 주입해 주는 API다.

상태 관리 라이브러리가 되기 위해서는 어떠한 상태를 기반으로 다른 상태를 만들어 낼 수 있어야 하고 필요에 따라 이러한 상태 변화를 최적화할 수 있어야 한다.

콘텍스는 둘 중 어느 것도 하지 못한다. 단순히 props 값을 하위로 전달해 줄 뿐, useContext를 사용한다고 해서 렌더링이 최적화되지는 않는다.
