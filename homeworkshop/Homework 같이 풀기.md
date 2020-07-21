# Homework 같이 풀기

## 1. 예약어

['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']



## 2. 실수비교

```python
num1 = 0.1 * 3
num2 = 0.3

#num1 과 num2 비교
print(num1 == num2) -> False

import math
result = math.isclose(num1, num2)
```





## 3. 줄바꿈/탭

```python
'\n'   #줄바꿈
'\t'   #탭
'\\'   #역슬래시 
```



## 4. formatting "안녕, 철수야"

```python
name = "철수"

print(f"안녕, {name}야")

print("안녕, {}야".format(name))
```



## 5. 형변환

```python
int('3.5') -> error
# 
```



## 6. 별

```python
n, m = 5, 9
print(("*" * n + "\n") * m)
```



## 7. 문자열

```python
message = '\"파일은 c:\\Windows\\Users\\내문서\\python에 저장이 되었습니다.\"\n나는 생각했다. \'cd를 써서 git bash로 들어가 봐야지.\''
print(message)
```



## 8. 근의공식

```python
(-b +- (b**2 -4*a*c)**(1/2)) / (2*a)
```



