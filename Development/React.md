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



## 상태 끌어올리기

틱택톡 게임을 하기 위한 기본 블록 구현을 했습니다. 하지만 지금 당장은 각 Square 컴포넌트에서 상태들이 캡슐화되어 있습니다. 제대로 동작하는 게임을 만들기 위해 우리는 어떤 플레이어가 게임에서 이겼는지 확인해야 합니다. 사각형에서 X와 O 를 번갈아 두어야합니다. 누가 이겼는지 확인하기 위해 Square 컴포넌트들을 가로지르지 않고 한 곳에서 9개의 모든 사각형들의 값들을 가지고 있을 것입니다.

Board가 각 Square의 현재 상태가 무엇인지만 조사해야 한다고 생각할지도 모릅니다. React에서 이 방법이 기술적으로 가능하지만 이는 코드를 이해하기 어렵고 리팩토링하기 불안정하고 어렵게 만드는 경향이 있습니다. 

이 방법 대신에 가장 좋은 해결책은 각 Square의 상태 대신에 Board 컴포넌트의 상태를 저장하는 것입니다. 그리고 Board 컴포넌트는 각 사각형에 이전에 인덱스를 표시한 방법과 같이 표시할 사각영을 각 Square에 알릴 수 있습니다.

**여러 자식들로부터 데이터를 모으기 원하거나 두 자식 컴포넌트들이 서로 통신하기 원할 때 부모 컴포넌트안에 있는 상태가 되도록 상태를 위로 이동시키세요. 부모 컴포넌트는 props를 통하여 자식 컴포넌트에 상태를 돌려졸 수 있습니다. 그러면 자식 컴포넌트들은 항상 서로서로 혹은 부모 컴포넌트와 동기할 수 있습니다.**

이와 같이 상태를 위로 끌어울리는 것은 React 컴포넌트들을 리팩토링할 때 사용하는 가장 흔한 방법입니다. 이 기회를 통해 시도해봅시다. Board에 생성자를 추가하고 9개의 사각형과 일치하는 9개의 null을 가진 배열을 포함한 state로 초기화하세요. 

```Js
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null);
    };
  }
  
  renderSquare(i) {
    return <Square value={i} />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

우리는 다음과 같이 보드가 보이도록 채울 것입니다.

```js
[
  'O', null, 'X',
  'X', 'X', 'O',
  'O', null, null,
]
```

Board의 `renderSquare` 메서드는 현재 다음과 같습니다.

```Js
renderSquare(i) {
    return <Square value={i} />;
}
```

Square에 `value` prop를 전달하도록 수정하세요.

```js
renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
}
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/gWWQPY?editors=0010)에서 볼 수 있습니다.

지금 우리는 사각형이 클릭될 때 일어날 일을 변경해야 합니다. Board 컴포넌트는 지금 어떤 사각형이 채워지는지 저장하고 있습니다. 이는 Square에 Board의 상태로 업데이트하기 위한 방법이 필요함을 의미합니다. 컴포넌트 상태가 각자 고려되기 때문에 Board의 상태가 직접 Square로부터 업데이트할 수 없습니다.

여기의 보통의 패턴은 Board로부터 Square로 사각형이 클릭될 때 호출되는 함수를 전달하는 것입니다. Board 안의 `renderSquare`를 다시 변경하여 다음과 같이 읽습니다.

```Js
renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }
```

가독성을 위해 반환된 요소들을 여러 줄로 쪼개고 괄호를 추가하여 JavaScript가 세미콜론을 붙이지 않고 코드를 끝내도록 했습니다.

Boare에서 Square로  `value`와 `onClick` 두 개의 props를 전달할 것입니다. 후자는 Square이 호출될 수 있는 함수입니다. Suqare를 변경하기 위해 다음 방법들을 따르세요.

- Square의 `rennder`에 있는 `this.state.value` 를 `this.props.value`로 변경하세요.
- Square의 `rennder`에 있는 `this.setState()` 를 `this.props.onClick()`로 변경하세요.
- 더이상 상태를 가지지 않도록 Square에 정의한 `constructor`를 삭제하세요.

```Js
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={() => this.props.onClick()}>
        {this.props.value}
      </button>
    );
  }
}
```

지금 사각형을 클릭하면 Board부터 전달되는 `onClick`함수를 호출합니다. 여기에서 일어나는 일을 상기해봅시다.

1. DOM `<button>` 컴포넌트에 내장된 `onClick` prop는 React에게 클릭 이벤트 리스터를 설정하도록 명령합니다.
2. 버튼이 클릭될 때 React는 Square의 `render()`  메서드 안에 정의된 `onClick` 이벤트 핸들러를 호출할 것입니다.
3. 이 이벤트 핸들러는 `this.props.onClick()`을 호출합니다. Suqare의 props는 Board로 부터 명시됩니다.
4. Board는 `onClick={() => this.handleClick(i)}`를 Square에 전달하고 호출될 때 Board의 `this.handleClick(i)`가 동작합니다.
5. Board에 있는 `handleClick()` 메서드는 아직 정의되지 않았으므로 코드는 오류가 납니다.

DOM `<button>` 요소의 `onClick` 속성이 React에서 특별한 의미를 가진다는 것을 유념해주세요. 하지만 Square의 `onClick` prop나 Board의 `handleClick` 메서는 다릅니다. React 애플리케이션에서는 속성을 위해  `on*` 이름을 사용하고 핸들러 메서드를 위해 `handle*`를 사용하여 처리하는 것이 일반적입니다.

사각형을 클릭해봅시다. `handleClick`을 아직 정의하지 않았기 때문에 에러를 보게됩니다. Board 클래스에 추가해봅니다.

```Js
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }
  
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }

  renderSquare(i) {
    return (
       <Square
         value={this.state.squares[i]}
         onClick={() => this.handleClick(i)}
        />
    );
  }
  
  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}{this.renderSquare(1)}{this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}{this.renderSquare(4)}{this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}{this.renderSquare(7)}{this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/ybbQJX?editors=0010)에서 볼 수 있습니다.

이미 있는 배열을 변형시키는 대신에 `squares` 배열을 카피하기 위해 `.slice()`를 호출합니다. 왜 immutability가 중요한지 배우고 싶다면 [이 섹션](https://reactjs.org/tutorial/tutorial.html#why-immutability-is-important)으로 이동하세요.

다시 사각형들을 채우기 위해 사각형을 클릭할 수 있어야 하지만 각 Square 대신에 Board 컴포넌트에 상태가 저장되어야 합니다. 이는 게임을 계속 구현하도록 해줍니다. 언제 Board의 상태가 어떻게 변화하는지 유념해주세요. Square 컴포넌트들은 자동적으로 랜더링됩니다.

Square은 더 이상 각자 상태를 유지하지 않습니다. 이들은 부모 Board 컴포넌트로부터 데이터를 전달받고 클릭될 때 부모에게 알립니다. **제어된 컴포넌트들**처럼 컴포넌트들을 호출합니다.



### 왜 immutability가 중요한가요

전의 예시 코드에서 존재하는 배열을 변경하지 않고 변화를 주기 이전에 `squares` 배열을 카피하기 위해  `.slice()` 연산자를 사용하도록 하였습니다. 이것이 무엇을 의미하고 왜 이 컨셉이 중요한지 얘기해봅시다.

#### mutation을 사용한 데이터 변화

```Js
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

#### mutation을 사용하지 않은 데이터 변화

```Js
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanges, but newPlayer is  {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var new Player = {...player, score: 2};
```

mutation을 하지 않더라도(또는 기본 데이터를 변경하여도) 결과적으로는 동일하지만 직접적으로 컴포넌트 및 전체 애플리케이션 성능을 향상시키는데 도움이 되는 장점이 있습니다.

#### 쉽게 Undo/Redo과 시간 여행하기

immutability은 복잡한 기능들을 훨씬 더 쉽게 구현할 수 있게 해줍니다. 예를 들어 이 튜토리얼보다 더 나아가 우리는 게임의 다른 스테이지 사이에 시간 여행을 구현할 것입니다. 데이터 변경을 피하는 것은 우리가 데이터의 이전 버전을 계속 참고할 수 있게 해주고 우리가 필요할 때 전환할 수 있게 해줍니다.

#### 변화 트래킹하기

만약 변화하는 객체가 변경될 수도 있다고 결정하는 것은 직접적으로 변화가 객체로 만들어지기 때문에 복잡합니다. 그러면 이는 이전버전을 카피하기 위해 전체 객체 트리를 현재 버전과 비교해야 합니다. 그리고 각 변수화 값들을 비교해야 합니다. 이 과정은 점점 복잡해질 수 있습니다.

불변하는 객체가 변화된다고 결정하는 것은 쉽게 고려될 수 있습니다. 만약 객체가 이전과 차이점이 참조되고 있다면 객체는 변경됩니다. 그게 답니다.

#### React에서 언제 다시 랜더링할지 결정하기

React에서 불변성의 가장 큰 장점은 간단한 순수 컴포넌트들이 빌드될 때입니다. 불변하는 데이터들이 더 쉽게 결정될 수 있기 때문에 변경들이 만들어지더라도 컴포넌트들이 다시 랜더링되어야 할 때를 결정하기 쉽게 도와줍니다.

`shouldCompoenenUpdate()` 에 대해 더 배우고 싶고 어떻게 순수 컴포넌트들이 최적화된 퍼포먼스를 찾을 수 있는지 배우기 위해서 [이 글](https://reactjs.org/docs/optimizing-performance.html#examples)을 보세요.



### 함수적인 컴포넌트

생성자를 지웠었다. 그리고 사실 React는 `render` 메서드만을 구성하는 Sqaure과 같은 컴포넌트 타입을 위한 함수적인 컴포넌트가 불리는 더 간단한 문법을 지원한다. `React.Component`를 확장한 클래스를 정의하는 것보다 간단하게 props를 가져오고 랜더링 해야할 것을 리턴하는 함수를 작성하는 것이 낫다.

이 함수를 가지는 전체 Square 클래스로 대체하세요.

```Js
class Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}      
    </button>
  );
}
```

여기에 나타난 `this.props ` 둘 다를 `props`로 바꾸어야 할 것이다. 애플리케이션에 있는 많은 컴포넌트들이 함수적인 컴포넌트로 구현될 수 있다. 이 컴포넌트들은 더 쉽게 쓰여지고 React는 나중에 더 효율적으로 최적화할것이다.

코드를 깔끔하게 하면서 `onClick={() => props.onClick()}`을 `onClick={porps.onClick}`으로 바꿨다. 함수를 전달하는 것은 이 코드로 충분하다. `onClick={props.onClick()}`는 `props.onClick`을 호출하기 때문에 동작하지 않을 것을 유념해라.

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/QvvJOv?editors=0010)에서 볼 수 있습니다.



### 변화 가져오기

우리의 게임에서 명백한 단점은 오로지 X만 플레이될 수 있다는 것이다. 고쳐보자.

기본적으로 첫 번째 이동을 'X'가 되도록 설정해봅시다. Board 생성자에서 시작 상태를 수정해봅시다.

```Js
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }
```

움직일 때 마다 `xIsNext`는 불린 값으로 바뀌어야하고 상태가 저장되어야 한다. Board의 `handleClick` 함수를 `xIsNext` 값이 바뀌도록 업데이트해봅시다.

```js
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNExt ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

이제 X와 O가 순서대로 사용됩니다. 다음에 무엇이 뜰지 보여주도록 다음 Board의 `render`에서 "status" 텍스트를 바꾸어봅시다. 

```Js
  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      // the rest has not changed
```

바꾼 후에는 다음과 같은 Board 컴포넌트가 있어야 합니다.

```js
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/KmmrBy?editors=0010)에 있습니다.



### 승자 선언하기

게임에서 언제 승리하는지 보여줍시다. 파일 맨 하단에 헬퍼 함수를 추가해주세요.

```js
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

Board의 `render` 함수에서 누가 게임에서 이겼는지를 확인하도록 호출할 수 있고, 누군가 이겼을 때 "Winner: [X/O]" 상태 텍스트를 만들 수 있습니다.

이 코드에서 Board의 `render`에서  `status` 선언을 대체해봅시다.

```Js
  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      // the rest has not changed
```

Board에서 `handleClick`를 빨리 리턴하도록 바꿀 수 있습니다. 그리고 누군가 이미 이긴 게임에서 클릭하는 경우를 무시하고 이미 칠해진 사각형을 무시할 수 있도록 바꿀 수 있습니다.

```Js
 handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

축하합니다! 이제 틱택톡 게임을 완성했습니다. React의 기본을 알았습니다. 진짜 승자는 당신입니다.

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/LyyXgK?editors=0010)에서 볼 수 있습니다.



## 히스토리 저장하기

보드의 예전 상태를 다시 방문할 수 있도록 하여 이전 움직임 이후의 모습을 볼 수 있게 만들어봅시다. 각 움직임이 만들어질때마다 이미 새 `squares` 배열을 만들었씁니다. 이는 과거 보드 상태를 동시에 쉽게 저장할 수 있음을 의미합니다.

상태에서 이것과 같은 객체를 저장하도록 계획해봅시다.

```Js
history = [
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null,
    ]
  },
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, null,
    ]
  },
  // ...
]
```

우리는 상위 레벨 게임 컴포넌트가 움직임 리스트에 보이는 것이 가능하길 원합니다. 그래서 우리는 Square을 Board로 끌어당겼습니다. 이제 다시 Board에서 Game으로 끌어당겨 탑 레벨에서 필요한 모든 정보를 저장해봅시다.

첫 번째로 생성자를 추가하여 Game의 초기 상태를 설정해봅시다.

```Js
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```

그 후 Board를 바꾸어 props를 거쳐 `squares`를 가져오고, 이전에 Square에서 만든 변경과 같이 Game에서 지정한 `onClick` prop를 가집니다. 각 사각형의 위치를 클릭 핸들러로 전달하여 어떤 사각형이 클릭되었는지 알 수 있습니다. 여기 필요한 단계 리스트가 있습니다.

- Board의 `constructor`를 삭제하세요.
- Board의 `renderSquare`에 있는 `this.props.squares[i]`를 `this.state.sqaures[i]`로 대체하세요.
- Board의 `renderSquare`에 있는 `this.handleClick(i)`를 `this.props.onClick(i)`로 대체하세요.

전체 Board 컴포넌트는 아래와 같습니다.

```js
class Board extends React.Component {
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

Game의 `render`는 가장 최신의 히스토리 전체를 보여줘야하고 게임 상태를 계산하여 가져올 수 있습니다.

```Js
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />

        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
```

Game이 상태를 랜더링하고 있기 때문에 `<div className='status'>{status}</div>`를 지울 수 있고 Board의 `render` 함수로부터 상태를 계산하는 코드를 지울 수 있습니다.

```js
  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
```

그 다음 Board에서 Game으로 `handleClick` 메서드를 옮겨야합니다. Board 클래스로부터 잘라내어 Game클래스로 붙여넣기 할 수 있습니다.

Game 상태는 다르게 구성되어있기 때문에 약간 변경해야 합니다. Game의 `handleClick`은 새로운 히스토리 항목을 연결하여 새로운 배열을 작성하여 스택에 푸시할 수 있습니다.

```js
  handleClick(i) {
    const history = this.state.history;
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      xIsNext: !this.state.xIsNext,
    });
  }
```

이 지점에서 Board는 `renderSquare`와 `render`만 필요합니다. 상태 초기화와 클릭 핸들러는 둘 다 Game에서 동작합니다.

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/EmmOqJ?editors=0010)에서 볼 수 있습니다.



### 움직임 보여주기

지금까지 게임에서 만든 움직임을 보여줍시다. 우리는 이전에 React 컴포넌트가 첫 클래스로 JS 객체이고 우리는 그것들을 통해 저장하고 전달할 수 있다고 배웠습니다. React에서 여러 아이템들을 랜더링하기 위해 우리는 React 요소의 배열을 전달했습니다. 배열을 빌드하는 가장 흔한 방법은 데이터 배열에서 map을 하는 것입니다. Game의 `render` 메서드에서 그것을 해봅시다.

```js
 render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/EmmGEa?editors=0010)에서 볼 수 있습니다.

히스토리의 각 단계에서 `<button>`이 있는 리스트 아이템 `<li>`을 만들었다. 우리가 곧 구현할 클릭 핸들러를 가지고 있다. 이 코드에서 다음과 같은 경고 메시지와 함께 게임에서 만들어지는 움직임의 리스트를 봐야한다. 

> 경고: 배열이나 이터레이터에 있는 각 자식은 유니크한 "key" prop을 가져야한다. "Game"의 render 메서드를 확인해보세요.

경고의 의미가 무엇인지 얘기해보자.



### Keys

아이템의 리스트를 랜더링할때 React는 항상 리스트에 있는 각 아이템에 대한 정보를 저장한다. 만약 상태를 가진 컴포넌트를 랜더링한다면 컴포넌트가 어떻게 실행되는지와 관계없이 상태는 저장되어야하고 React는 네이티브 뷰의 뒤에 참고할 것을 저장한다.

리스트를 업데이트할때, React는 무엇을 바꿀지 결정해야 한다. 리스트에 아이템들을 추가하고 지우고 재배열하고 수정할 수 있었다.

이 코드를 아래의 코드로 변형하는걸 상상해보자.

```js
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

```js
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

사람의 눈에는 Alexa와 Ben의 자리가 바뀌고 Claudia가 추가된 것처럼 보인다. 하지만 React는 단순히 컴퓨터 프로그램이고 당신의 의도를 알지 못합니다. 결과적으로 React는 리스트의 각 요소에서 *key* 속성을 지정하기를 요청합니다. 문자열은 형제로부터 각 컴포넌트들을 구분합니다. 이 경우에 `alexa`, `ben`, `claudia`는 분별할 수 있는 키가 될 것입니다. 만약 아이템들이 데이터베이스의 객체와 일치한다면 데이터베이스 ID을 선택하는 것이 좋을 것입니다.

```js
<li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
```

`key`는 React에 의해 제공된 특별한 속성입니다. (`ref`을 따라 더 확장된 기능) 요소가 만들어질때 React는 `key` 속성을 가져오고 리턴된 요소에 직접적으로 key를 저장합니다. key가 props의 한 부분으로 보일지라도 이것은 `this.props.key`를 이용해 참고할 수 없습니다. React는 자동적으로 어떤 자식이 수정되어야할지 결정하는 동안 key를 사용합니다. 컴포넌트가 자신의 키를 물어볼 방법은 없습니다.

리스트가 랜더링될 때 React는 새로운 버전의 각 요소들을 가져오고 이전 리스트에서 매칭되는 키를 가진 것을 찾습니다. key가 세트에 추가될 때 컴포넌트는 만들어집니다.  키가 삭제될때 컴포넌트는 소멸됩니다. 키들은 React가 각 요소를 구분하기를 요청하여 다시 랜더링하는 것을 무시하고 상태를 유지할 수 있게 합니다. 만약 컴포넌트의 키를 바꾼다면 완전히 지워지고 새로운 상태로 생성될 것입니다.

**동적으로 리스트를 빌드할때마다 적절한 키를 할당하는 것을 강력 추천합니다.** 만약 적절한 키를 가지지 못한다면 이를 수행할 수 있도록 데이터를 재구성하여야 할지도 모릅니다.

특정한 키를 구분하지 못한다면 React는 경고를 주고 배열 인덱스를 키로 사용합니다. 이는 올바른 선택이 아닙니다. 만약 너가 리스트에 있는 요소들을 정렬하거나 리스트에 있는 버튼을 통해 지우거나 추가할때. 명시적으로 `key={i}`를 전달하는건 경고를 보여주지는 않지만 같은 문제를 발생시켜 대부분의 경우에 추천되지 않습니다.

컴포넌트의 키가 전체적으로 유니크할 필요는 없지만 관련있는 형제들 사이에서는 유니크해야 합니다.



##시간 여행 실행하기

움직임 리스트를 위해 우리는 각 스텝에서 유니크 ID를 가졌습니다. 발생할 때 마다 움직임의 숫자. Game의 `render` 메서드에서 키는 `<li key={move}`로 가지고 경고는 사라집니다.

```js
 const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/PmmXRE?editors=0010)에서 볼 수 있습니다.

`junmTo`가 정의되지 않았기 때문에 어떤 움직임 버튼을 클릭하면 에러가 뜹니다. 우리가 현재 보고 있는 스텝이 무엇인디 알려주기 위해 Game 상태에 새로운 키를 추가해봅시다.

처음으로 `stepNumber: 0`를 Game의 `constructor`의 초기 상태로 추가해봅시다.

```js
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    };
  }
```

다음으로 우리는 각 상태를 수정하기 위해 Game의 `jumpTo` 메서드를 정의할 것입니다. 또한 `xIsNext`를 수정할 것입니다. 만약 움직임의 숫자 인덱스가 짝수라면 `xIsNext`를 true로 설정합니다.

Game 클래스에`jumpTo`라 불리는 메서드를 추가합니다.

```js
handleClick(i) {
    // this method has not changed
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // this method has not changed
  }
```

Game `handleClick`에 상태를 수정하기 위해 `stempNumber:history.length`를 추가함으로 새로운 움직임이 만들어질때 `stepNumber`는 업데이트 됩니다. 현재의 보드 상태를 읽을 때 `handleClick`이 `stepNumber`라고 깨달아 새로운 요소를 만들기 위해 클릭하는 시간대로 코드가 돌아가게 할 수 있습니다.

```Js
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

이제 히스토리의 각 스텝으로부터 읽기 위해 Game의 `render`를 수정할 수 있습니다.

```js
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // the rest has not changed
```

현재의 코드는 [이곳](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)에서 볼 수 있습니다.

이제 어떤 움직임 버튼을 클릭하든 보드는 즉시 그 시간대 보여지는 게임으로 업데이트될 것입니다.



### 마무리

틱택톡 게임을 완성했습니다.

- 틱택톡 게임을 플레이해보세요.
- 한 명의 플레이어가 게임에서 이길때를 나타내줍니다.
- 게임이 진행되는 동안 움직임 기록이 저장됩니다.
- 게임 보드의 에전 버전을 보여주기 위해 시간을 되돌리는 것을 할 수 있습니다.

잘 동작하네요! 지금 React가 어떻게 작동하는지 관해 잘 알게되었다고 느끼면 좋겠습니다.

최종 결과물은 [여기](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)에서 확인하세요.

시간이 여유 있거나 새로운 스킬들을 연습해보고 싶다면 성장을 위해 해볼 수 있는 몇 가지 아이디어가 있습니다. 점점 더 어려워지게 배치되어있습니다.

1. 움직임 리스트에서 (col, row) 형태에 각 움직임 위치를 표시하세요.
2. 움직임 리스트의 선택된 아이템을 볼드처리하세요.
3. 하드 코딩한 것들 대신 사각형을 두 개의 루프를 사용하여 Board를 다시 작성하세요.
4. 오름차순 혹은 내림차순 뭐든지 움직임을 정렬하는 버튼을 추가해보세요.
5. 누군가 이겼을 때 무엇 때문에 이겼는지 세 개의 사각형을 하이라이트하세요.

가이드가 진행되는 동안 우리는 요소, 컴포넌트, props, 상태를 포함한 React의 수많은 컨셉들을 다뤘습니다. 각 주제에 대한 깊은 설명을 원한다면 [남은 문서](https://reactjs.org/docs/hello-world.html)를 확인하세요. 컴포넌트 정의에 대해 더 많이 배우고 싶다면 [이 문서](https://reactjs.org/docs/react-component.html)를 확인하세요.