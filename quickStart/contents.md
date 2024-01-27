## Quick Start
## JSX로 마크업 작성하기
리액트를 이용하여 스타일이나 데이터를 화면에 렌더링 할 수 있다.
```tsx
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}

```

### 조건부 렌더링
if같은 조건문이나 삼항연산자로 특정 상태에 따라서 렌더링을 정할 수 있다.
```tsx
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
// 삼항연산자
```tsx
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```
삼항연산자나 &&같은 조건문은 if문과 달리 jsx내부에서 사용 가능하다.
#### 주의사항
&&같은 논리연산자를 쓸때는 조심해야한다. 가령 A&&B는 둘중 하나의 조건이 falsy할경우에 A라는 값을 들고있고(0&&30 -> 0, 0||false -> false) 0이나 NaN같은 값이 리턴되는경우에는 해당 문자열을 화면에 렌더링한다.
```tsx
// 화면에 NaN이라는 문자열과 Propfile컴포넌트가 둘 다 렌더링 된다.
import Profile from './Profile.js';
const user = [
  {
    id: NaN,
    name: "Hedy Lamarr",
    imageUrl: "https://i.imgur.com/yXOvdOSs.jpg",
    imageSize: 90
  },
  {
    id: "Hedy Lamarr1",
    name: "Hedy Lamarr",
    imageUrl: "https://i.imgur.com/yXOvdOSs.jpg",
    imageSize: 90
  }
];

export default function App() {
  return (
    <>
      {user.map(
        (userInfo) =>
          userInfo.id && <Profile user={userInfo} key={userInfo.id} />
      )}
    </>
  );
}

```
<img width="249" alt="image" src="https://github.com/endmoseung/ReactDocsStudy/assets/103626175/306c16d5-f9c5-4504-b6c9-955c76069a0f">

다음은 자바스크립트에서 falsy하다고 여기는 값을 정리한것이다. ==연산자로 false와 비교하면 0이나 "" []같은 값들은 true로 찍힌다.
1. false: 불리언 값 false
2. 0: 숫자 0
3. -0: 음의 숫자 0
4. 0n: BigInt로 표현된 0
5. "": 빈 문자열
6. null: 값이 없음을 나타내는 특별한 값
7. undefined: 값이 할당되지 않음을 나타내는 특별한 값
8. NaN: 숫자가 아님(Not-a-Number)

### 목록 렌더링
컴포넌트 목록을 렌더링하려면 for loop 및 배열 map() 함수와 같은 JavaScript 기능을 사용해야 한다. map으로 리스트를 그릴경우 형제사이의 구분을 위해 key값을 등록해야하고 일반적으로 자료의 id를 많이 사용한다.

```tsx
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

그럼 아래 코드처럼 map으로 새로운 배열을 반환한 형태를 직접만들어주어도 똑같이 동작할까 ?
```tsx
const hi = [<div key={1}>1</div>, <div key={2}>2</div>, <div key={3}>3</div>]

  return (
      <div>{hi}</div>
)
```
정답은 Yes. 똑같이 동작한다.
<br>
그럼 만약 두번의 map반복문이 있을경우 두개에서 각각 똑같은 id값을 가져도 상관없을까 ?
```tsx
  const test = [1, 2, 3]
  const test2 = [1, 2, 3]

  return (
      <div>
        <div>
          {test.map((src) => (
            <div key={src}>{src}</div>
          ))}
        </div>
        <div>
          {test2.map((src) => (
            <div key={src}>{src}</div>
          ))}
        </div>
      </div>
)
```
우선 공식문서에서는 형제사이의 중복된 key값은 피하고 문서 전체에서는 상관없다고 적혀있다. 그러므로 상관없고 만약에 가독성을 위해서 정리해야한다면 리터럴템플릿을 사용하여 고유한 키값을 만드는걸 권장한다.

### 이벤트에 응답하기 
컴포넌트 내부에 이벤트 핸들러 함수를 선언하여 이벤트에 응답할 수 있다.
```tsx
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```
자바스크립트의 eventListner에 함수를 전달한다. <br>
onClick={handleClick()}으로 전달했을 때 alert이 두 번 뜨는 현상은 React의 strict mode를 해제해주면 된다.

### 화면 업데이트하기
컴포넌트가 특정 정보를 ``기억``하여 표시하기를 원하는 경우가 종종 있다. 예를 들어, 버튼이 클릭된 횟수를 카운트하고 싶을 수 있다. 이렇게 하려면 컴포넌트에 state를 추가하면 된다.
```tsx
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

### 데이터 공유하기
만약 자식컴포넌트에 똑같은 데이터를 공유하고 싶다면 데이터를 부모에서 자식으로 전달(props)합니다.
<img width="673" alt="image" src="https://github.com/endmoseung/ReactDocsStudy/assets/103626175/ffe9b4a6-6d4a-4079-a942-a22df75a05a5">
```tsx
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}
```
### 자바스크립트 함수에 대해
```
함수 선언 방식인 일반 함수와 화살표 함수, 그리고 콜백 함수의 차이에 대해 알아보겠습니다.

일반 함수
일반 함수는 function 키워드를 사용하여 선언합니다.
this 바인딩은 함수가 호출된 시점에 따라 결정됩니다.
arguments 객체를 사용할 수 있습니다.
함수 내부에서 자신의 this를 가리키는 arguments.callee를 사용할 수 있습니다.

화살표 함수 (Arrow Function):
화살표 함수는 =>를 사용하여 선언합니다.
this 바인딩은 화살표 함수가 정의된 곳의 바깥쪽 범위에 의해 결정됩니다.
화살표 함수는 자신만의 arguments 객체를 가지지 않습니다.
arguments.callee를 사용할 수 없습니다.

콜백 함수
콜백 함수는 다른 함수의 인자로 전달되어 나중에 호출되는 함수입니다.
이 콜백 함수는 일반 함수든 화살표 함수든 어떤 형태로든 정의될 수 있습니다.
콜백 함수의 this는 호출한 함수에 따라 결정됩니다. 따라서, 일반 함수의 경우 호출한 시점의 바인딩을 가지고, 화살표 함수의 경우 정의된 곳의 바깥쪽 범위에 의해 결정됩니다.
요약하자면, 일반 함수와 화살표 함수는 함수 선언 방식과 this 바인딩의 동작 방식에서 차이가 있습니다. 콜백 함수는 다른 함수의 인자로 전달되어 호출되는 함수이므로, 해당 함수의 선언 방식에 따라 this 바인딩이 결정됩니다.
```
#### arguments객체
arguments 객체는 일반 함수에서 사용되며, 함수 내부에서 현재 호출된 함수에 전달된 인자들을 포함하는 유사 배열 객체입니다. arguments 객체를 사용하여 함수 내에서 동적으로 인자에 접근하거나, 인자의 개수를 파악할 수 있습니다.
```tsx
// 일반 함수에서 arguments 객체 사용
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sum(1, 2, 3, 4)); // 출력: 10


// 화살표 함수에서 나머지 매개변수 사용
const sumArrow = (...numbers) => {
  let total = 0;
  for (let i = 0; i < numbers.length; i++) {
    total += numbers[i];
  }
  return total;
}

console.log(sumArrow(1, 2, 3, 4)); // 출력: 10
```
