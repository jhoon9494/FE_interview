# React immutable

## 불변성이란?

데이터나 객체가 `메모리`에 할당된 이후 그 상태가 더 이상 변경되지 않는 것을 의미한다.

`원시타입`은 메모리의 고정 크기로 원시 값을 저장하고 해당 값을 변수가 직접적으로 가리키는 형태를 띈다.

또한, `불변성`이란 특징을 가지고 있기 때문에, 재할당이 될 때 메모리에 할당된 값을 교체하는 것이 아니라 새로운 메모리에 값을 할당하고 변수가 새로운 메모리 주소를 가리키게 된다.

```javascript
// 원시타입
let a = 1;
let b = a;

a = 2;
console.log(a, b); // 2, 1
```

위의 예제처럼 a에 2를 할당하게 되면 새로운 메모리에 2라는 Number가 할당되고 a는 그 주소를 가리키게 되지만, b는 여전히 이전 메모리 주소를 참조하여 1을 가지게 된다.

반면에 `참조타입`은 변수의 크기가 동적으로 변할 수 있기때문에, 힙이라 불리는 별도의 메모리 공간에 값을 저장하며 변수에 할당할 때 힙 메모리의 주소값이 저장된다.

또한 원시타입과 달리 내부 값이 재할당 되더라도 이전과 동일한 메모리 주소값을 가진다는 특징을 가지고 있다.

```javascript
// 참조타입
let obja = { name: 'a' };
let objb = obja;

obja.name = 'b';
console.log(obja.name, objb.name); // b b;
```

위의 예제 처럼 obja의 name 프로퍼티의 값을 b로 변경할 경우, 불변성을 갖지 않으므로 이전 주소값의 내부 값이 변경된다.

obja와 objb 모두 같은 주소값을 가진 객체를 가지고 있기 때문에, objb도 name 프로퍼티가 b로 변경된 객체를 가지게된다.

<br/>

## 리액트에서 불변성이 중요한 이유는 무엇인가요?

리액트의 상태 업데이트를 위해 불변성이 중요하다.

리액트는 상태를 업데이트할 때 `얕은 비교`를 수행한다. 즉, 배열이나 객체의 속성 하나하나를 비교하는게 아니라 `이전 주소값`과 `현재 주소값`만을 비교하여 상태 변화를 감지한다.

상태의 모든 값에 대해 깊은 비교를 수행하는 것이 아니므로 효율적인 상태 업데이트를 수행할 수 있다.

만약 리액트의 값이 불변하지 않는다면 즉, 값이 변하더라도 같은 주소 값을 가진다면 리액트가 상태를 업데이트할 때 값의 변경을 감지하지 못해서 리렌더링이 일어나지 않는다.
