# JSX란?

JSX는 자바스크립트 내부에서 표현하기 까다로웠던 XML 스타일의 트리 구문을 작성하는데 많은 도움을 주는 새로운 문법이라고 볼 수 있다

## JSX의 정의

JSXElement, JSXAttributes, JSXChildren, JSXString라는 4가지 컴포넌트를 기반으로 구성

### JSXElement

JSX를 구성하는 가장 기본 요소로, HTML의 요소(element)와 비슷한 역할을 한다.

- JSXOpeningElement: 일반적으로 볼 수 있는 요소. JSXOpeningElement로 시작했다면 후술할 JSXClosingElement가 동일한 요소로 같은 단계에서 선언돼 있어야 올바른 JSX 문법으로 간주된다

  - `<JSXElement JSXAttributes(optional)>`

- JSXClosingElement: JSXOpeningElement가 종료됐음을 알리는 요소. 반드시 JSXOpeningElement와 쌍으로 사용돼야 한다

  - `<JSXElement />`

- JSXSelfClosingElement: 요소가 시작되고, 스스로 종료되는 형태를 의미한다. `<script/>`와 동일한 모습을 띠고 있다. 이는 내부적으로 자식을 포함할 수 없는 형태를 의미한다.

  - `<JSXElement JSXAttributes(optional) />`

- JSXFragment: 아무런 요소가 없는 형태로, JSXSelfClosingElement 형태를 띨 수는 없다. </>는 불가능하다. 단 <></>는 가능하다.
  - <>JSXChildren(optional)</>

#### JSXElementName

JSXElement의 요소 이름으로 쓸 수 있는 것을 의미

**JSXIdentifier**

- JSX 내부에서 사용할 수 있는 식별자를 의미한다. 이는 자바스크립트 식별자 규칙과 동일하다. 즉, <$></$>, <_></_>도 가능하지만 자바스크립트와 마찬가지로 숫자로 시작하거나 $와 \_ 외의 다른 특수문자로는 시작할 수 없다

```JavaScript
function Valid1() {
  return <$></$>
}

function Valid2() {
  return <_></_>
}

// 불가능
function Valid3() {
  return <1></1>
}
```

**JSXNamespacedName**

- JSXIdentifier: JSXIdentifier의 조합, 즉 :을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다. :로 묶을 수 있는 것은 한 개뿐이다. 두 개 이상은 올바른 식별자로 취급X

```JavaScript
function valid() {
  return <foo:bar></foo:bar>
}

// 불가능
function valid() {
  return <foo:bar:baz:baz></foo:bar:baz:baz>
}
```

**JSXMemberExpression**

- JSXIdentifir.JSXIdentifier의 조합, 즉 .을 통해 서로 다른 식별자를 이어주는 것도 하나의 식별자로 취급된다. :로 묶는 JSXNamespacedName과는 다르게 .을 여러 개 이어서 하는 것도 가능하다. 단 JSXNamespacedName과 이어서 사용하는 것은 불가능

```JavaScript
// 가능
function valid1() {
    return <foo.bar></foo.bar>
}

// 가능
function valid2() {
    return <foo.bar.baz></foo.bar.baz>
}

// 불가능
function  invalid() {
    return <foo:bar.baz></foo:bar.baz>
}
```

### JSXAttributes

JSXElement에 부여할 수 있는 속성을 의미. 단순히 속성을 의미하기 때문에 모든 경우에서 필수 값이 아니고, 존재하지 않아도 에러가 나지 않는다

**JSXSpreadAttributes**

- 자바스크립트의 전개 연산자와 동일한 역할을 한다고 볼 수 있다
- { ...AssignmentExpression } : 이 AssignmentExpression에는 단순히 객체뿐만 아니라 자바스크립트에서 AssignmentExpression으로 취급되는 모든 표혀식이 존재할 수 있다. 조건문 표현식, 화살표 함수, 할당식 등

**JSXAttribute**

- 속성을 나타내는 키와 값으로 짝을 이루어서 표현한다. 키는 JSXAttributeName, 값은 JSXAttributeValue로 불린다
- JSXAttributeName: 속성의 키 값
- JSXAttributeValue: 속성의 키에 할당할 수 있는 값. "큰 따옴표로 구성된 문자열", '작은 따옴표로 구성된 문자열', { AssignmentExpression }, JSXElement, JSXFragment 중 하나를 만족해야 한다

### JSXChildren

JSXElement의 자식 값을 나타낸다. JSX는 트리 구조를 나타내기 위해 만들어졌고 부모와 자식 관계를 나타낼 수 있으며, 그 자식을 JSXChildren이라고 한다

#### JSXChild

JSX를 이루는 기본 단위. JSXChildren은 JSXChild를 0개 이상 가질 수 있다. JSXChildren은 JSXChild가 없어도 상관없다.

- JSText: {, <, >, }을 제외한 문자열 이는 다른 JSX 문법과 혼동을 줄 수 있기 때문

```JavaScript
function App() {
    return <>{"{} <>"}</>
}
```

- JSXElement: 값으로 다른 JSX 요소가 들어갈 수 있다
- JSXFragment: 값으로 빈 JSX 요소인 <></>가 들어갈 수 있다
- { JSXChildExpression (optional) }: 이 JSXChildExpression은 자바스크립트의 AssignExpression을 의미힌다.

```JavaScript
export default function App() {
  return <>{(() => 'foo')()}</>
}
```

### JSXString

JSXAttributeValue와 JSXText는 HTML과 JSX 사이에 복사와 붙여넣기를 쉽게 할 수 있도록 설계돼 있다

HTML에서 사용 가능한 문자열은 모두 JSXString에서도 가능하다. 이는 개발자가 HTML의 내용을 손쉽게 JSX로 가져올 수 있도록 설계된 부분이다.

문자열은 큰 따옴표로 구성된 문자열, 작은 따옴표로 구성된 문자열, JSXText를 의미

## JSX 예제

```JavaScript
// 하나의 요소로 구성된 가장 단순한 형태
const ComponentA = <A>안녕하세요. </A>

// 자식 없이 SelfCLosingTag로 닫혀 있는 형태
const ComponentB = <A />

// 옵션을 {}와 전개 연산자로 넣을 수 있다.
const ComponentC = <A { ...{ required: true } } />

// 속성만 넣어도 가능
const ComponentD = <A required />

// 속성과 속성을 넣을 수 있다
const ComponentE = <A required={false} />

const ComponentF = (
  <A>
    {/*문자열 큰따옴표 작은따옴표 모두 가능*/}
    <B text="리액트"/>
  </A>
)

const ComponentG = (
    <A>
      {/*옵션의 값으로 JSXElement를 넣는 것 또한 올바른 문법*/}
        <B optionalChildren={<>안녕하세요.</>}/>
    </A>
)

const ComponentH = (
    <A>
        {/*여러 개의 자식도 포함할 수 있다*/}
        안녕하세요
        <B text="리액트"/>
    </A>
)
```

## JSX는 어떻게 자바스크립트에서 변환될까?

JSX가 변환되는 방식을 알려면 리액트에서 JSX를 변환하는 @babel/plugin-transform-react-jsx 플러그인을 이용해 JSX 구문을 자바스크립트가 이해하는 형태로 변환한다.

```JSX

// JSX 코드
const Component = <>Hello World</>

// @babel/plugin-transform-react-jsx로 변환
var Component = React.createElement(React.Fragment, null, 'Hello World')

// 리액트 17, 바벨 7.9.0 이후 버전에서 추가된 자동 런타임으로 트랜스파일한 결과

var Component = (0, _jsxRuntime.jsx)(_jsxRuntime.Fragment, {
  children: 'Hello World',
})
```

두 결과물에 약간의 차이가 있지만 다음과 같은 공통점이 있다

- JSXElement를 첫 번째 인수로 선언해 요소를 정의한다
- 옵셔널인 JSXChildren, JSXAttributes, JSXString은 이후 인수로 넘겨주어 처리한다.

이 점을 활용하면 경우에 따라 다른 JSXElement를 렌더링해야 할 때 요소 전체를 감싸지 않더라도 처리할 수 있다

```JavaScript
function Heading(isHeading) {
  return isHeading ? (
    <h1 className="text">Go</h1>
  ) : (
    <span className="text">Go</h1>
  )
}

// JSX가 변환되는 특성을 활용하여 간결하게 처리
function Heading(isHeading) {
  return createElement(
    isHeading ? 'h1' : 'span',
    { className: 'text'},
    "Go",
  )
}
```

JSX 반환값이 React.createElement로 귀결된다는 사실을 파악하면 이런 식으로 쉽게 리팩터링 할 수 있다

## 정리
