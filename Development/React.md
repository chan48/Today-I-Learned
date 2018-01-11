- [React.js 공식 문서](https://reactjs.org/)
- [React.js 한글 번역 문서](https://reactjs-kr.firebaseapp.com/)



# 개요

React: 유저 인터페이스 빌드를 위한 JavaScript 라이브러리

- 선언형: 인터렉티브한 UI를 쉽게 만들 수 있음. 애플리케이션의 각 상태에 대한 간단한 뷰를 설계하면 데이터가 변경될 때 적절한 구성요소만 효과적으로 업데이트하고 렌더링함. 선언형 뷰는 예측 가능하고 디버그 하기 쉬운 코드를 만들어줌.
- 컴포넌트 기반: 상태를 가지고 관리하는 캡슐화된 컴포넌트를 생성한 다음 복잡한 UI를 만들기 위해 구성함. 컴포넌트 로직은 템플릿 대신 JavaScript로 작성되므로 애플리케이션을 통해 풍부한 데이터를 쉽게 전달하고 DOM에서 상태를 유지할 수 있음.
- 한 번 배우고 어디서나 작성할 수 있음: 나머지 기술 스택이 무엇이 될지 가정하지 않았으므로 기존 코드를 다시 작성하지 않고 React에서 새로운 기능을 개발할 수 있음. React Native를 이용하여 모바일앱을 만들거나 Node를 이용한 서버에서 렌더링을 할 수도 있음.



### 단일 컴포넌트

React 컴포넌트는 입력 데이터를 받고 화면에 표시할 것을 반환하는 `render()` 메서드를 구현. JSX라는 유사 XML 문법을 사용함. 컴포넌트에서 전달하는 입력 데이터는 `this.props`를 통해 `render()`에서 접근할 수 있음.

**JSX는 React를 사용할 때 필수가 아닌 옵션임.**

```Jsx
class HelloMessage extends React.Component {
	render() {
        return (
			<div>	
            	Hello {this.props.name}
          	</div>
        );
    }
}

ReactDOM.render(
  	<HelloMessage name="Taylor" />,
	mountNode // todo
);
```



### 상태 기반 컴포넌트

입력 데이터를 가져오는 것 외에도(`this.props`를 통해 접근), 컴포넌트는 내부 상태 데이터를 관리할 수 있음(`this.state`를 통해 접근). 컴포넌트의 상태 데이터가 바뀌면, 렌더링된 마크업은 `render()`를 다시 호출하여 업데이트 됨.

```Jsx
class Timer extends React.Component {
  constructor(props) {
    super(props);
    this.state = { seconds: 0 };
  }
  
  tick() {
    this.setState(prevState => ({
      seconds: prevState.seconds + 1
    }));
  }
  
  componentDidMount() {
    this.interval = setInterval(() => this.tick(), 1000);
  }
  
  componentWillUnmount() {
    clearInterval(this.interval);
  }
  
  render() {
    return (
		<div>
			Seconds: {this.state.seconds}
		</div>
    );
  }
}

ReactDOM.render(<Timer />, mountNode);
```



### 애플리케이션

`props`와 `state`를 사용하여 간단한 Todo 애플리케이션을 만들 수 있음. 이 예제에서는 `state`를 사용하여 사용자가 입력한 텍스트뿐만 아니라 아이템의 현재 리스트를 추적. 이벤트 핸들러는 인라인으로 보이지만, 이벤트 위임을 사용해 구현하고 수집됨.

```Jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: [], text: '' };
    this.handleChange = this.handleChange.bind(this); 
    this.handleSumbit = this.handleSubmit.bind(this);
  }
  
  render() {
    return (
    	<div>
    		<h3>TODO</h3>
    		<TodoList items={this.state.items} />
    		<form onSubmit={this.handleSubmit}>
    			<input
    				onChnage={this.handleChange}
    				value={this.state.text}
    			/>
    			<button>
    				Add #{this.state.items.length + 1}
    			</button>
    		</form>
    	</div>
    );
  }
  
  handleChange(e) {
    this.setState({ text: e.target.value });
  }
  
  handleSubmit(e) {
    e.preventDefault();
    if (!this.state.text.length) {
      return;
    }
    const newItem = {
      text: this.state.text,
      id: Date.now()
    };
    this.setState(prevState => ({
      items: prevState.items.concat(newItem),
      text: ''
    }));
  }
}

class TodoList extends React.Component {
  render() {
    return (
    	<ul>
    		{this.props.items.map(item => (
    			<li key={item.id}>{item.text}</li>
    		))}
    	</ul>
    );
  }
}

ReactDOM.render(<TodoApp />, mountNode);
```



## 외부 플러그인을 사용한 컴포넌트

React는 유연하며 다른 라이브러리나 프레임워크에 접근할 수 있도록 해주는 훅을 제공함. 이 예제에서는 외부 마크다운 라이브러리인 **remarkable**을 사용하여 `<textarea>`의 값을 실시간으로 변환해줌.

```Jsx
class MarkdownEditor extends React.component {
  constructor(props) {
    super(props);
    this.handleChnage = this.handleChnage.bind(this);
    this.state = { value: 'Type some *markdown* here!' };
  }
  
  handleChnage(e) {
    this.setState({ value: e.target.value });
  }
  
  getRawMarkup() {
    const md = new Remarkable();
    return { __html: md.render(this.state.value) };
  }
  
  render() {
    return (
    	<div className="MarkdownEditor">
    		<h3>Input</h3>
    		<textarea
    			onChange={this.handleChange}
    			defaultValue={this.state.value}
    		/>
    		<h3>Output</h3>
    		<div 
    			className="content"
    			dangerouslySetInnerHTML={this.getRawMarkup()}
    		/>
    	</div>
    );
  }
}

ReactDom.render(<MarkdownEditor />, mountNode);
```



## Tutorial: Intro To React



## 시작하기 전에

### 무엇을 빌드하고 있는가

가이드에서는 반응형 틱택톡 게임을 구현할 예정입니다.

결과물을 확인하길 원한다면 [이곳](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)에서 확인할 수 있습니다. 아직 이해가 되지 않거나 친숙하지 않은 문법이 사용되도 걱정하지 마세요. 가이드를 통해 한 단계씩 틱택톡 게임을 어떻게 구현하는지 배울 예정입니다.

게임을 해보세요. 이동 리스트에 있는 버튼을 클릭하여 그 시간 대로 이동하고, 이동한 후 보드가 어떻게 보이는지 볼 수 있습니다.

일단 게임에 익숙해지면 마음껏 탭을 닫으세요. 다음 섹션에서 간단한 템플릿부터 시작할 것입니다.



### 미리 준비할 것들

HTML과 JavaScript에 익숙할 것이라 가정하여 가이드를 진행할 예정이다. 만약 이전에 HTML과 JavaScript를 사용해본적이 없더라도 가이드를 따라올 수 있어야 합니다.

만약 JavaScript에 대해 다시 알 필요가 있다면 우리는 [이 가이드](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)를 추천합니다. 또한 JavaScript의 최신 버전인 ES6의 몇 가지 특징들을 사용하고 있음을 알아두세요. 가이드에서 우리는 [화살표 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 상태들을 사용합니다. [Babel REPL](https://babeljs.io/repl/#?presets=react&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUPGDADkdECChWeASl4AlOMOBQAIgHkAssp0aIySpogoaFBUQmISdC48QA)를 사용하여 ES6가 코드가 어떻게 컴파일되는지 확인할 수 있습니다.



### 어떻게 가이드를 따라하면 되나요

가이드를 끝까지 따라오기 위한 두 가지 방법이 있습니다. 브라우저에서 코드에 쓰거나 컴퓨터의 로컬 개발 환경을 설치할 수 있습니다. 편하게 느껴지는 방법을 선택하여 따르시면 됩니다.

#### 브라우저에서 코드를 쓰기 원한다면

가장 빠르게 시작할 수 있습니다!

첫 번째로 새로운 탭에서 [시작 코드](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)를 여세요. 빈 틱택톡 필드가 보일 것입니다. 가이드에서 이 코드를 수정할 것입니다.

로컬 개발 환경 설정에 관한 다음 섹션을 스킵할 수 있습니다. 바로 오버뷰로 넘어가세요.

#### 에디터에서 코드를 쓰기 원한다면

컴퓨터에 프로젝트를 설치할 수 있습니다.

안내: **이 방법은 선택이지 필수가 아닙니다!**

이 방법은 더 많은 것을 해야하지만 에디터의 편리함을 이용할 수 있습니다.

만약 이 방법을 해보길 원한다면 다음의 스텝들을 따라하세요.

1. 설치된 `Node.js`가 최신 버전인지 확인하세요.
2. 새로운 프로젝트를 생성하기 위해 [설치 방법](https://reactjs.org/docs/add-react-to-a-new-app.html)을 따라하세요.

```Shell
$ npm install -g create-react-app
$ create-react-app my-app
```

3. 새 프로젝트의 `src/` 폴더에 있는 모든 파일들을 삭제하세요. (내용만 삭제하고 폴더는 삭제하지 마세요.)

```shell
$ cd my-app
$ rm -f src/*
```

4. [이 CSS 코드](https://codepen.io/gaearon/pen/oWWQNa?editors=0100)를 `src/` 폴더에 있는 이름이 `index.css`인 파일을 추가하세요.
5. [이 JS 코드](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)를 `src/` 폴더에 있는 이름이 `index.js`인 파일을 추가하세요.
6. `src/` 폴더에 있는 `index.js`의 맨 위에 아래 세줄을 추가하세요.

```Js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

이제 프로젝트 폴더에서 `npm start` 명령어를 실행하고 브라우저에서 `http://localhost:3000`를 여세요. 그러면 빈 틱택톡 필드를 볼 것입니다.

여러분의 데이터에서 문법 하이라이팅 설정을 하고 싶다면 [이 문서](http://babeljs.io/docs/editors)를 추천합니다.



### 도와주세요! 막히는 부분이 있어요!

만약 막히는 부분이 있다면 [커뮤니티 리소스](https://reactjs.org/community/support.html)를 확인하세요. 특히 [Reactiflux chat](https://reactjs.org/community/support.html#reactiflux-chat)는 빠르게 도움을 받기 아주 좋은 방법입니다. 만약 어디에서도 좋은 대답을 듣지 못했다면 이슈를 올리세요. 그러면 우리가 도와드립니다.

이 과정이 끝났다면 시작해봅시다!



## 오버뷰

### React란?

React는 유저 인터페이스 구현을 위한 선언적이고, 효율적이고, 유동적인 JavaScript 라이브러리.

몇 가지 다른 종류의 컴포넌트들을 가지고 있지만 `React.Component`의 서브클래스들을 가지고 시작할 것입니다.

```Jsx
class ShoppingList extends React.Component {
  render() {
    return (
    	<div className="shopping-list">
    		<h1>Shopping List for {this.props.name}</h1>
    		<ul>
    			<li>Instagram</li>
    			<li>WhatsApp</li>
    			<li>Oculus</li>
    		</ul>
    	</div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />	
```

잠시동안 재미있는 XML과 비슷한 태그들을 사용할 것입니다. 컴포넌트들은 React에게 당신이 무엇을 렌더하고 싶은지 알려줍니다. 그러면 React는 효율적으로 데이터가 변할 때 올바른 컴포넌트들을 곧장 업데이트하고 렌더합니다.

여기에서 ShoppingList는 **React 컴포넌트 클래스**나 **React 컴포넌트 타입**입니다. 하나의 컴포넌트는 `props`라 불리는 파라미터를 사용하고 `render` 메서드를 통해 보여줄 뷰 계층을 리턴해줍니다.

`render` 메서드는 랜더링하길 원하는 *description*을 리턴한 다음 React는 description을 가져오고 스크린에 랜더합니다. 특히, `render`는 랜더할 내용인 간단한 description인 **React 요소**를 리턴합니다. 대부분의 React 개발자들은 JSX라 불리는 특별한 문법을 사용합니다. JSX는 디 구조들을 쓰기 쉽게 만들어줍니다. `<div />`은 빌드 시  `React.createElement('div')`로 변환됩니다. 다음의 예제와 동일합니다.

```Jsx
return React.createdElement('div', {className: 'shopping-list},
	React.createdElement('h1', /* ... h1 children ... */},
	React.createdElement('ul', /* ... ul children ... */}
);
```

전체 코드는 [여기](https://babeljs.io/repl/#?presets=react&code_lz=DwEwlgbgBAxgNgQwM5IHIILYFMC8AiJACwHsAHUsAOwHMBaOMJAFzwD4AoKKYQgRg65cAyiXJVqUADKMmUAGbEATlADepRWSQA6SpiwBfTtwD0fAdwCucc12ANWASUrME1RZmDH7R2_YDqhAhMSACC5J7egtz2APIwVhZIEWDmnlYcnuAQrADc7EA)에서 볼 수 있습니다.

만약 더 궁금하다면 `createElement()`에 대한 더 자세한 설명을 [API reference](https://reactjs.org/docs/react-api.html#createelement)에서 볼 수 있습니다. 이 가이드에서는 직접적으로 이 것을 사용하지 않을 예정입니다. 우리는 JSX를 사용할 것입니다.

JSX에 있는 중괄호 안에서 JavaScript 문법을 사용할 수 있습니다. 각 React 요소는 변수에 저장하거나 프로그램을 전달할 수 있는 실제 JavaScript 객체입니다.



`ShoppingList` 컴포넌트는 DOM 컴포넌트 구성 요소만 랜더링하지만 `<ShoppingList />` 코드를 작성하여 커스텀 React 컴포넌트들을 쉽게 구성할 수 있습니다. 각 컴포넌트는 캡슐화되고 독립적으로 동작할 수 있습니다. 이는 간단한 컴포넌트들로 복잡한 UI들을 구현할 수 있게 해줍니다.



### 시작하기

[예시 코드](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)를 통해 시작해봅시다.

이 코드는 오늘 우리가 구현할 것의 틀을 포함하고 있습니다. 필요한 스타일들이 준비되어 있기 때문에 JavaScript만 신경쓰면 됩니다.

세 가지의 컴포넌트들로 구성되어 있습니다.

- Square
- Board
- Game

Square 컴포넌트는 하나의 `<button>`을 랜더링합니다. Board 컴포넌트는 9개의 사각형들을 랜더링합니다. Game 컴포넌트는 나중에 우리가 채워 넣을 placeholder를 가진 하나의 보드를 랜더링합니다. 이 시점에서 이 컴포넌트들은 아무런 동작도 하지 않습니다.



### props를 통해 데이터 전달하기

이제 본격적으로 시작하기 위해 Board 컴포넌트로부터 Square 컴포넌트로 데이터를 전달해봅시다.

Board의 `renderSquare` 메서드에서 Square 컴포넌트 prop에  `value` 데이터를 전달하기 위해 코드를 변경해주세요.

```Js
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
```

변경한 뒤 value 데이터를 보여주기 위해 Square 컴포넌트의 `render` 메서드 안의 `{/* TODO */} ` 코드를  `{this.props.value}`로 변경해주세요.

변경 전:

![https://reactjs.org/static/tictac-empty-1566a4f8490d6b4b1ed36cd2c11fe4b6-a9336.png](https://reactjs.org/static/tictac-empty-1566a4f8490d6b4b1ed36cd2c11fe4b6-a9336.png)

변경 후: 랜더링된 결과물에서 각 사각형 안에 숫자가 있는 것을 볼 수 있습니다.

![https://reactjs.org/static/tictac-numbers-685df774da6da48f451356f33f4be8b2-be875.png](https://reactjs.org/static/tictac-numbers-685df774da6da48f451356f33f4be8b2-be875.png)

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/aWWQOG?editors=0010)에서 볼 수 있습니다.



### 인터렉티브 컴포넌트

클릭 시 "X"로 채워시는 Square 컴포넌트를 만들어봅시다. Square의 `render()` 함수에서 반환된 버튼 태그를 다음과 같이 변경해봅시다.

```Js
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

지금 사각형을 클릭하면 브라우저에서 알럿창이 뜨는걸 볼 수 있습니다.

이는 새로운 JavaScript 화살표 함수 문법을 사용한 것입니다. `onClick` prop에 함수를 넘긴 것을 유의해주세요. `onClick={alert('click')}`은 버튼을 클릭하면 즉시 알럿창을 띄워줍니다.

React 컴포넌트들은 생성자 안의 `this.state`를 설정하여 상태를 가질 수 있습니다. 이 상태는 각 컴포넌트 별로 가지고 있습니다. 사각형의 현재 value 데이터를 state에 저장하고 사각형을 클릭할 때 바꿔봅시다.

첫 번째로 state를 초기화하기 위해 클래스에 생성자를 추가해줍니다.

```js
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }
   
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

JavaScript 클래스에서 서브클래스의 생성자를 정의할 때 `super();` 메서드를 명시적으로 호출해야 합니다.

Square `render` 메서드를 현재 state에서 value 데이터가 보이고 클릭 시 바뀌도록 변경해봅시다.

- `<button>` 태그 안의 `this.state.value` 를 `this.props.value`로 변경하기
- `() => alert()` 이벤트 핸들러를 `() => this.setState({value: 'X'})`로 변경하기

다음과 같이 `<button>` 태그가 변경되면 됩니다.

```Js
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }
   
  render() {
    return (
      <button className="square" onClick={() => this.setState({value: 'X'})}>
        {this.state.value}
      </button>
    );
  }
}
```

`this.setState`가 호출될 때마다 컴포넌트 업데이트가 예정되어 있으므로 전달된 상태 업데이트에서 React가 병합되고 하위 컴포넌트와 함께 다시 랜더링됩니다. 컴포넌트가 랜더링될 때 `this.state.value`는 `'X'`가 되고 그리드 안에 X가 보일 것입니다.

사각형을 클릭하면 그 안에 X가 보일 것입니다.

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/VbbVLg?editors=0010)에서 볼 수 있습니다.



### 개발자 도구

[크롬](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?hl=en)과 [파이어폭스](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)에서 사용하는 React 개발자 도구 확장 프로그램은 React 컴포넌트 트리를 브라우저 개발자 도구 안에서 검사할 수 있게 해줍니다.

![https://reactjs.org/static/devtools-878d91461c78d8f238e116477dfe0b46-6ca3b.png](https://reactjs.org/static/devtools-878d91461c78d8f238e116477dfe0b46-6ca3b.png)

트리 안의 컴포넌트들의 props와 state를 검사할 수 있게 해줍니다.

설치 후 페이지에 있는 원하는 컴포넌트를 오른쪽 클릭하고 "Inspect"를 클릭하여 개발자 도구를 열면 React 탭이 오른쪽의 마지막 탭으로 나타납니다.

**그러나 CodePen를 사용하여 확장 프로그램을 동작시키고 싶다면 추가적으로 필요한 단계들이 있습니다.**

1. 로그인 혹은 회원가입을 하여 이메일을 인증받으세요. 
2. "Fork" 버튼을 클릭하세요.
3. "Chnage View"를 클릭하고 "Debug mode"를 선택하세요.
4. 새롭게 열린 탭에서는 React 탭이 있는 개발자 도구를 볼 수 있습니다.

