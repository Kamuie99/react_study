# 3. 컴포넌트

### 컴포넌트

- 컴포넌트를 선언하는 방식은 두 가지이다.
- 하나는 함수 컴포넌트이고, 또 다른 하나는 클래스형 컴포넌트이다.

**함수 컴포넌트**

- render 함수가 꼭 있어야하고, 그 안에서 보여줘야 할 JSX를 반환해야 한다.
- 선언하기가 훨씬 편합니다. 메모리 자원도 클래스형보다 덜 사용한다.
- 단점은 state와 라이프사이클 API의 사용이 불가능하다 → Hooks 도입 후 해결

```jsx
import './App.css';

function App() {
	const name = '리액트';
	return <div className="react">{name}</div>;
}

export default App;
```

**클래스형 컴포넌트**

```jsx
import {Component} from 'react';

class App extends Component {
	render() {
		const name = "react";
		return <div className="react">{name}</div>;
	}
}

export default App;
```

<aside>
💡  클래스형 컴포넌트의 경우 이후 배울 state 기능 및 라이프 사이클 기능을 사용할 수 있는 점과 임의 메서드를 정의할 수 있다는 것이 차이점이다.

</aside>

### props

- properties를 줄인 표현인 props, 컴포넌트 속성을 설정할 때 사용하는 요소
- 해당 컴포넌트를 불러와 사용하는 부모 컴포넌트에서 설정할 수 있다.
- props 값은 컴포넌트 함수의 파라미터로 받아 와서 사용할 수 있다.
- props를 렌더링할 때JSX 내부에서 { } 기호로 감싸 주면 된다.
- 클래스형 컴포넌트에서 props를 사용할 때는 render 함수에서 this.props를 조회하면 된다.

```jsx
const MyComponent = props => {
	return <div>안녕하세요, 제 이름은  {props.name} 입니다. </div>;
};

export default MyComponent;
```

**childern**

- 컴포넌트 태그 사이의 내용을 보여 주는 props인 children

```jsx
import MyComponent from './MyComponent';

const App = () => {
	return <MyComponent>리액트<MyComponent/>;
};

export default App;
```

> 이와 같이 작성하였을 때, `{props.chilren}` 을 사용할 수 있다.
> 

### 비구조화 할당(구조분해 문법)

- props 값을 조회할 때마다 props.name, props.children과 같이 props.라는 키워드를 앞에 붙여야 함
- 이를 편하게 하기 위해 ES6의 비구조화 할당 사용

```jsx
const MyComponent = props => {
	const {name, children} = props;
	return (
		<div>
			안녕하세요, 제 이름은 {name}입니다. <br />
			children 값은 {children}입니다.
		</div>
	);
};

export default MyComponent;
```

**propTypes**

- 컴포넌트의 필수 props 지정과 props 타입(type) 지정에 사용
- propTypes 지정은 defaultProp 설정과 비슷함, import 구문을 사용해 불러와야함
- 필수 propTypes 지정에는 뒤에 isRequired를 붙여 주면 된다.
- `MyComponent.propTypes = { name: PropTypes.string };`

> 이와 같이 사용했을 대, name 값은 무조건 문자열(string) 형태로 전달해야 된다는 것을 의미한다.
> 

<aside>
💡 defaultProps와 propTypes는 필수 사항은 아니지만, 큰 규모의 프로젝트를 진행할 때 다른 개발자들과 협업한다면 어떤 props가 필요한지 쉽게 알 수 있어 개발 능률이 좋아진다.

</aside>

### state

- 컴포넌트 내부에서 바뀔 수 있는 값을 의미, 컴포넌트 사용 과정에서 부모 컴포넌트가 설정하는 값인 state
- 컴포넌트 자신은 해당 props를 읽기 전용으로만 사용 가능, props 변경은 부모 컴포넌트에서 가능
- sate는 클래스형 컴포넌트가 지니고 있는 것과 함수 컴포넌트에서 `useState`를 사용하는 두가지가 있다.
- state 객체 안에는 여러 값이 있을 수 있다.
- `this.setState` 를 사용하여 `state` 값을 업데이트할 때는 상태가 비동기적으로 업데이트 됨.
- setState를 사용하여 값을 업데이트 후 특정 작업 처리는 `setState`의 두번째 파라미터로 콜백함수 등록

useState

- 리액트 16.8 버전 이후 useState를 사용하여 함수 컴포넌트에서도 state 사용이 가능해짐
- 이 과정에서 Hooks라는 것을 사용하게 됨.
- useState 사용 방법은 배열 비구조화 할당 문법과 상당히 유사함

```jsx
const array = [1, 2];
const one = array [0];
const two = array [1];

/* 배열 비구조화 할당 */
const array = [1, 2];
const [one, two] = array;

/* useState 사용 */
const Say = () => {
	const [message, setMessage] = useState('');
	const onClickEnter = () => setMessage('안녕하세요!);
	const onClickLeave = () => setMessage('안녕히 가세요!);
```

state사용 주의사항

- state 값을 바꾸어야 할 때는 setState 혹은 useState를 통해 전달받은 세터 함수를 사용해야 한다.

> 배열이나 객체를 업데이트 할 때는 객체 사본을 만들고 그 사본에 값을 업데이트 후 그 사본의 상태를 setState 혹은 세터함수를 통해 업데이트한다.
> 
- 객체에 대한 사본을 만들 때는 spread 연산자 (…)을 사용하여 처리, 배열 사본은 배열의 내장함수 사용

<aside>
💡 props는 부모 컴포넌트가 설정, state는 컴포넌트 자체적으로 지닌 값으로 내부에서 값을 업데이트
새로운 컴포넌트를 만들 때는 useState 권장, 함수 컴포넌트와 Hooks 사용이 주요 개발 방식이 될 것

</aside>
