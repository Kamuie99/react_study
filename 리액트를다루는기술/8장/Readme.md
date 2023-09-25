# 8. Hooks

### useState

- 가장 기본적인 Hook이며, 함수 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해줌
- useState 함수의 파라미터에는 상태의 기본 값을 넣어준다.
- 함수가 호출되면 배열을 반환, 첫 번째 원소는 상태 값, 두번째는 상태를 설정하는 함수

```jsx
import { useState } from "react";

const Counter = () => {
  const [value, setValue] = useState(0);
  return (
    <div>
      <p>
        현재 카운터 값은 <b>{value}</b>입니다.
      </p>
      <button onClick={() => setValue(value + 1)}> +1 </button>
      <button onClick={() => setValue(value - 1)}> -1 </button>
    </div>
  );
};
export default Counter;
```

**useState 여러 번 사용하기**

- 하나의 useState 함수는 하나의 상태 값만 관리할 수 있음

> 컴포넌트에서 관리해야 할 상태가 여러 개라면 useState를 여러번 사용하면 됨
> 

### useEffect

- 리액트 컴포넌트가 렌더링될 때 마다 특정 작업을 수행하도록 설정할 수 있는 Hook
- 추후의 호환성을 위해 useEffect는 개발 환경에서 컴포넌트가 화면에 나타날 때 두 번 호출된다.

**마운트될 때만 실행하고 싶을 때**

- 화면에 맨 처음 레더링 될때만 실행하고, 업데이트될 때는 실행하지 않으려면

> 함수의 두 번째 파라미터로 비어 있는 배열을 넣어 주면 된다.
> 

```jsx
useEffect(() => {
	console.log('마운트될 때만 실행됩니다.');
}, []);
```

**특정 값이 업데이트될 때만 실행하고 싶을 때**

- useEffect의 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어 준다.
- 배열 안에는 useState를 통해 관리하고 있는 상태를 넣어 줘도 되고, props로 전달받은 값을 넣어 줘도 된다.

```jsx
useEffect(() => {
	console.log(name);
	}, [name]);
```

**뒷정리하기**

- useEffect는 렌더링되고 난 직후마다 실행된다.
- 두 번째 파라미터 배열에 무엇을 넣는지에따라 실행 조건 달라진다.
- 컴포넌트가 언마운트되기 전이나 업데이트 직전 어떠한 작업을 수행하고 싶다면

> useEffect에서 뒷정리(cleanup)함수를 반환해 주어야 한다.
> 

```jsx
useEffect (() => {
	console.log('effect');
	console.log(name);
	return () => {
		console.log('claenup');
		console.log(name);
	};
}, [name]);
```

> 언마운트될 때만 뒷정리 함수를 호출하려면 useEffect 함수의 두 번째 파라미터에 비어있는 배열을 넣는다.
>
