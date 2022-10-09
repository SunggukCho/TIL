# python 헷갈린 개념 
- 1. float연산, 
- 2. String interpolation

## 1. float 연산 주의사항

```python
a = 3.5
b = 3.2
a+b == 3.7 (True)
but, a-b == 0.3 (False)
c = 0.3
d = a-b
```

실제로 a-b = 2.9999999...로 연산된다.

따라서 실수의 연산은 항상 주의해야 한다.



### 1.1 해결방법 (1) 라운딩(Round + ing)

가장 쉬운 `해결 방법은 반올림(round)`해주는 것이다

```python
# 해결법 Rounding!
round(a-b) = 0.3 (True)
```

바로 a-b 값을 사용하기보다는, 이 round 된 값을 다른 변수에 할당해서 사용하면 된다.



### 1.2 해결방법 (2) epsilon

```python
# sys모듈의 epsilon을 사용해서 해결할 수 있다
# `epsilon`은 부동소수점 연산에서 반올림을 함으로써 발생하는 오차를 상환하는 수
import sys
print(sys.float_info.epsilon)
#sys.float_info.epsilon 값은 상당히 작은 값이기 때문에, 오차가 이 수 보다 작은 경우 둘을 같은 수라고 볼 수 있다.
abs(a-b) <= sys.float_info.epsilon
```



### 1.3 해결방법 (3) math module

```python
# math module을 활용하는 방법
import math
math.isclose(c, d) == True (True)
```

이 때, c==d 가 된 것이 아니다. 

c와 d는 이전과 값이 그대로 동일하므로 동일 값을 사용하려면

`result = math.isclose(c,d)`으로 다른 문자열로 할당해서 사용하면 된다. 



---



## 2. String interpolation 문자열 삽입

> 일반 문자열(str) 속에 변수를 넣는 것이라 생각하면 된다.
>
> 예) "my name is {name}" 이라는 문자열을 출력한다.
>
> name = "John" 인 경우, my name is John 으로 출력된다.



이 방식은 총 3가지 방식이 있고 뒤로갈수록 최신 방법이다. 

하지만 오래된 문서나 코드에는 이전 버전으로 되어있을 수 있으니 모두 알아두는 것이 좋다.



refs. 

- %-formatting  https://www.learnpython.org/en/String_Formatting

- str.format()  https://pyformat.info/

- f-strings  



### 2-1. %-formatting

- Type1. %s - String (or any object with a string representation, like numbers) - 기본적으로 문자열, 그러나 int나 float이 아닌 모든 형식은 %s를 쓴다고 생각하면 편하다.
- Type2. %d - Integers. int는 %d

```python
name = "John"
age = 30
pi = 3.14

print('내 이름은 %s 이고 %d살 입니다' % (name, age))
-> '내 이름은 John이고 30살입니다'
```

- Type3. %f - Floating point numbers. float은 %f
- Type4. %.<number of digits>f - Floating point numbers with a fixed amount of digits to the right of the dot. float중에서 소수 자리 수를 선택하려면 f앞에 숫자를 써준다.

```python
pi = 3.141592
{2f}.format(pi) => 3.14           #소수 두 번째 자리 수 까지 표현 

mylist = [1, 2, 3]
print("A list is %s" % mylist)    #list도 %s를 사용한다
'A list is [1, 2, 3]'
```



### 2-2. str.format()

```python
'내 이름은 {} 입니다. 제 나이는 {}입니다'.format(name, age)
'{} {}'.format('one', 'two')
'{1} {0}'.format('one', 'two')
```

- 내 이름은 John입니다. 제 나이는 30입니다.
- one two
- two one

이렇게 문자열 뒤에 .format()을 붙여주고, 이때 괄호()안에 표현하고 싶은 변수, 문자열, 딕셔너리, 함수, 클래스 등을 넣어줄 수 있다.

```python
data = {'first': 'Hodor', 'last': 'Hodor!'}
'{first} {last}'.format(**data)
-> 'Hodor Hodor!'
```



### 2-3. f-strings (python 3.6 이후 버전. 현재까지 최신)

f-strings 는 모든 타입에 대해서 f글자를 사용한다. 문자열, int, float, list, 함수 등이 사용 가능하다

1. 문자열 & int 예시

```python
name = "John"
age = 30
f"Hello, {name}. You are {age}."
-> 'Hello, John. You are 30.'
```

2. 함수 예시

```python
def to_lowercase(input)
    return input.lower()

name = "John"
f"{to_lowercase(name)} is funny."
-> 'john is funny.'                #대문자 -> 소문자 변환 함수 적용
```

3. 내장함수, method 등 사용 예시

```python
f"{name.lower()} is funny."
-> 'john is funny'
```

4. multiline f-strings 예시

```python
name = "John"
profession = "comedian"
affiliation = "Monty Python"
message = (
    f"Hi {name}. "
    f"You are a {profession}. "
    f"You were in {affiliation}." )
print(message)
'Hi John. You are a comedian. You were in Monty Python.'
```





---