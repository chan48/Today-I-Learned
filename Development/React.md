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



