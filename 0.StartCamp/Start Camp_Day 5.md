# Start Camp_Day 5

## 텔레그램에 ChatBot 만들기

### 웹페이지에서 ChatBot의 메시지를 전달하기
> 텔레그렘에서 botfather를 이용해 bot을 만들어야함
> 과정이 조금 기억이 안나 나중에 정리해보자
```python
from flask import Flask, request, render_template
from decouple import config
api_url = "https://api.telegram.org"
# TELEGRAM_TOKEN과 CHAT_ID를 감추기 위해 config 함수 쓰기
token = config('TELEGRAM_TOKEN')
chat_id = config('CHAT_ID')

# 사용자의 input 값을 받는 홈
@app.route("/write")
def write():
    return render_template("write.html")
# 사용자의 input값을 이제 그걸 텔레그렘에 전달
@app.route("/send")
def send():
    msg = request.args.get("msg")
    url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={msg}"
    requests.get(url)
    return render_template("send.html")
```
write.html
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
   <form action="/send">
       <input type="text" name = "msg">
       <input type="submit">
   </form> 
</body>
</html>
```
send.html
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
    <h1>성공적으로 메세지 전달 완료</h1>
</body>
</html>
```

### 챗봇에 이제 명령을 내려 결과가 나오게 하기

#### 점심메뉴 추천해주기
```python
@app.route(f"/{token}",methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    # 요청받은 data에서 유저의 id와 msg를 저장하자
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')
	if user_msg == "점심메뉴":
            menu_list = ["삼계탕", "철판낙지덮밥", "물냉면",]
            result = random.choice(menu_list)
```
우선 텔레그램에 채팅을 하면 채팅한 사람의 id와 msg를 저장해준다.
그리고 메뉴리스트를 미리 만들어서 random.choice를 이용해 랜덤하게 하나의 메뉴를 선택해 응답한다.
#### 명언
```python
Fighting = ["쉽게 얻을 수 있는 것 중에서 가치 있는 건 거의 없어요. \
                희생을 요하는 힘겨운 싸움을 통해 도달할 수 있는 목표만이 \
                진정 추구할 만한 가치가 있으니까요. ",
                "행운은 도전하는 자만이 얻을 수 있습니다. ",
                "실패는 우리가 다른 방향으로 나아갈 수 있게 해주는 표지판이에요. ",
                "삶은 용기의 크기에 따라 축소되거나 확장됩니다.",
                "습관적인 일상에서 한 걸음만 벗어나도 우리는 새로운 방향으로 나아가게 됩니다.",
                "자신을 사랑하게 되면 마주치는 모든 사람에게 그 사랑을 확장시키는 법을 배울 수 있어요.",

    ]
    elif user_msg == "명언":
            result = random.choice(Fighting)
    
```
#### 로또
```python
elif user_msg == "로또":
            result = sorted(random.sample(list(range(1,46)),6))
```
elif로 묶어서 하자면 오름차순으로 묶어서 동시에 6개의 숫자를 선택하는 코드를 작성

#### 네이버 파파고를 이용해 번역하기
```python
# list_exam = [1, 2, 3, 4, 5]
# 어떤 list_exam[0:2] == [1, 2]
# string 도 마찬가지
# list_example_2 = "노는게제일좋아"
# list_example_2[0:3] = "노는게"
# list_example_2[3:] = "제일좋아"
elif user_msg[0:2] == "번역":
            raw_text = user_msg[3:]
            papago_url="https://openapi.naver.com/v1/papago/n2mt"
            data = {
                "source":"ko",
                "target":"en",
                "text":raw_text,
                # raw_text에 번역할 언어 들어감
            }
            # papago를 쓰기 전 api를 쓰기 위해 id와 비밀번호?? 발급
            header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret,
            }
            
            res = requests.post(papago_url,data=data,headers=header)
            translate_res = res.json()
            translate_result = translate_res.get('message').get('result').get('translatedText')
            
            result = translate_result
```
#### 네이버 클로바 얼굴인식 사용해보기
```python
# requests.get.json() -> in flask
# request.get_json() -> in request
# 둘의 차이가 있음 다른 메소드

# 사용자가 보낸 사진을 찾는 과정
        file_id = data.get('message').get('photo')[-1].get('file_id')
        file_url = f"{api_url}/bot{token}/getFile?file_id={file_id}"
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"
        
        # 사용자가 보낸 사진을 클로바로 전송
        # res -> 사진에 관한 데이터

        res = requests.get(file, stream = True)
        ## 네이버
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret,
            }    
        clova_res = requests.post(clova_url, headers=header, files = {"image":res.raw.read()})
        # get 함수를 통해 계속해서 딕셔너리를 탐색해야 한다.
        # 만약 혼자 한다면 천천히 할 것
        if clova_res.json().get('info').get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get('confidence')
            result = f"{name}일 확률이 {confidence*100}%입니다."

        else:
            # 사람이 없음
            result = "사람이 없습니다."
```
##### 전체 코드

```python
# 왜 이런 route가 생긴지 모르겠음
# 웹 서비스 POST,GET,DELETE,UPDATE를 알아야 이해 가능할 듯
@app.route(f"/{token}",methods=['POST'])
def telegram():
    print(request.get_json())
    data = request.get_json()
    user_id = data.get('message').get('from').get('id')
    user_msg = data.get('message').get('text')
    Fighting = ["쉽게 얻을 수 있는 것 중에서 가치 있는 건 거의 없어요. \
                희생을 요하는 힘겨운 싸움을 통해 도달할 수 있는 목표만이 \
                진정 추구할 만한 가치가 있으니까요. ",
                "행운은 도전하는 자만이 얻을 수 있습니다. ",
                "실패는 우리가 다른 방향으로 나아갈 수 있게 해주는 표지판이에요. ",
                "삶은 용기의 크기에 따라 축소되거나 확장됩니다.",
                "습관적인 일상에서 한 걸음만 벗어나도 우리는 새로운 방향으로 나아가게 됩니다.",
                "자신을 사랑하게 되면 마주치는 모든 사람에게 그 사랑을 확장시키는 법을 배울 수 있어요.",

    ]
    
    if data.get('message').get('photo') is None:
        if user_msg == "점심메뉴":
            menu_list = ["삼계탕", "철판낙지덮밥", "물냉면",]
            result = random.choice(menu_list)

        elif user_msg == "로또":
            result = sorted(random.sample(list(range(1,46)),6))

        elif user_msg == "명언":
            result = random.choice(Fighting)

        elif user_msg[0:2] == "번역":
            raw_text = user_msg[3:]
            papago_url="https://openapi.naver.com/v1/papago/n2mt"
            data = {
                "source":"ko",
                "target":"en",
                "text":raw_text,
            }
            header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret,
            }
            res = requests.post(papago_url,data=data,headers=header)
            translate_res = res.json()
            translate_result = translate_res.get('message').get('result').get('translatedText')
            
            result = translate_result
            

        else:
            result = user_msg
    
    else:
        
        # 사용자가 보낸 사진을 찾는 과정
        # requests.get.json() -> in flask
        # request.get_json() -> in request
        # 둘의 차이가 있음 다른 메소드
        file_id = data.get('message').get('photo')[-1].get('file_id')
        file_url = f"{api_url}/bot{token}/getFile?file_id={file_id}"
        file_res = requests.get(file_url)
        file_path = file_res.json().get('result').get('file_path')
        file = f"{api_url}/file/bot{token}/{file_path}"
        
        # 사용자가 보낸 사진을 클로바로 전송
        # res -> 사진에 관한 데이터

        res = requests.get(file, stream = True)
        ## 네이버
        clova_url = "https://openapi.naver.com/v1/vision/celebrity"
        
        
        header = {
                'X-Naver-Client-Id':naver_id,
                'X-Naver-Client-Secret':naver_secret,
            }    
        clova_res = requests.post(clova_url, headers=header, files = {"image":res.raw.read()})
        
       
        
     
        
        if clova_res.json().get('info').get("faceCount"):
            # 누구랑 닮았는지 출력
            celebrity = clova_res.json().get('faces')[0].get('celebrity')
            name = celebrity.get('value')
            confidence = celebrity.get('confidence')
            result = f"{name}일 확률이 {confidence*100}%입니다."

        else:
            # 사람이 없음
            result = "사람이 없습니다."

    res_url = f"{api_url}/bot{token}/sendMessage?chat_id={user_id}&text={result}"
    requests.get(res_url)

    return '', 200

if __name__ == "__main__":
    app.run(debug=True)
```

