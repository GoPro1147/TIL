# Start Camp_Day 1

## Python 기본 문법

### 1. 저장
#### 1-1. 변수
- `dust = 10`의 의미는 **dust라는 상자에 10을 보관**하는 것
- `dust == 10`의 의미는 **dust와 10은 같은 값**이다 라는 뜻

#### 1-2. 리스트(List)
- `dusts = [55, 123, 144,]` 의 의미는 **속성이 같은 수치들을 묶은 것**
- `dust[0] = 55` -> Python의 경우 첫 번째 원소를 부를 때 **0번째**부터 시작

#### 1-3. 딕셔너리(Dictionary)
```python
dusts = {
	"1일차":30,
	"2일차":40,
	"3일차":50,
}
```
> Tip : 딕셔너리, 리스트 만들 때 경우 마지막 원소에도 ,(세미콜론)을 삽입하여 
> 추후 데이터 삽입 시  오류 없이 넣기 위해 추가할 것
> 딕셔너리를 추가할 때 이왕이면 위의 형식을 준수하면 조금 더 깔끔함

### 2. 조건
> Python은 들여쓰기로 그 문장에 속해있는지 안 속해있는지 파악한다

#### 2-1. if문 (조건문)

```python
if(True):
	print("if문 안쪽입니다.")
```

#### 2-2. if/else문 (조건문)
```python
if(True):
	print("if문 안쪽입니다.")
else:
	print("else문 안쪽입니다.")
```

#### 2-2. if/elif/else문 (조건문)
```python
if(True):
	print("if문 안쪽입니다.")
elif:
	print("elif문 안쪽입니다.")
elif:
	print("elif문 안쪽입니다.")
	...
else:
	print("else문 안쪽입니다.")
```
### 3. 반복
> Python은 들여쓰기로 그 문장에 속해있는지 안 속해있는지 파악한다
#### 3-1 while문
##### 형식
```python
while(조건)
	조건이 참이라면 계속 돌아감
	조건이 참이라면 계속 돌아감
```
##### 예시(3번 인사하는반복문)
```python
n = 0
while(n<3):
	print("Hello")
	n = n + 1
```

#### 3-2 for문
##### 형식
```python
for i in 어떠한 list
	list의 길이만큼 반복함
```
##### 예시(dusts 리스트에 있는 값 출력)
```python
dusts = [55, 123, 144,]
for i in list(dusts)
	print(i)
```
> while과 for문의 차이는 종료 조건 명시에 있는지 없는지의 차이
> while문은 종료 조건이 필수적 없으면 무한루프에 걸림
> for문은 종료 조건이 없어도 리스트의 길이가 유한하기에 종료됨

### 추가로 알아야할 사항??
#### 내장함수
`print()`는 파이썬이 가지고 있는 내장함수이다.
> 내장함수에 다 포함하지 않는 이유는 기능을 이것 저것 추가하면
> 복잡하기 때문에 필요로 하는 기능들만 넣음

#### 외장함수 random 함수
`random.choice(list목록)` : list 목록 중 하나의 항목을 Random으로 출력
`random.sample(list목록,k)` : list 목록 중 k개 만큼 Random으로 출력하는데 겹치지 않음

> 외장함수를 쓰기 전 반드시 `import 외장함수 명`을 입력해야함
```python
import random
...
random.sample(list,k)
```

#### print(f"{변수이름}")

`print(f"blah blah {변수이름} blah")` 시 f의 역할은 "문자열" 안에 변수를 참조하기 위해 선언해야 함


