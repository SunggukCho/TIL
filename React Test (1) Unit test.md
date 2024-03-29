# React Test (1) Unit test

## Test 환경 설정 - JEST

`CRA`를 하면 자동으로 설치해주는 jest로 시작해보자. jest는 jasmine 기반 테스팅라이브러리이다. 

이 외에도 mocha, Karma 등 다양한 [테스팅 라이브러리간 비교](https://medium.com/welldone-software/an-overview-of-javascript-testing-in-2019-264e19514d0a) 글을 읽고 싶으면 링크를 클릭해보자.



CRA로 만든 react 프로젝트에서 jest를 테스트 해볼 예정이다. 귀찮으면 이미 해놓은 아래 프로젝트를 클론하면서 따라해보자.

```bash
$ git clone https://github.com/likelionSungGuk/todolistRTKtypescript.git
```



## :one: Matchers

### 1. toBe

```javascript
// fn.js
export const fn = {
  add: (num1, num2) => num1 + num2,
  makeUser: (name, age)=> ({
    name,
    age
  })
}
```

```javascript
// fn.test.js

test('a + b =', ()=> {
  // fn.add(1, 2)를 하면 3이 될거라 기대한다.
  expect(fn.add(1, 2)).toBe(3)
})
```

위 처럼 test의 첫번째 인자는 화면에 표시될 텍스트를 넣어주면 된다. 두 번째 인자에는 콜백함수를 열어주고 그 안에서 expect().`matchers` 형식으로 써준다. 여기서 살펴볼 `toBe` matcher는 expect내부의 값이 나타낼 기대값을 의미한다. 결과가 toBe내부와 같다면 true로 테스트를 통과한다.



:bulb: 간단 Tip!

>  가장 처음에 쓰는 `test`는 `it`으로 써도 똑같이 동작한다. 영어 문장에서 `it`으로 쓰면 하나의 완결된 문장을 만들기 쉽기 때문에 it으로도 쓰는 경향이 있다고 한다. 둘 중 취향에 따라 골라 쓰자!



### 2. toEqual / toStrictEqual

```javascript
test('make user: 이름과 나이를 전달받아서 객체를 반환', ()=>{
  expect(fn.makeUser("Mike", 25)).toEqual({
  // expect(fn.makeUser("Mike", 25)).toBe({ // X
    name: "Mike",
    age: 25
  })
})
```

이렇게 객체를 만드는 경우에는 toBe를 쓰면 false가 나온다. `객체는 depth가 깊을 수 있기 때문에` 재귀적으로 타고 들어가는 `toEqual/toStrictEqual`을 toBe대신 써줘야 한다.



### 3. 기타 jest matcher들

위 두 가지 외에도 다양한 matcher들이 있다. 활용법들을 한 번 쭉 살펴보는 것이 좋지만 이 글에서는 소개만 하고 넘어간다.

1. Boolean 

    - toBeNull

    - toBeUndefined

    - toBeDefined


2. Falsy/Truthy

    - toBeFalsy

    - toBeTruthy


3. 대소비교

    - toBeGreaterThan (크다)

    - toBeGreaterThanOrEqual (크거나 같다)

    - toBeLessThan

    - toBeLessThanOrEqual


4. epsilon check
    - toBeCloseTo // ex: add(0.1, 0.2).toBeCloseTo(0.3) -> true (toBe(0,3) -> false)





## :two: Describe

`describe` 라는 키워드를 사용하면 여러 테스트 케이스를 묶을 수 있습니다. 

'sum' 이라는 공통 키워드로 묶을 수 있는 테스트의 경우 describe 키워드를 사용해 하나로 묶어서 테스트할 수 있습니다.

```javascript
// sum.js
function sum(a, b) {
  return a + b;
}

function sumOf(numbers) {
  let result = 0;
  numbers.forEach(n => {
    result += n;
  });
  return result;
}

// 각각 내보내기
exports.sum = sum;
exports.sumOf = sumOf;
```

```javascript
// sum.test.js
const { sum, sumOf } = require('./sum');

describe('sum', () => {
  it('calculates 1 + 2', () => {
    expect(sum(1, 2)).toBe(3);
  });

  it('calculates all numbers', () => {
    const array = [1, 2, 3, 4, 5];
    expect(sumOf(array)).toBe(15);
  });
});
```

![describe로 테스트하면 하나로 묶어준다.](https://i.imgur.com/CP1J77P.png)



### sumOf 함수 리팩토링해서 테스트코드의 진수 맛보기

```javascript
// sum.js
function sum(a, b) {
  return a + b;
}

function sumOf(numbers) {
  return numbers.reduce((acc, current) => acc + current, 0);
}

// 각각 내보내기
exports.sum = sum;
exports.sumOf = sumOf;
```

위와 같이 reduce를 사용해서 sumOf를 리팩토링한 후 그대로 테스트를 돌리면 통과한다. 이렇게 코드를 리팩토링해도 테스트가 있다면 품질을 보장할 수 있다는 것이 장점이다.





## :three: 비동기 함수의 경우 어떻게 테스트를 할 수 있을까?

### 1. done() 

```javascript
// fn.js
export const fn = {
  ...,
  getName: (callback) => {
    const name = "Mike";
    setTimeout(()=> {
      callback(name)
    }, 3000);
  }
}
```

```javascript
// fn.test.js
it('name is Mike', (done) => {
  fn.getName((name) => {
    expect(name).toBe("Mike");
    done();
  });
})
```

여기서 done을 쓰지 않으면 어떻게 될까?

```bash
PASS  src/components/__tests__/fn.test.js
PASS  src/components/__tests__/TodoList.test.js

Test Suites: 2 passed, 2 total
Tests:       5 passed, 5 total
Snapshots:   1 passed, 1 total
Time:        2.715 s

Ran all test suites related to changed files.

Watch Usage: Press w to show more.

```

setTimeout을 3000ms 로 줬는데도 불구하고 2.715s 만에 테스트가 끝나버렸다. 결과는 그냥 PASS.

해당 콜백을 기다리지 않고 지나쳤기 때문인데, 이 때문에 `done()`을 활용한다. 

done()에 도착하기 전까지의 동작을 전부 기다리다가 done에 도착하면 끝난다.



### 2. Promise resolve

(1) then

```javascript
// fn.js
export const fn = {
...,
  getName: (callback) => {
    const name = "Mike";
    setTimeout(()=> {
      callback(name)
    }, 3000);
  },
  getAge: () => {
    const age = 30;
    return new Promise((resolve, reject) => {
      setTimeout(()=> {
        resolve(age)
      }, 3000);
    })
  }
}
```

```javascript
// fn.test.js
it('age is 30', () => {
  // async 2. Promise()
  return fn.getAge().then( age => {
    expect(age).toBe(30);
  });
})
```

fn.getAge()함수는 Promise객체를 리턴해준다. 해당 함수의 return 값을 받아야 하기 때문에 테스트 함수에서도 return을 받아줘야 한다.



(2) Matcher 활용 - resolves

```javascript
// fn.test.js
it('age is 30', () => {
  // async 2. Promise()
    return expect(fn.getAge()).resolves.toBe(30)
  });
})
```

위와 같이 `resolves` matcher를 활용해서 좀 더 간단히 작성할 수 있다.



### 3. async / await

```javascript
// fn.test.js
it('age is 30', async () => {
  // async 3. async/await
	await expect(fn.getAge()).resolves.toBe(30)
});
```

역시나 async/await을 사용하면 가장 간단하게 비동기처리를 할 수 있다.



## :four: 테스트 전/후 처리

### beforeEach / afterEach

```javascript
// fail
let num = 1; 
test('1+1 = 2', () => {
  num = fn.add(num, 1)
  expect(num).toBe(2)
})
test('1+2 = 3', () => {
  fn.add(num, 2)
  expect(num).toBe(3)
})
```

이 경우 num 이 매 테스트마다 재할당되면서 두번째 테스트가 실패하게 된다.

이럴 때는 모든 테스트 이전에 num을 1로 지정해주는 matcher인 beforeEach를 활용하면 된다.

```javascript
// success!!
let num;
beforeEach(()=>{
  num = 1;
})
test('1+1 = 2', () => {
  num = fn.add(num, 1)
  expect(num).toBe(2)
})
test('1+2 = 3', () => {
  num = fn.add(num, 2)
  expect(num).toBe(3)
})
```

- afterEach는 모든 테스트 직후에 실행되는 로직이다.
- 첫 num값만 다르게 주고 싶은 경우에 활용할 수 있다.



### beforeAll / afterAll

만약 각각의 테스트 전/후에 해둬야 할 작업이 중복되면서 꽤 시간이 걸리는 작업이라면 이런 경우에는 beforeAll/afterAll 을 활용해서 테스트 전/후에 한 번만 해두고 테스트가 끝날때까지 유지할 수 있다.

```javascript
// fn
export const fn = {
 ...
  connectUserDB: () => {
    return new Promise((resolve, reject) => {
      setTimeout(()=> {
        resolve({
          name: "Mike",
          age: 30
        })
      }, 3000)
    })
  },
  disConnectUserDB: () => {
    return new Promise((resolve, reject) => {
      setTimeout(()=> {
        resolve()
      }, 3000)
    })
  }
}
```

user DB를 연결하는 상황이라 가정해보자. user DB에 연결하는데 3초, disconnect하는데 3초가 걸린다. 이걸 beforeEach, afterEach로 매 테스트마다 초기화 한다면 매우 비효율적이다. afterAll과 beforeAll을 테스트가 전부 실행되기 전과 전부 실행되고 나서 한 번씩 실행되는 matcher이므로 아래와 같이 사용하면 된다.

```javascript
let user;
beforeAll(async()=>{
  user = await fn.connectUserDB();
})

afterAll(async () => {
  return await fn.disConnectUserDB();
})

test('User name is Mike', () => {
  expect(user.name).toBe("Mike")
})
test('Mike is 30', () => {
  expect(user.age).toBe(30)
})
```





## :five: Mock함수

```javascript
const mockCallback = jest.fn(x => 42 + x);
forEach([0, 1], mockCallback);

// The mock function is called twice
expect(mockCallback.mock.calls.length).toBe(2);

// The first argument of the first call to the function was 0
expect(mockCallback.mock.calls[0][0]).toBe(0);

// The first argument of the second call to the function was 1
expect(mockCallback.mock.calls[1][0]).toBe(1);

// The return value of the first call to the function was 42
expect(mockCallback.mock.results[0].value).toBe(42);
```



mock함수는 왜 사용하는 것일까?

예를 들어 createUser함수가 있다고 가정해보자. 이 함수가 실행되면 db에 실제로 유저가 생성된다. createUser함수를 테스트하기 위해서 createUser함수를 호출한다면 원치않는 유저가 DB에 계속 생성될 수 있다. 때문에 createUser함수를 모방한 함수(mockCreateUser)를 하나 만들어서 모방한 함수가 테스트를 잘 통과하면 createUser도 무사히 통과될 것이라 예상할 수 있다.



```javascript
//
jest.mock('../fn')
fn.connectUserDB().mockReturnValue({name: "Mike", age: 30})

test('test', async ()=>{
  const user = await fn.connectUserDB();
  expect(user.name).toEqual("Mike");
})
```







---

references.

https://learn-react-test.vlpt.us/#/01-javascript-testing

https://www.youtube.com/playlist?list=PLZKTXPmaJk8L1xCg_1cRjL5huINlP2JKt