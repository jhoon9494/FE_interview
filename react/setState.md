# React setState

## setState란?

React 컴포넌트의 상태를 업데이트하고, 변경된 상태에 따라 리렌더링을 트리거하는 함수이다.

클래스 컴포넌트에서는 `this.setState()` 메소드를 사용하며, 함수 컴포넌트에서는 `useState` 훅을 사용하여 반환되는 배열의 두번째 값인 setter를 사용한다.

<br />

## setState는 동기 함수? or 비동기 함수?

`setState`는 **이벤트 핸들러 내**에서 리액트 내부적으로 `비동기` 작업을 실행한다.

> 만약, `setState` 호출이 이벤트 핸들러가 아닌 컴포넌트 내에서 발생한 경우, 일반적으로 비동기 처리되지 않고 즉시 실행된다.

즉, `setState` 함수가 실행되면 즉시 상태가 변하는 것이 아니라 `비동기적`으로 상태를 업데이트하며, 업데이트된 상태를 이용하여 컴포넌트를 다시 렌더링한다.

```javascript
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
    console.log(count); // 0
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>+1</button>
    </div>
  );
}
```

위 예제 코드는 버튼을 클릭하여 count를 1씩 증가시키고 count를 출력하는 코드이다.

현재 count가 0일 때, 만약 setCount가 동기적으로 동작했더라면 count는 1이 될 것이다.

하지만, 실제로 setCount는 비동기적으로 동작하기 때문에 0이 출력되는 것이다.

### 이벤트 루프와 setState

추가적으로, `setState` 함수는 `Promise`, `setTimeout` 등 이벤트 루프를 이용한 비동기 작업과 조금 다르게 동작한다.

`setState`는 리액트 내부적으로 비동기 작업을 실행하기 때문에, 상태의 업데이트가 완료된 시점을 예측하기 어려워서 setState의 `호출 순서`를 보장할 수 없다.

반면에 `이벤트 루프`를 이용한 경우, 이벤트 루프의 비동기 처리 순서에 따라 작업이 진행된다.

그리고 콜스택이 비었을 때 비동기 처리가 완료된 작업이 실행되므로 실행 시점을 명확히 알 수 있어 `호출 순서`를 보장할 수 있다.

<br />

## setState batch

위에서 `setState`는 이벤트 핸들러 내에서 비동기적으로 실행된다고 했다.

리액트는 이벤트 핸들러 내에서 일어나는 모든 `setState`를 모아서 일괄적으로 처리한다.

즉, 하나의 이벤트 핸들러 내에서 여러 번의 `setState` 호출이 있어도 한 번의 리렌더링만 일어난다.

만약 모든 `setState` 호출이 즉시 리렌더링을 발생시키게 된다면 성능 저하의 원인이 될 수 있다.

그러므로 이러한 동작 방식은 리액트의 성능을 최적화하기 위한 것이다.
