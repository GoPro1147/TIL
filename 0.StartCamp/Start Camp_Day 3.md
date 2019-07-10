# Start Camp_Day 3

## Python을 이용해 파일을 가지고 놀기
### 1. 살짝 old한 Ver.
```python
# open 함수의 경우 파일 명과 어떤 방식으로 열 것인지 선언해야함
# 두번째 변수의 값들
# r : 말그대로 읽기
# w : 기존 데이터 지우고 다시 덮어쓰기
# a : 기존 데이터에 새로 추가시킴
f = open("file.txt",'w')
f.write("아녕하세요")
f.close()
```
### 2. 요즘 Style의 Ver.
```python
# with 문 내에서 open된 건 난 f라고 부를거야!
with open("ssafy.txt", 'w') as f:
	f.write("싸피화이팅!!!")
```
with 구문의 경우는 따로 `f.close()`를 쓸 필요가 없음

> 주석 처리시 단축키 **Ctrl + /**
> 주석 시킬 부분을 드래그 후 단축키를 누르면 주석 처리가 된다.
> VScode가 언어에 따른 주석 처리를 자동으로 해준다.


### 3. csv 파일을 다뤄보자
```python
import csv
lunch = {
    "BBQ" : "123-4567",
    "중국집" : "789-789",
    "한식" : "456546",
}
# encoding을 하는 이유는 한글이 깨질수 있어서
# newline = ""은 csv를 만들 때 windows에서 한칸씩 공백을 넣기 때문에 이를 방지
with open("lunch.csv",'w',encoding = "utf-8",newline = "") as f:
	#writer는 csv 파일을 써주는 친구(csv 파일 전문가)
	csv_writer = csv.writer(f)
	
	for item in lunch.items():
	csv_writer.writerow(item)
```
#### 3-1. 저번 시간의 환율 정보를 csv파일로 저장해보자
```python
import requests
import csv
from bs4 import BeautifulSoup

url = "https://finance.naver.com/marketindex/exchangeList.nhn"

response = requests.get(url).text
soup = BeautifulSoup(response,"html.parser")

tr = soup.select('tbody > tr')

with open("naver_exchange.csv",'w',encoding = "utf-8",newline = "") as f:
    csv_writer = csv.writer(f)
    for r in tr:
    	# 출력 확인을 한다.
        print(r.select_one('.tit').text.strip())
        print(r.select_one('.sale').text)
        row =[r.select_one('.tit').text.strip(),r.select_one('.sale').text]
        csv_writer.writerow(row)
```

`<태그이름 속성명 ="속성 값"></태그이름>`
`<태그이름 속성명 ="속성 값" 속성명2="속성 값"></태그이름>`

#### 3-2. 빗썸을 크롤링해 각종 코인의 정보를 csv파일로 저장해보자
```python
	import requests
import csv
from bs4 import BeautifulSoup

url = "https://www.bithumb.com/"

response = requests.get(url).text
soup = BeautifulSoup(response,"html.parser")

tr = soup.select('tbody > tr')
with open("bitcoin_exchange.csv",'w',encoding = "utf-8",newline = "") as f:
    csv_writer = csv.writer(f)
    for r in tr:
        print(r.select_one('.blind').text)
        print(r.select_one('.sort_real').text)
        row = [r.select_one('.blind').text,r.select_one('.sort_real').text]
        csv_writer.writerow(row)
```
#### 3-3. 다나와에서 삼성 evo 860의 최저가를 가져와보자
준비중
```python

```


## HTML & CSS 맛보기
> 모르는 태그는 https://www.w3schools.com/ 에서 찾아보자
### HTML
HTML(HyperText Markup Language)
코드로 보는 사람도 있고 그냥 문서로 보는 사람도 있음

시작은 논문을 편하게 관리하고 보기 위해 시작

HTML 코드는 대부분  <태그이름></태그이름>으로 구성되어 있다.

다음의 밑의 코드를 보고 분석해보자
```html
<!DOCTYPE html>
<html>
    <head>
    	<style>
        	h1 {
    			background-color:red;
		}
			a {
    			color : brown;
		}
			.blue {
    			background-color : blue;
		}
			#git {
    			margin-top: 10px;
    			background-color : black;
    			border-bottom-right-radius: 10px;
    			width: 15px;
		}
		</style>
    </head>
    <body>
        <h1>HTML</h1>
        <h1 class="blue">CSS</h1>
        <h2 class="blue">HyperText Markup Language</h2>
        <a href="https://naver.com">네이버</a>
        <!-- <태그이름></태그이름> -->
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2"></태그이름> -->

        <!-- <ol>은 orderlist의 준말 -->
        <!-- <li>은 list의 준말 -->
        <ol> 
        <li><strong>파이썬</strong></li>
        <li class="blue">HTML</li>
        <li id="git" class="blue">Git</li>
        </ol>        
    </body>

</html>

```
`<!DOCTYPE html> ` : 시작 시 이 문서가 무슨 형식으로 써졋는지 알려줄 필요가 있어서 삽입
`<style></style>`   : 각 태그나 class id의 속성을 미리 입력함
> 속성을 지정하는 방법은 여러가지가 있다.
> `<h1 background-color : blue>HTML</h1>` 이런식으로 태그 선언하면서 스타일 지정
> 하지만 이왕이면 모아두는게 더욱 깔끔

`<a href="https://naver.com">네이버</a>` : 웹 주소를 기입해 누르면 이동할 수 있는  
하이퍼링크 생성

`<ol></ol>` : orderlist의 준말 list를 만들 때 쓰임 이때 <li></li>에 리스트에 기입할 정보를 입력
`<strong>파이썬</strong>` : 파이썬이라 적힌 부분을 굵은 글씨체로 표시

#### ID와 Class의 차이
속성을 적용하는 것은 같다. 다만, Class에 적용된 속성보다 ID에 적용된 속성을 우위로 처리한다.
즉 Class는 많이 쓰이는 부분에 선언해주고 ID는 특정한 부분에 선언해주는 것이 좋다.
`<li class="blue">HTML</li>` : 코드 실행시 HTML 부분의 배경이 파란색으로 색칠

`<li id="git" class="blue">Git</li>` : Class보다 ID가 위이므로 Git이 검은색으로 색칠

### CSS

<style></style> 사이에 있는 것은 html을 꾸미는 파일
따로 관리할 수 있도록 파일을 intro.css로 만들자

```css
/*  여기는 css파일입니다 
	css는 주석 표시가 다르다
*/
h1 {
    background-color:red;
}
a {
    color : brown;
}
.blue {
    background-color : blue;
}
#git {
    margin-top: 10px;
    background-color : black;
    border-bottom-right-radius: 10px;
    width: 15px;
}

```

그리고 html파일에 이 css파일을 쓴다고 선언해주어야 한다.
`<style></style>` 내의 부분을 `<link rel="stylesheet" href="./intro.css">` 로 변경

변경된 html
```html
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="./intro.css">
    </head>
    <body>
        <h1>HTML</h1>
        <h1 class="blue">CSS</h1>
        <h2 class="blue">HyperText Markup Language</h2>
        <a href="https://naver.com">네이버</a>
        <!-- <태그이름></태그이름> -->
        <!-- <태그이름 속성명="속성값" 속성명2="속성값2"></태그이름> -->

        <!-- <ol>은 orderlist의 준말 -->
        <!-- <li>은 list의 준말 -->
        <ol> 
        <li><strong>파이썬</strong></li>
        <li class="blue">HTML</li>
        <li id="git" class="blue">Git</li>
        </ol>        
    </body>

</html>

```