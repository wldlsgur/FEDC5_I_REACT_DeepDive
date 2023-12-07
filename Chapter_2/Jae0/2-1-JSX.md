💡 JSX는 React가 등장하면서 새롭게 등장한 문법임

React의 문법이라고 생각할 수 있지만 React에 종속되지 않은 XML과 같은 하나의 문법.

하지만, JS ECMAScript 라고 불리는 JavaScript 문법의 표준 문법에 해당하지 않음.

그렇기 때문에 JSX를 JS에서 사용하기 위해서는 별도의 트랜스파일러를 거쳐야 JS 런타임 환경에서

이해할 수 있는 의미 있는 JS코드로 변환됨.

🔥즉! JS 내부의 표현하기 까다로운 XML 트리 구문을

좀 더 쉽게 작성하도록 도와주기 위한 문법

**특징**

- JSX 내부에서 태그에 아무것도 전달하지 않으면 React.Fragmant 태그의 사용과 동일함
- JSX 내부의 component명은 항상 대문자로 시작해야함
  ⇒ 기존의 HTML 요소들과 구분을 짓기 위함! HTML 요소는 이름이 소문자로 시작됨

**네이밍**

- JS의 식별자 규칙과 동일함. 따라서 `$` `_` 외의 다른 특수문자로 시작할 수 없음
- `:` 을 통해 `식별자:식별자` 조합으로 사용가능. 단 한번만 `:` 을 사용 가능함
- `.` 을 통해 `식별자.식별자` 조합으로 이어서 사용가능함. `.`은 여러번 이어줄 수 있음

**JSX의 자체 정의**

- JSXOpeningElement `< Element >`
  가장 일반 적인 요소로 항상 뒤에는 JSXClosingElement가 동일한 Element로 선언되어짐
- JSXClosingElement `</ Element >`
  JSXOpeningElement 가 종료 되었음을 알려주는 Element
- JSXSelfClosingElement `< Element />`
  Element가 시작되어지고 스스로 종료되어지는 형태를 의미하고, 내부에 자식 Element는 존재할 수 없음
- JSXFragment `<>` `</>`
  아무런 Element가 없는 형태로 JSXSelfClosingElement형태로는 사용이 불가능함

```jsx
// JSXFragment
<>

	// JSXOpeningElement
	<Container>

		// JSXSelfClosingElement
		<Box />

	//JSXSelfClosingElement
	<Container/>

// JSXFragment
</>
```

**표현식 사용**

사용하고자 하는 표현식을 중괄호 `{}` 로 감싸서 사용함

- JSX자체도 표현식으로 `if` `for` 안에서도 사용 가능하고 변수, 함수 에도 사용 가능함

```jsx
return (
  // 표현식을 중괄호 {} 를 통해 넣을 수 있음
  <h1>{1 + 1}</h1>
);

const jsx = <div></div>;
```

**속성 정의**

속성에 값으로 문자열 리터럴을 통해 속성을 정의할 수 있음

또, 중괄호 `{}`를 사용하여 속성 값으로 표현식을 넣을 수 있음 ⇒ 다만 두 방식을 동시에 사용은 불가능

```jsx
<div className="string"></div>

const name = "표현식임"
<div className={name}>
```

**조심**

하나의 Component에 JSX문법이 많아지면 오히려 가독성이 매우 떨어지는 단점이 존재함
