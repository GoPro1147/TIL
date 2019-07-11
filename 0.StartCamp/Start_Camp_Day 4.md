# Start Camp_Day 4
.
127.0.0.1 -> 자기 자신을 가르키는 ip 주소
## Flask를 사용해보자

### 코드를 실행하면 서버가 open됨
> 서버주소 127.0.0.1:5000 을 들어가면 나옴
```python
from flask import Flask
app = Flask(__name__)

# 웹페이지 로드 시 가장 먼저 보이는 곳
@app.route("/")
def hello():
    return "Hello World!"
```
### 127.0.0.1:5000/hi를 치면 확인 가능 
```python
# 127.0.0.1:5000/hi를 치면 확인 가능
@app.route("/hi")
def hi():
    return "Hello hi"
```

### html 태그가 python에서 값을 보낼 때 적용이 가능하다.
```python
# html이 적용되는지 보기위한 코드
@app.route("/html_tag")
def html_tag():
    return "<h1>안녕하세요</h1>"
```
### 두 줄 이상, 두개의 태그를 동시에 보내고 싶어요
> 밑의 코드는 잘못된 코드
```python
# return : 함수 실행 시 반환되는 값
# 반환(output)값은 오직 하나만 선언해야 함
# 밑의 return "<h2>반갑습니다</h2>"는 실행되지 않음
@app.route("/html_tags")
def html_tags():
    return "<h1>안녕하세요</h1>"
    return "<h2>반갑습니다</h2>"
# html_tags 함수 수정
# 여러 줄을 쓰기 위해 """ 안에 넣기 """
```

### 두 태그, 두 줄이상 쓰기 위해 """ """에 넣기
```python
def html_tags():
    return """
    <h1>안녕하세요</h1>
    """
```

### 1학기 끝나는 날을 계산해 볼까???
```python
import datetime
@app.route("/dday")
def dday():
    today = datetime.datetime.now()
    endday = datetime.datetime(2019,11,29)
    d = endday - today
    return f"1학기 종료까지 {d.days}일 남음!!"
```
### render_template()함수를 이용  html 문서를 연결할 수 있다.
```python
from flask import Flask,render_template, 
...
@app.route("/html_file")
def html_file():
    return render_template('index.html')
```
index.html 코드
> html 코드를 간단하게 만들어 주는 단축키 **Ctrl + 1 후 Tab**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>안녕하세요!!</title>
</head>
<body>
    <h1>여기는 나의 첫번째 웹페이지</h1>
</body>
</html>
```


### app.route()함수의 괄호 안에 변수를 선언하는 것을 variable route라고 한다.
```python
# <string:name> string 형식의 name 변수 지정
# 웹페이지에 127.0.0.1:5000/greeting/하하하 치면 결과값을 보자
@app.route("/greeting/<string:name>")
def greeting(name):
    return f"안녕하세요 {name} 님"
```

### {} 안에서도 연산이 된다

```python
@app.route("/cube/<int:num>")
def cube(num):
    return f"{num}의 세제곱은 {num**3}입니다."
```

### Debug 모드를 작동시키기 위한 코드
```python
if __name__ == '__main__':
    app.run(debug = True)
```

### 웹페이지에 데이터를 보내보자
```python
@app.route("/cube_html/<int:num>")
def cube_html(num):
    cube_num = num**3
    return render_template("cube.html",num_html=num,cube_num_html=cube_num)
```
`render_template("문서.html",문서에서 쓸 변수 = python에서 보낼 변수)` 이 함수는 변수 값을 취합해 문서.html에 보내어 완전한 html문서로 바꾸어준다.

cube.html 코드
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <strong>{{num_html}}</strong>의 세제곱은 <i>{{cube_num_html}}</i>입니다.
    <!-- 중괄호 두개 안에다가 파이썬 변수를 담을 수 있다. -->
</body>
</html>
```

또다른 예시
```python
@app.route('/greeting_html/<string:name>')
def greeting_html(name):
    return render_template("greeting.html", name=name)
```
greeting.html 코드
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    {% if name =="태환" %}
        <p style="color:blue">{{name}}</p>아 안녕
    {% else %}
        <p style="color:blue">{{name}}</p>님 안녕하세요!
    {% endif %}
</body>
</html>
```
html코드 내부에서도 조건문을 쓸 수 있다.

### 랜덤으로 음식 정해주기
```python
@app.route("/lunch")
def lunch():
    menu = {
        "짜장면":"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT_cWGrG5DLezSHn6Mc8Zb_EDZHylA5-z_ddG5cBmbvbOyhova4vA",
        "치킨":"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRgMQ1vXPXXoAVyzSQj1hcWXK9k7X6hLf0oCcGHYCQNc6wHQYZs",
        "스파게티":"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRYONoSw5U5_z_OtxfgLgX8_JYNKfQqzcmwe-s83Bvm1qXY2Fyd", 
    }
	#우선 딕셔너리에서 리스트를 뽑는다.
    menu_list =list(menu.keys())
    # 랜덤으로 하나 뽑는다
    pick = random.choice(menu_list)
    # 뽑은 것의 이미지의 링크를 img에 넣는다.
    img = menu[pick]
	# lunch.html에 전달
    return render_template("lunch.html", pick = pick, img = img)
```
lunch.html 코드
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h3>
        오늘의 점심은 {{pick}} 어떠세요?
    </h3>
    <img src="{{img}}" alt="">
</body>
</html>
```
### html 내부에서는 for문도 쓸 수 있다.
```python
@app.route('/movies')
def movies():
    movie_list = ['타짜 3', '스파이더맨', '알라딘', '존윅']
    return render_template("movies.html",movie_list = movie_list)
```
movies.html 코드
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <ol>
        {% for movie in movie_list %}
            <li>{{movie}}</li>
        {% endfor %}
    </ol>
</body>
</html>
```
#### html코드에서 for문과 if문은 반드시 끝나는 부분을 선언해줘야 한다.

### 내부의 html에서 python으로 그리고 다시 html에게 데이터를 보내는 방법
```python
@app.route('/ping')
def ping():
    return render_template("ping.html")

@app.route('/pong')
def pong():
    # 값을 전달하기 위해 함수 사용
    user_input = request.args.get("test")
    return render_template("pong.html",user_input = user_input)
```
ping.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>여기는 핑입니다.</h1>
    <form action="/pong">
        <input type="text" name="test">
        <input type="submit">
    </form>
</body>
</html>
```

`<input type="text" name="test">`ping.html에서 input으로 받은 변수명이 name
`user_input = request.args.get("test")`그 값을 받는 함수인 request.args.get()

pong.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>여기는 퐁입니다.</h1>
    사용자가 방금 입력한 데이터는
    <p>{{user_input}}</p>
    입니다.
</body>
</html>
```
### 내 웹사이트에서 네이버, 구글 검색해보기
python에서 선언
```python
@app.route("/naver")
def naver():
    return render_template("naver.html")

@app.route("/google")
def google():
    return render_template("google.html")
```

naver.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="https://search.naver.com/search.naver">
        <input type="text" name = "query">
        <input type="text" name = "hihi">
        <input type="submit">
    </form>
</body>
</html>
```

google.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>구글 페이크 검색창</h1>
    <form action="https://www.google.com/search">
        <input type="text" name = q>
        <input type="submit">
    </form>
</body>
</html>
```

### 아스키 코드 아트

```python
@app.route('/text')
def text():
    return render_template("text.html")
```
우선 python 에서 경로를 만들어 변환하고 싶은 글자를 입력받게 한다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>입력한 글자를 아스키 코드로 바꿔줍니다.</h1>
    <!-- form action은 내부 경로에서도 이동이 가능하다. -->
    <form action="/result">
        <input type="" name = "raw">
        <!-- submit의 버튼은 value 속성으로 변경이 가능하다. -->
        <input type="submit" value = "변경!!!">
    </form>
</body>
</html>
```
text.html에서 받은 입력값을 이제 아스키코드 아트로 변환해주는 사이트에 넣어야한다.
우선 result 루트에 관해 선언
```python
@app.route('/result')
def result():
	# text.html에서 받은 입력값을 받는다.
    raw_text = request.args.get("raw")
    # 아스키코드로 변환해주는 사이트
    url = "http://artii.herokuapp.com/make?text="
    # 웹 크롤링때처럼 사이트에 요청한다.
    res = requests.get(url + raw_text).text
    # 이제 결과값을 보여주기 위해 result.html을 연다.
    return render_template("result.html",res = res)
```

result.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>결과는 다음과 같습니다.</h1>
    <pre>{{res}}</pre>
</body>
</html>
```
### 간단한 랜덤게임 만들기

만약 조선시대에 태어났다면 어떻게 살았을 까??
랜덤 함수를 이용해 이름을 입력하면 랜덤하게 직업이 나오도록 조절
```python
@app.route("/random_result")
def random_result():
    job = {
        "백정":"http://ojsfile.ohmynews.com/STD_IMG_FILE/2011/1208/IE001378397_STD.jpg",
        "망나니":"http://dn.joongdo.co.kr/mnt/images/file/2016y/05m/13d/20160513000002314_1.jpg",
        "기생":"https://t1.daumcdn.net/cfile/tistory/252DBC3A5281BD561F",
        "왕":"http://img.imbc.com/adams/Content/201311/130292092951497056.jpg",
        "내시":"https://img.insight.co.kr/static/2018/01/17/700/pawu5k09jy55jz8c13t0.jpg",
        "궁예":"http://mimg.segye.com/content/image/2018/08/24/20180824002497.jpg",
    }
    raw_text = request.args.get("name")
    
    job_list =list(job.keys())
    job_pick = random.choice(job_list)
    job_img = job[job_pick]
    return render_template("random_result.html",job_pick = job_pick, name = raw_text, job_img = job_img)
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
        <h1>{{name}}의 어울리는 직업은 바로 {{job_pick}}</h1>
        <img src="{{job_img}}" alt="">
</body>
</html>
```
### 자신의 로또 번호를 입력받아 이전 로또 당첨 번호와 비교해보기
우선 로또를 입력받을 사이트를 생성
```python
@app.route('/lotto')
def lotto():
    return render_template("lotto.html")
```
lotto.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="/lotto_result">
        <input type="text" name = "numbers">
        <input type="submit">
    </form>
</body>
</html>
```
lotto.html에서 받은 번호를 이제 비교해야함

```python
@app.route('/lotto_result')
def lotto_result():
    # 사용자가 입력한 정보를 가져오기
    # user_numbers = 12 5 4 7 34 9 8 이런식으로 띄어쓰기로 구분해야함
    numbers = request.args.get("numbers").split()
    user_numbers = []
    for a in numbers:
        user_numbers.append(int(a))
    
    # 로또 홈페이지에서 정보를 가져오자
    url = "https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=866"
    res = requests.get(url)
    # json파일은 python에서 문자열로 보임
    # 그래서 json()을 이용해줘야함
    lotto_numbers = res.json()
    winning_numbers = []
    for i in range(1,7):
        winning_numbers.append(lotto_numbers[f'drwtNo{i}'])
    bonus_number = lotto_numbers['bnusNo']
    bnus_check = 1

    for b in range(len(user_numbers)):
        if user_numbers[b] == bonus_number:
            bnus_check = 0

    matched = set(user_numbers) & set(winning_numbers) 
    if len(matched) == 6:
        result = "1등"
    elif (len(matched) == 5) & (bnus_check == 0):
        result = "2등"
    elif len(matched) == 5:
        result = "3등"
    elif len(matched) == 4:
        result = "4등"
    elif len(matched) == 3:
        result = "5등"
    else:
        result = "꽝"

    return render_template("lotto_result.html",u = user_numbers, w = winning_numbers, b = bonus_number, r = result)
```

lotto_result.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    {{u}}
    {{w}}
    {{b}}
    {{r}}
</body>
</html>
```
