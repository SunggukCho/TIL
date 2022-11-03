    # All useEffect Mistakes Every Junior React Developer Makes
## 1. 의존성 배열이 원시타입일 때와 참조타입일 때

```javascript
const [state, setState] = useState({
    name: "",
    selected: false,
})

useEffect(()=>{
    console.log('state가 변경되었습니다.')
}, [state])

const handleAdd = () => {
    sestState(prev => {...prev, name})
}
const handleSelect = () => {
    sestState(prev => {...prev, selected})
}

return (
<>
    <button onClick={handleAdd}>add</button>
    <button onClick={handleSelect}>select</button>
<>
)
```
위 예시에서 useEffect를 참조타입인 state 객체로 의존성 배열을 넣어서 state가 변경될 때마다 "state가 변경되었습니다." 라는 로그를 찍도록 했습니다. 그리고 이는 name을 추가할 때 마다 적절히 작동합니다. 하지만 이미 selected 처리가 된 state에 계속 handleSelect를 호출해보면, 계속해서 "state가 변경되었습니다."라는 로그가 찍힙니다. '어? 이미 selected는 true가 되어서 state는 계속 똑같은데 왜 계속 로그가 찍히는거지?'라는 생각이 들 수 있습니다.

이 사태가 일어나는 원인은 state라는 객체의 주소가 렌더링 될 때 마다 계속 변경되기 때문입니다. state의 객체 메모리 주소가 계속 변경되므로 useEffect는 계속해서 state가 바뀌었다고 판단하고 로그를 찍는 것입니다.
따라서 이를 해결하기 위해서는 크게 두 가지 방법이 있습니다.
1. useEffect의존성 배열에 state.name, state.selected 등 정확한 원시값을 넣어 준다.
2. useMemo를 활용해서 state.name, state.selected값이 변화할 때에만 값을 변환해주는 변수를 하나 설정한 뒤 해당 변수를 useEffect의 의존성 배열에 넣어준다.

1번은 알기 쉬우니 바로 넘어가고 2번에 대한 내용만 좀 더 추가해보겠습니다.
```javascript
const user = useMemo(()=> ({
    name: state.name,
    selected: state.selected
}, [state.name, state.selected])

useEffect(()=>{
    ...}, [user])
}
```
이처럼 수정하면 원하는 동작을 이뤄낼 수 있습니다.
의존성 배열에 객체를 넣을 때에는 예상치 못한 동작이 발생하여 컴포넌트 성능을 떨어뜨릴 수 있으므로 훨씬 신중해야 합니다.

## 2. setState 시 기존 값 수정
1초에 카운트를 하나씩 올리는 App을 만든다고 가정해봅시다.
```javascript
const [number, setNumber] = useState(0)

useEffect(()=>{
    console.log("Effect")
    setInterval(()=> {
        setNumber(number+1)
    }, 1000)
}, [number])
```
이렇게 하면 로그에 effect 옆에 384, 2132 등 로그가 엄청나게 여러번 찍힐것이며 렌더링 되는 브라우저에도 깜빡임(glich)가 보일 것입니다.
이것보다는 의존성 배열에서 number를 빼고 setNumber도 함수를 활용하여 기존 값에 +1을 하는 방식으로 아래와 같이 수정해주면 훨씬 낫다.

```javascript
const [number, setNumber] = useState(0)

useEffect(()=>{
    console.log("Effect")
    setInterval(()=> {
        setNumber(prev => prev+1)
    }, 1000)
}, [])
```
하지만 이 방식도 면밀히 살펴보면 충분치 않다. 왜냐하면 setInterval이 계속해서 실행되기 때문에 시간이 지날수록 number가 렌더링 되는 것이 예상 동작과는 달라진다.

```javascript
const [number, setNumber] = useState(0)

useEffect(()=>{
    console.log("Effect")
    const interval = setInterval(()=> {
        setNumber(prev => prev+1)
    }, 1000)
    
    return ()=>{
        clearInterval(interval)
    }
}, [])
```
따라서 위와 같이 interval을 한 번 실행하고 나면 cleanup 해주어 기존 interval을 메모리에서 제거해준 뒤 1000ms 가 지난 다음 새로운 interval만이 제대로 동작하게 된다.

## 3. fetching data
```javascript
const [posts, setPosts] = useState([])

useEffect(()=>{
    fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res => res.json())
    .then(data => {
        setPosts(data)
    })
}, [])
```
위 코드를 실행하면 웬만한 상황에서는 문제없이 working하는 걸로 보입니다. 하지만 네트워크를 3G slow로 해놓고서 해당 fetch가 끝나기 전에 뒤로가기를 눌러 컴포넌트 렌더링이 진행되지 않은 상태에서 render를 끝내려고 해봅시다. 뒤로 가기를 눌렀음에도 불구하고 fetching이 계속 되고 결과적으로 memory leak이 발생합니다.
따라서 이를 해결하기 위해서 cleanup이 필요합니다.

```javascript
const [posts, setPosts] = useState([])

useEffect(()=>{
    let isCancelled = false
    fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res => res.json())
    .then(data => {
        if (!isCancelled) {
            setPosts(data)
        }
    })
    return ()=>{
        isCancelled = true
    }
}, [])
```

출처: https://youtu.be/QQYeipc_cik