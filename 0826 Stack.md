# 0826 Stack

## 1. 특성

- 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조
- 스택에 저장된 자료는 선형 구조를 갖는다
  - 선형구조: 자료 간의 관계가 1대1의 관계를 갖는다
  - 비선형구조: 자료간의 관계가 1대 N의 관계를 갖는다
- 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있다
- 마지막에 삽입한 자료를 가장 먼저 꺼낸다. 후입선출(LIFO)
  - 예를 들어 스택에 1, 2, 3순으로 삽입한 후 꺼내면 역순인 3, 2, 1순으로 꺼낼 수 있다.



## 2. 구현

### 연산

- 삽입: push
- 삭제: pop
- 공백인지 아닌지 확인하는 연산: isEmpty

- 스택의 top에 있는 원소(item)을 반환하는 연산: peek



### 삽입/삭제 과정

![팁스소프트 > MFC/API 가이드 > [자료구조] 스택(Stack)에 대하여](https://lh3.googleusercontent.com/proxy/dSo0DyJ3FDnc5sJGXgLXnhBXPATzBWdFcCxIajusLsgT1CUJXmRFKc1As_P7pVil5sjF92H4-1nm_P2EtiwwaVf2F79Gw4Lq3pfixg)

- push 할 때는 (1) top + 1하고 (2) push
- pop 할 때는 (1) 꺼내고, (2) top -1



### 함수 만들기

```python
def push(item):
    global top
    if top > 100 - 1:
        return
    else:
        top += 1
        stack[top] = item

def pop():
    global top
    if top == -1:
        print("stack is empty")
        return
    else:
        result = stack[top]
        top -= 1
        return result

stack = [0] * 100           #고정
top = -1
```

- stack (List) 자료형은 `'참조형'`이므로, 전역변수가 함수 내에서 `Read, Write가 모두 가능`하다
- 하지만 top과 같은 자료형은 `'값형'`은 `'Read'` 만 가능하다.
- 따라서 함수 내에서 사용할 때 top은 global을 해주는 것이다.



## 3. 응용 

### 1. 괄호검사

- 조건1. 왼쪽 괄호의 개수와 오른쪽 괄호의 개수가 같아야 한다.
- 조건2. 같은 괄호는 왼쪽 괄호가 오른쪽 보다 먼저 나와야 한다
- 조건3. 괄호 사이에는 포함 관계만 존재한다

```python
if((i==0) && (j==0) 
```

- 이 문자가 stack에 들어왔다. 올바로 사용되었는지 Stack을 사용해서 체크해보자

- 왼쪽 괄호가 나오면 push, 오른쪽 괄호가 나오면 pop

- 이렇게 해서 마지막에 stack에 괄호가 남아있지 않으면 올바른 것이고 아니면 잘못된 형식이다.
- 도중에 스택이 empty 상황이 나오더라도 조건 1, 2에 맞지 않는다.



### 2. Fucntion call

- 프로그램 에서의 함수호출과 복귀에 따른 수행 순서를 관리
- 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조이므로, 후입선출 구조의 스택을 이용하여 수행순서 관리
- 함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임에 저장하여 시스템 스택에 삽입
- 함수의 실행이 끝나면 시스템 스택의 top원소(스택프레임)을 삭제(pop) 하면서 프레임에 저장되어 있던 복귀 주소를 확인하고 복귀
- 함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면서 시스템 스택은 공백스택이 된다.



### 3. 재귀호출

>  자기자신을 호출하여 순환 수행되는 것

- 함수에서 실행해야 하는 작업의 특성에 따라 일반적인 호출 방식보다 재귀호출 방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성

  - 예) 팩토리얼

    ```python
    def fact(n):
        if n == 1:
            return 1
        else:
            return n * fact(n-1)
    ```

  

  - 피보나치 - O(2**n)

    ```python
    def fibo(n):
        if n < 2:
            return n
        else:
            return fibo(n-1) + fibo(n-2)
    ```

    

## 4. 메모이제이션 Memoization O(n)

> 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행속도를 빠르게 하는 기술
>
> 동적 계획법(DP)의 핵심이 되는 기술이다.

- 글자 그대로 해석하면 ''메모리에 넣기''라는 의미이며,  '기억되어야할 것'이라는 뜻이다. 흔히 기억하기, 암기하기 라는 뜻의 memorization과 혼동하지만 다른 단어이다.

```python
def  fibo2(n):
    if n >= 2 and len(memo) <= n:
        memo.append(fibo2(n-1)+fibo2(n-2))
    return memo[n]

memo = [0,1]
print(fibo2(7))
```

- 한 번 계산할 때 memo라는 배열에 값을 추가한다.
- 따라서 다시 호출할 때는 그 값을 재귀로 구하지 않고 memo에서 그 값을 불러온다.
- 이렇게 되면 2^n 이던 시간복잡도가 n으로 확 줄어든다.
  - -> python은 재귀 depth 가 1000까지 가능하다. 이 메모이제이션을 사용하면 fibo2(998) 까지 연산이 가능하다.



## 5. DP (Dynamic Programming)

> 동적 계획 알고리즘은 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘
>
> 동적 계획 알고리즘은 먼저 입력 크기가 작은 부분 문제들을 모두 해결한 후에 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결하여 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘이다.

```python
def fibo(n):
    f = [0,1]
    for i in range(2, n+1):
        f.append(f[i-1]+f[i-2])
    return f[n]

print(fibo(6))
```

- 피보나치 수 DP적용
  - 피보나치 수는 부분 문제의 답으로부터 본 문제의 답을 얻을 수 있으므로 최적 부분 구조로 이루어져 있다.

1. 문제를 부분 문제로 분할한다.
   - Fib(n) = Fib(n-1) + Fib(n-2)
   - Fib(n-1) = Fib(n-2) + Fib(n-3)
   - ....
   - Fib(2) = Fib(1) + Fib(0)
2. 부분 문제로 나누는 일을 끝냈으면 가장 작은 부분 문제부터 해를 구한다
3. 그 결과는 테이블에 저장하고 테이블에 저장된 부분 문제의 해를 이용하여 상위 문제의 해를 구한다.



- 정리하면, memoization은 재귀함수 구조의 비효율을 개선했지만 그래도 재귀함수 구조이다. 반면 DP는 재귀함수 구조가 아니다. 

- 따라서 Memoization을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능면에서 더 효율적이다. 재귀적 구조는 오버헤드가 발생하기 때문이다.



## 6. DFS

비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요

1. 깊이 우선 탐색 (DFS)
2. 너비 우선 탐색 (BFS)

이렇게 있는데 오늘 배울 내용은 `깊이 우선 탐색 (DFS)이다`



> DFS는, 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법

가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 `스택` 사용



1. 시작 정점 v를 결정하여 방문한다
2. 정점 v 에 인접한 정점 중에서
   1. 방문하지 않은 정점 w가 있으면 정점 v를 스택에 push하고 정점 w를 방문한다. 그리고 w를 v로하여 다시 2)를 반복한다.
   2. 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로하여 다시 2)를 반복한다.
3. 스택이 공백이 될 때까지 2)를 반복한다.

```python
# 정점, 간선
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
def dfs(v):
    #방문체크
    visited[v] = 1
    print(v, end=' ')
    #v의 인접한 정점 중, 방문 안 한 정점을 재귀 호출
    for w in range(1, N+1):
        if G[v][w] == 1 and visited[w] == 0:
            dfs(w)

N, E = map(int, input().split())            #정점
temp = list(map(int, input().split()))      #간선들

#인접행렬
G = [[0] * (N+1) for _ in range(N+1)]

#방문체크
visited = [0] * (N+1)

#간선들을 인접행렬에 저장
for i in range(E):
    s, e = temp[2*i], temp[2*i+1]
    #start check
    G[s][e] = 1								#행렬에 간선있으면 1 넣어줌
    G[e][s] = 1								#뒤바꿔서도 해줘야 무방향 그래프를 뜻함. 한쪽만 있으면 방향 그래프임.

dfs(1)
[output] 1 2 4 6 5 7 3 
```






