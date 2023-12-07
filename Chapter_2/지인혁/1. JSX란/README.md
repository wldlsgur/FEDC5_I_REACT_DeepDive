# JSX란?

JSX는 흔히 개발자들이 알고 있는 XML과 유사한 내장형 구문이며, **리액트에서 종속적이지 않은 독자적인 문법**이다.

페이스북에서 독자적으로 개발했다는 사실에서 JSX는 ECMAScript라고 불리는 자바크스립트 표준의 일부는 아니다. 즉 브라우저에 의해서 실행되거나 표현되도록 만들어진 구문이 아니라는 뜻이다.

JSX는 자바스크립트 표준 코드가 아닌 페이스북이 임의로 만든 새로운 문법이기 때문에 JSX는 반드시 트랜스파일러를 거쳐야 자바스크립트 런타임이 이해할 수 있는 의미 있는 자바스크립트 코드로 변환된다.

JSX의 설계 목적은 다양한 트랜스파일러에서 다양한 속성을 가진 트리 구조를 토큰화에 ECMAScript로 변환하는 데 초점을 두고 있다.

JSX 내부에 트리 구조로 표현하고 싶은 다양한 것들을 작성해두고, 이 JSX를 트랜스파일이라는 과정을 거쳐 자바스크립트(ECMAScript)가 이해할 수 있는 코드로 변경하는 것이 목표다.

요약하자면 **JSX는 자바스크립트 내부에서 표현하기 까다로웠던 XML 스타일의 트리 구무을 작성하는데 많은 도움을 주는 새로운 문법**이다.

<hr>

### JSX의 정의

JSX는 기본적으로 JSXElement, JSXAttributes, JSXChildren, JSXStrings라는 4가지 컴포넌트를 기반으로 구성돼 있다.

#### JSXElement

JSX를 구성하는 가장 기본 요소로, HTML 요소(element)와 비슷한 역할이다.

-   JSXOpeningElement

일반적으로 볼 수 있는 요소. JSXOpeningElement로 시작했다면 JSXClosingElement가 동일한 요소로 같은 단계에서 선언돼 있어야 올바른 JSX 문법이다.

```JSX
<JSXElement>
```

-   JSXClosingElement

JSXOpeningElement가 종료됐음을 알리는 요소. 반드시 JSXOpeningElement와 쌍으로 사용돼야 한다.

```JSX
<JSXElement \>
```

-   JSXSelfClosingElement

요소가 시작되고, 스스로 종료되는 형태. 이는 내부적으로 자식을 포함할 수 없는 형태를 의미한다.

```JSX
<JSXElement JSXAttributes(optional) />
```

-   JSXFragment

아무런 요소가 없는 형태. </>는 불가능하다. 단 <></>는 가능하다.

```JSX
<>JSXChildren(Optional)</>
```

-   요소명은 대문자로 시작해야만 되는거 아닌가?

리액트에서 컴포넌트를 만들어 사용할 때 반드시 대문자로 시작하는 컴포넌트를 만들어야만 사용 가능하다. JSXElement에 명시돼 있는 표준에는 없는 내용인데, 이유는 **리액트에서 HTML 태그명과 사용자가 만든 컴포넌트 태그명을 구분 짓기 위해서다**.

#### JSXElementName

JSXElementName은 JSXElement의 요소 이름으로 쓸 수 있는 것을 의미한다.

-   JSXIdentifier

JSX 내부에서 사용할 수 있는 식별자를 의미. 자바스크립트 식별자 규칙과 동일하며 숫자로 시작하거나 $와 \_외의 다른 특수문자로는 시작할 수 없다.

```JSX
// 불가능
function Invalid1() {
    return <1></1>
}
```

-   JSXNamespaceName : JSXIdentifier:JSXIdentifier의 조합, 즉 :을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다.

:로 묶을 수 있는 것은 한 개뿐이다. 두 개 이상은 올바른 식별자로 취급되지 않는다.

```JSX
// 가능
function Valid() {
    return <foo:bar></foo:bar>
}

// 불가능
function Invalid1() {
    return <foo:bar:baz></foo:bar:baz>
}
```

-   JSXMemberExpression: JSXIdentifier.JSXIdentifier의 조합, 즉 을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다.

:로 묶는 JSXNameSpaceName과는 다르게 .을 여러 개 이어서 하는 것도 가능하다. 단 JSXNameSpacedName과 이어서 사용하는 것을 불가능 하다.

```JSX
// 가능
function Valid() {
    return <foo.bar></foo.bar>
}

// 가능
function Valid() {
    return <foo.bar.baz></foo.bar.baz>
}

// 불가능
function  InValid() {
    return <foo:bar.baz></foo:bar.baz>
}
```

#### JSXAttributes

JSXElement에 부여할 수 있는 속성을 의미. 모든 경우에서 필수값이 아니고, 존재하지 않아도 에러가 나지 않는다.

-   JSXSpreadAttributes

자바스크립트의 전개 연사자와 동일한 역할이다.

```JSX
const App = () => {
  const buttonProps = {
    text: "Click me",
    onClick: () => alert("Button clicked!"),
  };

  return <Button {...buttonProps} />;
};
```

-   JSXAttribute

속성을 나타내는 키와 값으로 짝을 이루어서 표현한다.

JSXAttributeName는 속성의 키 값이다.

JSXAttributeValue는 속성의 키에 할당할 수 있는 값이다. 조건은 큰따옴표, 작은따옴표, { AssignmentExpression } 스프레드 연산자가 값이 될 수 있다.

즉 자바스크립트에서 변수에 값을 넣을 수 있는 표현은 JSX 속성의 값으로도 가능하다.

> JSXElement 값으로 다른 JSX요소가 들어갈 수 있다. 또한 JSXFragment 값으로 비어있는 형태의 JSX요소도 들어갈 수 있다.

#### JSXChildren

JSXElement의 자식 값을 나타낸다.

JSX로 부모와 자식 관계를 나타낼 수 있으며, 그 자식은 JSXChildren이라 한다.

JSXChildren은 JSXChild를 0개 이상 가질 수 있다. 즉 JSXChild가 없어도 상관 없다.

-   JSXText

{, }, <, >를 제외한 문자열이 올 수 있다. 이는 다른 JSX문법과 혼동을 줄 수 있는데 사용하고 싶다면 문자열로 표시하는 방법이 있다.

```JSX
function App() {
    return <>{"{} <>"}</>
}
```

-   JSXElement

값으로 다른 JSX 요소가 들어갈 수 있다.

```JSX
function App() {
    return <div><Component /></div>
}
```

-   JSXFragment

값으로 빈 JSX요소 <></>가 들어갈 수 있다.

```JSX
function App() {
    return <div><></></div>
}
```

-   { JSXChildExpression (optional) }

```JSX
function App() {
    return <div><>{(() => "foo")()}</div>
}
```

#### JSXStrings

JSXAttributeValue와 JSXText는 HTML과 JSX 사이에 복사와 붙여넣기를 쉽게 할 수 있도록 설계돼 있다.

HTML에서 사용 가능한 문자열들은 모두 JSXStrings에서도 가능하다. 이는 개발자가 HTML의 내용을 손쉽게 JSX로 가져올 수 있도록 설계되었다.

자바스크립트와 한 가지 중요한 차이점은 \로 시작하는 이스케이프 문자 형태다. \는 자바스킓트에서 특수문자를 처리할 때 사용되므로 몇 가지 제약 사항이 있지만 HTML에서는 아무런 제약 없이 사용할 수 있다.

> \를 표현하기 위해서 \\로 이스케이프 해야함

<hr>

### JSX 예제

```JSX
// 하나의 요소로 구성된 가장 단순한 형태
const ComponentA = <A>안녕하세요.</A>

// 자식 없이 SelfCLosingTag로 닫혀 있는 형태
const ComponentB = <A />

// 옵션을 {}와 전개 연산자로 넣을 수 있다.
const ComponentC = <A { ...{ required: true } } />

// 속성만 넣어도 가능
const ComponentD = <A required={false} />

const ComponentF = <A>{"문자열 큰따옴표 작은따옴표 모두 가능"}</A>

const ComponentG = (
    <A>
        <B>JSXElement도 가능</B>
    </A>
)

const ComponentH = (
    <A>
        여러 자식도 가능
        <B />
    </A>
)
```

<hr>

### JSX는 어떻게 자바스크립트에서 변환될까?

JSX가 변환되는 방식을 알려면 리액트에서 JSX를 변환하는 @babel/plugin-transform-react-jsx 플러그인을 통해 JSX 구문을 자바스크립트가 이해하는 형태로 변환한다.

```JSX
const ComponentA = (
    <div>
        <span>hello world</span>
    </div>
)

// JSX 코드를 변환한 결과
var ComponentA = (
    "div",
    null,
    React.createElement("span", null, "hello world");
)

// 리액트 17, 바벨 7.9.0 이후 버전에서 추가된 자동 런타임으로 트랜스파일한 결과

var ComponentC = (0, _jsxRuntime.jsx)("div", {
    children: (0, _jsxRuntime.jsx)("span", {
        children: "hello word",
    })
})
```

두 결과물에 차이가 있지만 공통점이 있다.

-   JSXElement를 첫 번째 인수로 선언해 요소를 정의한다.
-   옵셔널인 JSXChildren, JSXAttributes, JSXStrings는 이후 인수로 넘겨주어 처리한다.

이 점을 활용한다면 경우에 따라 다른 JSXElement를 렌더링해야 할 대 굳이 요소 전체를 감싸지 않더라도 처리할 수 있다.

```JSX
function ComponentA(isLoading) {
    return isLoading ? (
        <h1 className="text">Hello</h1>
    ) : (
        <span className="text">Hello</span>
    )
}

function ComponentB(isLoading) {
    return createElement(
        isLoading: ? "h1" : "span",
        { className: "text" },
        "Hello",
    )
}
```

JSX 반환값이 결국 React.createElement로 귀결된다는 사실을 파악하면 이런식으로 쉽게 리팩터링할 수 있다.

<hr>

### 정리

JSX는 자바스크립트 코드 내부에 HTML과 같은 트리 구조를 가진 컴포넌트를 표현할 수 있다는 점에서 각광받고 있다.

JSX가 HTML 문법과 자바스크립트 문법이 뒤섞여서 코드 가독성을 해친다는 의견도 있다. JSX내부에 자바스크립트 문법이 많아질수록 복잡성이 증대하면서 코드의 가독성도 해칠 것이므로 주의해서 사용해야한다.

그리고 리액트 내부에서 JSX가 어떻게 변환되는지, 어떤 결과물을 만들어내는지, 알아두면 향후에 리액트 애플리케이션을 만드는데 도움이 될 수 있다.

때에 따라서는 직접 createElement를 사용해 컴포넌트를 구성하는 편이 더 효율적일 수 있다.

<hr>
