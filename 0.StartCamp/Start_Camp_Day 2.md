# Start Camp_Day 2

## 1. Python 개발 환경설정
> 프로그램을 설치할 때 반드시 읽어보면서 진행할 것
### 1-1. Python 3.7.3 설치

[Python Official Site](http://python.org ) 에서 Python 3.7.3 설치
> Add Path 체크 필수

### 1-2. Git 설치

[Git Official Site](https://gitforwindows.org/) 에서 Git bash 설치

```bash

ls    // 현재 위치의 디렉토리의 파일 명 출력 옵션 : -a
cd    // cd 위치 다음 위치로 이동 
rm 	  // rm 파일명 파일 제거 rm -r 폴더 제거 -r(recursive)
mkdir // mkdir 폴더명 폴더명으로 된 폴더 생성
mv	  // mv 파일명 이동할 위치 이동할 위치로 파일을 옮김
mv file.txt ../ // 상위 폴더로 이동
mv    // mv 파일명 바꿀파일명 -> 파일명을 바꿔줌
mv file.txt myfile.txt

```

### 1-3. VScode 설치

[VSCode Official Site](https://code.visualstudio.com/) 에서 VScode 설치

VSCODE 환경 설정

## 2. Python을 실생활에 이용하기

### 2-1. 내장함수 browser 함수 이용
> browser는 자주 쓰이지 않음
```python
import webbrowser
webbrowser.open("naver.com")
```
위 코드 실행 시 기본 브라우저로 naver가 실행이 됨
```python
import webbrowser
webbrowser.open("naver.com")
webbrowser.new_tab("naver.com")
```
위 코드 실행 시 탭에 추가가 되어야 하지만 왜인지 모르게 안됨

### 2-2. 외장함수 requests와 bs4 이용 kospi 정보 뽑기(웹 크롤링)
우선 외장함수를 쓰기 위해서는 외장함수를 다운로드 해야함
> Terminal 에서 칠 것
```bash
pip install requests
```
> Text Editor에서 칠 것
```python
import requests
response = requests.get("https://finance.naver.com/sise")
print(response.text)
```
코드 실행 시 터미널에 결과값이 나옴
결과값은 크롬에서 **F12**를 누르면 나오는 노트와 같음
즉 requests 는 사이트 접속 시 서버에서 주는 응답을 받음

이제 Python에서 가공하기 쉽게 만들어주는 함수 BeautifulSoup함수를 이용
**BeautifulSoup** 의 경우 bs4라는 함수 더미에 있는 함수
그래서 

```python
from bs4 import BeautifulSoup
```
이런식으로 선언해준다.

BeautifulSoup 함수를 이용해 데이터를 가공
`soup = BeautifulSoup(response,"html.parser")`
> soup라는 변수 안에 BeautifulSoup 함수의 결과값을 저장한다.
> BeautifulSoup를 사용할 때 불러들이는 응답의 문서가 어떤 건지 지정해주면 더 좋다.
> html, xml 등등
`soup.select(웹에서의 select data)`
> 웹에서의 select data의 경우 뽑아야할 데이터의 위치에서 마우스 오른쪽 우클릭 후
> 검사 항목을 클릭해 하이라이트 되어 있는 부분을 Copy 하는데
> Copy의 그 중 Copy select를 선택하여 웹에서의 select data 부분에 붙인다.
`soup.select_one()`
> select()와 select_one()의 차이는 괄호 안의 조건을 바탕으로 검색하는데
> select()의 경우 조건내의 결과 값을 다 부르지만 select_one()은 조건내의 결과 값의 맨 앞 부분을
> 불러들인다.

코스피를 뽑아주는 최종 코드
``` python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://finance.naver.com/sise/").text
soup = BeautifulSoup(response,"html.parser")
kospi = soup.select_one("#KOSPI_now")
print(kospi.text)
```

### 2-3. 위의 내용을 바탕으로 실시간 검색어 1위 정보 뽑기
```python
import requests
from bs4 import BeautifulSoup

response = requests.get("https://naver.com").text
soup = BeautifulSoup(response, "html.parser")
naver = soup.select_one("#PM_ID_ct > div.header > div.section_navbar > div.area_hotkeyword.PM_CL_realtimeKeyword_base > div.ah_list.PM_CL_realtimeKeyword_list_base > ul:nth-child(5) > li:nth-child(1) > a.ah_a > span.ah_k")

print(naver.text)
```
### 2-4. Python을 이용 파일 생성
> faker 함수도 Terminal을 이용해 미리 가져와야 원할한 실습이 가능
> In Terminal
> pip install faker
```python
from faker import Faker
import os

for i in range(100):
	# Faker 함수를 쓸 때 우선 언어를 어떤 것을 쓸지 결정함
    f = Faker("ko_KR")
    # 랜덤으로 만들어진 파일명을 가진 문자열 변수
    filename = f"{i}_{f.name()}.txt"
    # 명령 프롬프트에 입력할 명령어를 가진 문자열 변수
    cmd = f"touch {filename}"
	# os 함수를 이용해 명령어를 가진 문자열 변수를 넣어 명령 수행
    os.system(cmd)
```
> faker 함수의 경우 임의의 정보를 랜덤으로 만들어주는 함수
> 한글 버전도 지원해준다.
> `import os`의 경우 os 내에서 할수 있는 행동을 python에서 쓸 수 있도록 연결해줌


### 2-5. Python을 이용 파일 이름 변경
#### 간단한 상황 예제 1
어떤 폴더 내에 무수한 파일들이 있는데 파일 명 앞머리에 Samsung_ 을 추가하라
```python
# 사용할 함수 선언하고
import os
# 파일명을 바꿀 위치를 지정해주고
os.chdir(r"C:\Users\student\StartCamp\students")
# os.listdir(".")의 경우 현재 폴더내의 파일들을 리스트화 한것
# 리스트의 개수만큼 반복 시작
for filename in os.listdir("."):
	#os.rename을 통해 (예전 파일명, 새롭게 바꿀 파일명)을 선언해 바꾼다.
    os.rename(filename, "Samsung_+filename)
    
```
#### 간단한 상황 예제 2
알고보니 Samsung_ 이 아닌 SSAFY_ 바꿔야 한다면?
```python
import os

os.chdir(r"C:\Users\student\StartCamp\students")

for filename in os.listdir("."):
	# replace("SAMSUNG_", "SSAFY_") SAMSUNG_을 SSAFY로 바꿔줌
    os.rename(filename, filename.replace("SAMSUNG_","SSAFY_"))
```

### 2-6. 배운 것에서 한 발짝 더 멀리 
#### 조금 덜 간단한 상황 예제
환율 정보를 각 나라 별로 알고 싶어요 지금 까지 배운 것을 이용해 실습

```python
# 웹사이트의 응답을 받기 위한 함수 선언
import requests
# 받은 응답을 Python이 처리하기 쉽게 가공해주는 함수 선언
from bs4 import BeautifulSoup
# url의 경우 웹사이트 안의 웹사이트(iFrame??)을 가져와야 함
url = "https://finance.naver.com/marketindex/exchangeList.nhn"
# 웹사이트 응답을 response에 저장
response = requests.get(url).text
# Python이 보기 쉽게 가공
soup = BeautifulSoup(response,"html.parser")
# 이건 F12를 관찰한 결과 tbody에 있는 tr을 선택하는 것
# 추후 html을 깊게 배울 시 미래의 내가 수정해 주세요
tr = soup.select('tbody > tr')
for r in tr:
	#tit의 경우 title의 약자 즉 각 나라 이름
    print(r.select_one('.tit').text.strip())
    #sale은 환율이 얼마인지 알려줌
    print(r.select_one('.sale').text)
# 이건 내가 짠 코드
#for i in range(1,30):
#    kospi = soup.select_one(f"body > div > table > tbody > tr:nth-child({i}) > td.sale")
#    print(kospi.text)

```

## 3. 내 인생 첫 Git 및 GitHub 활용과정
> Git과 GitHub 사용이 익숙해지면 따로 정리하자
> GitHub는 이미 가입했어요

### 3-1. 처음 내 repo 올리기
	1. Git Bash 들어가기
	2. 내 프로젝트 폴더에 들어가서 git init 명령어 타이핑
	>  현재 위치 옆 부분에 (master) 이라고 적혔는지 확인할 것
	3. ls -a 입력 후 .git/ 생성되었는지 채크
	4. Visual Studio Code에 들어가서 .gitignore파일을 새로 생성
	5. https://gitignore.io/ 사이트 접속 후 프로젝트에 사용된 프로그램, 언어 찾기
	6. 모두 복사해 .gitignore 파일에 모두 붙여넣기
	7. Git Bash에 명령어 git add . 치기
	>  위의 명령어는 현재 폴더 내의 모든 파일을 추가할 것이다
	8. git commit -m "first commit" 이 올릴 파일에 커밋트를 추가하기
	9. 초기 설정 시 어디에 올릴지 모르기 때문에 commit 명령어에서 오류가 뜸
	10. git config --global user.email "내 GitHub 이메일 입력"
	11. git config --global user.name "내 GitHub 닉네임 입력"
	12. 위의 두 명령어를 Git Bash에 타이핑해 설정을 완료할 것
	13. 로그인하라는 팝업창이 뜰텐데 다 로그인해주세요
	14. 다시 git commit -m "first commit" 명령어로 커밋 추가
	15. GitHub 홈페이지에 오른쪽 상단 부분 플러스 기호 누르기
	16. new repository 항목을 클릭하고 Repository Name에 프로젝트명 넣기
	17. 자랑하고 싶으면 Public으로 공개하도록 하자 그 후 만들기 버튼 누르면
	18. 창이 뜨면서 git remote ~~~~ 라는 부분을 복사해 git bash에 붙여넣는다
	19. 그래서 git bash에 붙인 명령어를 실행 후
	20. 다시 git bash에 git push origin master 을 누르면 모든게 업로드됩니다.