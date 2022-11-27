# Constate

> 리액트 훅을 이용해 로컬 state을 사용하고, 그것을 React Context로 손쉽게 사용할 수 있게 도와주는 라이브러리

## Basic Example
```javascript
import React, { useState } from "react";
import constate from "constate";

// 1️⃣ Create a custom hook as usual
function useCounter() {
  const [count, setCount] = useState(0);
  const increment = () => setCount(prevCount => prevCount + 1);
  return { count, increment };
}

// 2️⃣ Wrap your hook with the constate factory
const [CounterProvider, useCounterContext] = constate(useCounter);

function Button() {
  // 3️⃣ Use context instead of custom hook
  const { increment } = useCounterContext();
  return <button onClick={increment}>+</button>;
}

function Count() {
  // 4️⃣ Use context in other components
  const { count } = useCounterContext();
  return <span>{count}</span>;
}

function App() {
  // 5️⃣ Wrap your components with Provider
  return (
    <CounterProvider>
      <Count />
      <Button />
    </CounterProvider>
  );
}
```

useCounter라는 hook을 constate으로 wrapping하면 Provider와 Context가 생성된다. 이렇게 하면 Provider가 감싸는 한도 내에서 Button과 Count 컴포넌트에서 동일한 count값과 그 count값을 update할 수 있는 setter인 increment를 쓸 수 있게 된다.
일반적인 React Context로 Provider와 Context를 만드는 것은 생각보다 보일러플레이트 코드가 많이 발생하는데, constate라이브러리가 그런 다소 귀찮을 만한 작업들을 처리해주어서 간단한 React Hook처럼 쓸 수 있게 되는 것이 이 constate라이브러리의 장점인 것으로 보인다.