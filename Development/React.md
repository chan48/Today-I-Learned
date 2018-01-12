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

