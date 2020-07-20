# Homework

## 1. python에서 사용할 수 없는 식별자(예약어)

```python
False, None, True, and, as, assert, break, class, continue, def, del, elif, else, except, finally, for, from, global, if, import, in, is, lambda, nonlocal, not, or, pass, raise, return, try, while, with, yield
```



## 2. 실수 비교

```python
num1 = 0.1 *3
num2 = 0.3

#1
num1 = round(num1)
print(num 1 == num2)

#2
import sys
print(sys.float_info.epsilon)
abs(num1-num2) <= sys.float_info.epsilon


#3
import math
num1 = math.isclose(num1, num2)
print(num 1 == num2)

```



## 3. 이스케이프 시퀀스

1) 줄바꿈: \n

2) 탭: \t

3) 백슬래쉬: \\



## 4. String Interpolation

```python
name = "철수"

#1. %-formatting
print("안녕 %s야" % name)

#2. .format
print("안녕 {}야".format(name))

#3. f-string
print(f"안녕 {name}야")
```





## 5. 형 변환

```python
str(1) - O

int('30') - O

int(5) - O

bool('50') - O

int('3.5') - X
#str이 int로 형변환 되기 위해서는 정수값을 문자의 형태로 가지고 있을 때만 가능하다
```



## 6. 네모 출력

반복문 안쓰고 못하겠습니다...

```python
n = 5
m = 9
star_n = "*" * n


print((star_n + "\n") * m) 
```



## 7. 이스케이프 시퀀스 응용

```PYTHON
print('\"파일은 c:\\Windows\\Users\\내문서\\python에 저장이 되었습니다.\"\n 나는 생각했다. \'cd를 써서 git bash로 들어가 봐야지\'')
```





## 8. 근의 공식

```python
(-b +- (b**2-4ac)**(1/2)) / (2a)
```



