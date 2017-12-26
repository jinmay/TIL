# React basic

아주 기초적인 React에 대해서 알아보자

1. What is React?

Facebook에서 만든 User Interface Library이다. Framework가 아닌 Library이며 혼동하지 말자. 주요 장점으로는 컴포넌트 단위로 재사용가능한 UI template을 만들 수 있다는 것이다. 이는 리액트만의 Virtual DOM을 이용하여 상태(state)가 변경되었을 때만 UI를 선택적으로 랜더링함으로써 최소한의 DOM 처리로 컴포넌트를 업데이트 할 수 있다는 장점이 있다.

2. props

부모 컴포넌트는 하위 컴포넌트로 데이터를 줄때 props를 이용할 수 있다. 

~~~react
const sample = [
  'first',
  'second'
]

class App extends Component {
  render() {
    return (
      <div className="App">
        <ChildComponent number={ sample[0] } /> 
        <ChildComponent number={ sample[1] } />
      </div> 
    );
  }
}

class ChildComponent extends Component {
  render() {
    return (
      <p>{ this.props.number }</p>
    );
  }
}
~~~

리액트의 데이터 흐름은 단방향으로써 상위 컴포넌트에서 하위 컴포넌트의 방향으로 많이 사용하는 거 같다.

3. LIfe-Cycle

컴포넌트가 생성될때 다음과 같은 라이프사이클을 갖는다.

**componentWillMount() -> render() -> componentDidMount()**

메소드 이름부터 굉장히 직관적임을 알 수 있다.

또한 컴포넌트가 업데이트 될때( = state가 변할때) 아래와 같은 순서의 라이프사이클을 갖는다.

**componentWillReceiveProps() -> shouldComponentUpdate() -> componentWillUpdate() -> render() -> componentDidUpdate()**

4. state

* state는 컴포넌트 안에서 상태의 변화를 감지하는 object이다.

주요 역할로는 state가 변할때마다 컴포넌트를 다시 render()하게 만드는 것이다.

~~~react
class App extends Component {
  state = {
    test: "hello world"
  }

  render() {
    return (
      <div className="App">
		{ this.state.test }        
      </div> 
    );
  }
}
~~~

위와 같이 state를 만들고

~~~react
class App extends Component {
  state = {
    test: "hello world"
  }

  componentDidMount() {
    setTimeout(() => {
      this.setState({
        greeting: 'hello again@@'
      })
    }, 2000)
  }

  render() {
    return (
      <div className="App">
		{ this.state.test }        
      </div> 
    );
  }
}
~~~

componentDidMount()를 이용하여 state의 값을 바꾸려고한다.

* **this.setState**말고 직접적으로 state를 변경하는 방법이 있는데 사용하지 않기를 권하고 있다

~~~react
componentDidMount() {
    setTimeout(() => {
      this.state.greeting: 'hello world again@@'
    }, 2000)
  }
~~~

위 와 같이 state를 직접적으로 변경할 수 있지만 브라우저의 콘솔을 확인해 본다면 **state를 직접적으로 변경하지 말라는 경고를 볼 수 있다**

<br>

~~~react
class App extends Component {
  state = {
    test_list: [
      'first',
      'second'      
    ]
  }  

componentDidMount() {
  setTimeout(() => {
    this.setState({
      test_list: [
        ...this.state.test_list,
        'third'
      ]
    })
  }, 2000);
}
  
  render() {
    return (
      <div className="App">
        { this.state.test_list.map(obj => {
          return <p>{ obj }</p>
        })}
      </div> 
    );
  }
}
~~~

componentDidMount()를 이용하여 state를 변경하고 다시 render()하는 코드이다.

위에서 주목해야 할 부분은 **...this.state.test_list**이다. 현재 state안에는 이미 코드가 있기때문에 뒤에 추가하기 위해서 사용했다.



* stateless component

state가 없는 - props - 만 있어도 되는 컴포넌트가 있다. 마찬가지로 Lifecycle도 존재하지 않는다.

단순히 리턴하기만을 위해 존재한다. 

~~~react
class MoviePoster extends Component {
  render() {
    return (
    	<img src={ this.props.poster } />
    )
  }
}

---------------------------------------------------------------------

function MoviePoster({poster}) {
    return (
        <img src={ poster } />
    )
}
~~~

위의 두 코드는 동일하게 작동한다.

