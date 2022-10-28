# Top 5 React Hook Mistakes Beginners Make
## 1. Using State when you don't need it - 불필요한 state사용
익숙한 로그인 창을 생각해봅시다.
input에 입력하는 이메일이나 비밀번호 값이 변경될 때마다 매번 체크를 해야하는 상황은 아닙니다. 대신 제출하기(submit)버튼이 클릭될 때에만 해당 값을 확인하면 되는데요, 대부분 이메일이나 비밀번호 input의 value로 state값을 넣습니다. 그리고 onChange로 state값에 변화가 있을 때마다 변경하게 되는데, 이런 경우 매 typing마다 렌더링 되는 문제가 발생합니다.
```javascript
// Don't! (X)
const [email, setEmail] = useState("")
const [password, setPassword] = useState("")

<form onSubmit={onSubmit}>
    <label htmlFor="email">Email</label>
    <input id="email" type="email" value={email} />
    <label htmlFor="password">Password</label>
    <input id="password" type="password" value={password} />
    <button type="submit">Submit</button>
</form>
```

```javascript
// Do! (O)
const emailRef = useRef();
const passwordRef = useRef();

<form onSubmit={onSubmit}>
    <label htmlFor="email">Email</label>
    <input id="email" type="email" ref={emailRef} />
    <label htmlFor="password">Password</label>
    <input id="password" type="password" ref={passwordRef} />
    <button type="submit">Submit</button>
</form>
```
#### 1번 해결: state 대신 ref를 쓰세요.
그리고 onSubmit이 일어날 때 email, password 값에 email: emailRef.current.value, password: passwordRef.current.value
이런식으로 값을 넣어주세요.

## 2. Not using the function version of useState - setState을 함수로 사용하지 않는 것
```javascript
const addCount = (amount) => {
    setCount(count+amount)
    setCount(count+amount)
}
```
위와 같이 count에 number를 더한 값을 setCount에 바로 넣는다고 해서 counter의 숫자가 달라지거나 하지 않습니다. 하지만 리액트의 setState은 버튼을 클릭하는 것처럼 해당 이벤트가 발생하는 그 순간에 모두 즉시 이뤄지는 것은 아닙니다. 비동기적으로 작동합니다. 때문에 setState에는 약간의 시간차가 있고 그 사이에 state변화가 여러가지가 있다면 모아서 한 번에 처리합니다. 이런 이유때문에 setState을 사용할 때에는 정말 특별한 케이스가 아닌 경우에는 기존의 값을 기준으로 어떻게 변경한 모습을 return할 지 함수형으로 수정해줘야 합니다.

```javascript
const addCount = (amount) => {
    setCount(prev => prev+amount)
}
```

## 3. Unnecessary useEffect - 불필요한 useEffect
```javascript
// Don't! (X)
const [firstName, setFirstName] = useState("")
const [lasttName, setLastName] = useState("")
const [fullName, setFullName] = useState("")

useEffect(()=>{
    setFullName(`${firstName} ${lastName}`)
}, [firstName, lastName])

return (
    <>
        <input value={firstName}... />
        <input value={lastName}... />
        {fullName}
    </>
)
```
위와 같이 작성해도 우리가 원하는 대로 동작을 하기는 합니다. 하지만 firstName과 lastName이 변경될 때마다 whole App이 모두 rerendering되기 때문에 불필요한 렌더링이 많아 최적화가 되지 않는 문제가 있습니다.

#### 해결. useEffect는 꼭 필요한 경우가 아니라면 사용하지 않고 state의 변경에 따라 변화하는 변수만을 사용해도 충분히 표현할 수 있다.

```javascript
// Do! (O)
const [firstName, setFirstName] = useState("")
const [lasttName, setLastName] = useState("")

const fullName = `${firstName} ${lastName}`

return (
    <>
        <input value={firstName}... />
        <input value={lastName}... />
        {fullName}
    </>
)
```

## 4. Referential Equality mistakes
```javascript
// Don't! (X)
const [firstName, setFirstName] = useState("")
const [lasttName, setLastName] = useState("")
const [darkMode, setDarkMode] = useState(false)

const person = {name, age}
useEffect(()=>{
    console.log(person)
}, [person])

return (
    <>
        <input value={firstName}... />
        <input value={lastName}... />
        {fullName}
    </>
)
```
위 코드와 같은 상황에서 만약 다크모드 토글 버튼을 클릭을 한다면 로그가 찍힐까? 아니면 안찍힐까?
찍힙니다.
이것은 referential Equality와 연관되어 있는데요, 객체는 원시타입(primitive type)이 아니라 참조타입입니다. 이 참조타입은 render 될 때마다 변경이 됩니다. JS에서 {} === {} 비교 연산자는 false가 나오죠.
따라서 useEffect에서는 이 person객체값과는 전혀 상관없는 다크모드 토글이 on/off되면서 rerender될 때마다 person객체의 메모리 주소가 달라지므로 계속해서 person객체가 변한다고 판단하므로 로그를 계속 찍습니다.
(https://www.youtube.com/watch?v=-hBJz2PPIVE)

#### 해결 1. useMemo로 person객체를 memoize합니다.
#### 해결 2. useEffect의 의존성 배열에 person.name 혹은 person.age 등 정확한 값을 지정합니다.

## 5. Not aborting fetch requests - fetch 취소를 컨트롤하지 않는다.

```javascript
export function useFetch(url {
    const [loading, setLoading] = useState(true)
    const [data, setData] = useState()
    const [error, setError] = useState()
    
    useEffect(()=>{
        setLoading(true)
        fetch(url)
        .then(setData)
        .catch(setError)
        .finally(()=> setLoading(false))
    }, [url])
)
```
위와 같이 fetch를 받아오는 useFetch custom hook이 있다고 가정해보자.


---
reference
https://youtu.be/GGo3MVBFr1A