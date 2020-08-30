# 0824 APS - String

## 1. 문자열 연산

> 문자열은 시퀀스 자료형으로 분류되고, 시퀀스 자료형에서 사용할 수 있는 인덱싱, 슬라이싱 연산들을 사용할 수 있다

```python
메소드
replace()
split()
isalpha()
find()
```

- 문자열은 튜플과 같이 요소값을 변경할 수 없다 (Immutable)



C: ASCII

Java: UTF-16, 2byte

Python: UTF-8



## 2. 문자열 활용하기

### 1. 문자열 뒤집기

algorithm -> mhtirogla



(1) 문자열은 immutable 이므로, List로 바꾸고 시작해야 한다.

(2) 문자열 길이 // 2 의 몫 만큼 바꾸기

예) algorithm -> 문자열 길이 9 이므로 1 - 9 / 2 - 8 / 3 - 7 / 4 - 6 을 SWAP한다

> 풀이 1 - for loop & SWAP 활용

```python
orginal_str = 'algorithm'
string = list(orginal_str)
len_str = len(string)

for i in range(len_str//2):
    string[i], string[len_str-i-1] = string[len_str-i-1], string[i]

result = ''
for i in string:
    result += i

print(result)
```

> 풀이 2 - reverse() 메소드 활용

```python
orginal_str = 'algorithm'
string = list(orginal_str)

string.reverse()
result = ''.join(string)

print(result)
```

> 풀이 3 - String 슬라이싱 활용 -> python 언어에 dominant될 수 있으므로 연습에는 부적합 ㅠ

```python
orginal_str = 'algorithm'
string = orginal_str[::-1]
print(string)
```

> 풀이 4. 문자열 뒤로 붙이기

```python
s = 'algorithm'
empty = ''

for i in s:
    empty = i + empty

print(empty)
```



### 2. eval, repr





## 3. 패턴매칭 

### 1. 완전검색(Brute Force) O(mn)





### 2. 카프-라빈 알고리즘 O(n)





### 3. KMP 알고리즘 O(n) 





### 4. 보이어-무어 알고리즘 O(n) 



