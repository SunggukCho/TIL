# 0721workshop

## 1. 간단한 n의 약수 (SWEA # 1933)

```python
#나의 풀이
number = int(input("1~1000 사이의 정수를 입력하세요: "))

divisor = []
i = 1
for i in range(1, number+1):
    if number % i == 0:
        divisor.append(i)
        i += 1
    else:
        i += 1

print(divisor)
```

```python
#강사님 풀이
N = int(input("1~1000 사이의 정수: "))

for i in ragne(1, N+1):
    if N % i == 0:
        print(i, end =" ")         #약수
```



## 2. 중간값찾기

```python
numbers = [
           85, 72, 38, 80, 69, 65, 68, 96, 22, 49, 67,
           51, 61, 63, 87, 66, 24, 80, 83, 71, 60, 64, 
           52, 90, 60, 49, 31, 23, 99, 94, 11, 25, 24
]
numbers.sort()

def find_median(numbers):
    if len(numbers) % 2 == 0:
        mid1 = int(len(numbers)/2)
        mid2 = int(len(numbers)/2) +1
        print(numbers[mid1])        
    else:
        mid = round(len(numbers)/2)
        print(numbers[mid])
        
        
find_median(numbers)
```



## 3. 계단 만들기

```python
number = int(input("숫자를 입력하세요: "))

i = 0
j = 0
for i in range(1, number+1):    #몇 줄 출력할 것인지
    for j in range(1, i+1):     #한 줄에 몇 개 출력할 것인지
        print(j, end = " ")
        j += 1
    print()
```





## 4. 구구단을 외자 2 x 1?1

```python
#N단을 나열하는 코드
number = int(input("1 ~ 9까지의 숫자를 넣어주세요"))
i = 0 
for i in range(9):
    print(str(number) + " x " + str(i+1) + " = ", + number * (i+1))
```



```python
#2단부터 9단까지 쭉 나오는 코드
for i in range(2, 10):
    print(f'-----[{i}단]------')
    for j in range(1, 10):
        print(f'{i} X {j} = {i*j}')
```

