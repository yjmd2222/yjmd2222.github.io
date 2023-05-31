---
title:  "AIB 데이터 엔지니어링 422 웹스크레이핑"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, 웹스크레이핑]
date: 2023-05-30 09:00
last_modified_at: 2023-05-30T09:00:00-09:00
---

HTML hypertext markup language. 웹브라우저상 보여지도록 디자인된 문서. markup: annotation으로 문서에서 일반적인 텍스트와 구분된 것들로, 태그로 구조적으로 작성됨

HTML 구조. jsbin.com에서 웹페이지 자동적으로 빌드해줌.
- head는 보여주는 부분에 없고 디자인 등 관련 메타데이터.
- body 사용자에게 보여지는 부분

이걸 구성하면 알아서 웹페이지가 보여진다.

태그 종류 보고 싶으면 MDN html 검색해보기. element reference. 대부분 웹사이트에서 지원되기는 하지만 그래도 호환되지 않은 것들도 있으니 확인 필요하다. 에러 발생하면 스스로 회복해서 원래 컨텐츠를 보여주기도 한다. validator에서 에러 확인 가능

body 안에도 구조를 나눈다. header, footer, main body 등. 웹사이트를 보면 박스로 구분된 것으로 생각해볼 수도 있다. css, React사용할 때 중요.

html css js. 갖다놓고 꾸미고 시킨다.

### 노트
웹스크레이핑: 필요한 데이터를 인터넷에서 모아오는 것.

HTML은 엄밀히 말하면 (MDN에 따라) 프로그래밍 언어는 아니다. 웹페이지가 어떻게 구성되어서 보여지는지에 대한 방식을 알려주는 알려주는 마크업 언어. 웹페이지 구성 형태.

여러 요소로 웹페이지를 구성할 때 각 요소는 태그로 표현이 된다. `head`를 예제로
- opening tag: `<head>`
- closing tag: `</head>`

모든 엘리먼트가 closing tag가 있는 것은 아님. 또한 하나의 요소 안에 다른 요소도 넣을 수 있다.

CSS: Cascading Style Sheets. HTML의 웹페이지가 어떻게 표현되는지에 대한 스타일시트. HTML 내에서 표현하는 것보다 다른 파일로 분리해서 표현하는 것이 구분되고, 또 양도 많아서 편하다. CSS selector는 특정 요소를 선택하는 방법이다. Type selector, Class selector, Id selector.
- 상속: 요소 안에 요소가 있으면 바깥 요소의 스타일을 상속 받는다. 하지만 안의 요소가 이미 스타일이 적용되어 있다면 그 스타일은 그대로 간다.
  ```html
  <div style="color:red">
    <p>no style by itself</p>
  </div>
  ```
- 클래스: 여러 요소의 스타일을 적용하고 싶을 때 클래스를 생성하여 그 클래스에 해당하는 요소들이 동일한 스타일을 상속받도록 한다.
  ```html
  <!--HTML-->
  <p class="banana">Banana class</p>
  ```
  ```css
  /* CSS */
  .banana {
    color:"yellow";
  }
  ```
`class="one two three"`처럼 여러 클래스를 동시에 부여할 수 있다.
- id: 스타일 적용하는 다른 방법. 클래스와 약간 비슷하면서도 다르다. 동시에 여러 개의 요소에 적용하기 보다는 특정 요소를 가리킬 때 사용.
  ```html
  <!--HTML-->
  <p id="pink">Pink</p>
  ```
  ```css
  /* CSS */
  #pink {
    color:"pink";
  }
  ```

inline, internal, external: CSS 위치
- inline: 태그 안에다가 넣기
- internal: head 안에 적용하기. `<style> </style>`
- external: 별도 외부 파일로.
  `<link rel="stylesheet" type="text/css" href="style.css">`

이렇게 CSS 배우는 이유는 찾고자 하는 데이터가 어떤 CSS를 사용했는지를 통해 가져오려고.

F12 눌러보기. html, css 확인 가능. console에서 `document.querySelector(...)`로 query 가능.

DOM: 문서 객체 모델 Document Object Model. 프로그래밍 언어로 HTML 문서에 접근. 글을 object로 취급. F12로 콘솔 창에 들어가면 자바스크립트를 통해 이를 사용해볼 수 있다.
- `document.querySelectorAll('p)`: p 요소를 담은 유사 배열을 받게 된다. 형태는 NodeList.
- 다른 것들은 다른 형태로 가져올 수도 있다.

크롤링에서 DOM이 중요한 이유는 하나의 문자열이 아니라 필요한 텍스트를 구분해낼 수 있어야 하기 때문이다.

스크레이핑과 크롤링 차이: 자동화와 인덱싱. 스크레이핑은 특정 정보를 가져오는 것에 초점이 맞추어져 있고, 크롤링은 사이트들을 인덱싱하는 것에 초점이 맞추어져 있다.

파이썬에서 소통을 위해 `requests` 라이브러리를 사용한다. 간단한 메소드를 통해 요청을 쉽게 실행할 수 있는 장점이 있다. `pip install requests`

```python
import requests

resp = requests.get('https://google.com') # get: 무언가를 가져올 때. return으로 Response
print(resp)
```
```python
<Response [200]>
```
Status code: 200이라면 OK. 성공적으로 접속. `resp.status_code`로 확인. 여러 다른 코드들이 있으므로 `raise_for_status()`를 통해 접속 불가라면 에러를 일으키는 것이 좋다.
```python
import requests
from requests.exceptions import HTTPError

url = 'https://google.com'

try:
    resp = requests.get(url)

    resp.raise_for_status()
except HTTPError as Err:
    print('HTTP 에러가 발생했습니다.')
except Exception as Err:
    print('다른 에러가 발생했습니다.')
else:
    print('성공')
```

실제로 응답을 받아오면 `text` 속성이 존재한다. 이것을 human-readable format으로 decode 해주어야 함. 이 정보를 얻어내기 위해 `BeautifulSoup` 사용. `pip install beautifulsoup4`. 기본적으로 `'html.parser'`로 파싱한다. html문서에 대해서 작업한다는 뜻.
```python
import requests
from bs4 import BeautifulSoup

resp = requests.get('https://google.com')

soup = BeautifulSoup(resp.content, 'html.parser')

dog_element = soup.find(id='dog')  # id는 한 번만 사용하는 것으로 설명했었으므로 find가 적합함.
cat-elements = soup.find_all(class_='cat') # class는 여러 번 사용되므로 find_all로 보는 것이 적절할 수 있다.

# 검색 결과 내 세부검색
for cat_el in cat_elements:
  cat_el.find(class_='fish')
```
기타 적용 방법
- 태그 `soup.find_all('div', class_='cat')`
- string: 문자열 내 string을 포함하고 있는지 `soup.find_all(string='raining')` 문자열을 반환한다.
  - 그대로 인식하는 것보다 pattern 찾는 것이 용이함. 예: `soup.find_all(string=lambda text: 'raining' in text.lower())`
  - 요소로 반환: `soup.find_all('h3', string='raining')`

요소를 찾았으면 내부 글을 얻어볼 수 있다.
```python
cat_el = soup.find('p', class_='cat')

cat_el.text # 문자열. str의 method 적용 가능
```

## 추가 학습할 내용
- [x] 정적/동적 스크레이핑
- [ ] 스크레이핑 결과 데이터베이스에 적재
- [ ] 콘솔에서 추출할 데이터 패턴 출력해보기
- [ ] 셀레니움
- [ ] 정규식 활용하여 문자열 찾기
- [ ] 클래스, 함수, 데코레이터 적용해서 코드 짜보기

## 정적/동적 스크레이핑
고정되어 있어서 소스가 변하지 않는 페이지는 정적 페이지. 무언가를 클릭한다면 url이 변하고 내용이 바뀐다. 동적페이지는 페이지 내에서 어떤 것을 수행하면 내용 내에서 무언가가 실행/바뀐다.

# 셀리니움
마우스나 키보드와 같은 행동. 검색 등 동적 정보 가져오기.

### 참고자료
- https://available-stem-0cc.notion.site/CodeStates-b64f0788d8cf4151ab12f9e0dbd314d8