---
title:  "AIB 데이터 엔지니어링 432 Flask"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, flask]
date: 2023-06-08 09:00
last_modified_at: 2023-06-08T09:00:00-09:00
---

- 정적 웹: 언제나 계속 똑같은 웹사이트를 보여줌.
- 동적 웹: php. html와 비슷함. 어딘가에서 데이터를 받아와서 웹페이지를 업데이트한다.
- 아이피: 고유주소. 전세계가 나누어 가져야 한다.

Flask/Django: 파이썬을 이용해서 웹 애플리케이션 작성을 도와준다.

Flask: 무언가를 만들어낼 수 있는 최소한의 도구 모음. micro web framework. 패키지 모듈 컬렉션 등 추가하면 개발에 수월. directory 안에 flask 관련 파이썬 코드 작성하고 `FLASK_APP={dirname} flask run`
- `@app.route('{endpoint}')`로 해당 위치/라우트에서 어떤 결과를 `return`할지 입력할 수 있다. 응답코드도 같이 return 가능. 기본적으로 문자열, 튜플, 딕셔너리 반환. 특히 튜플은 response 코드 같이 return해주는 용도로만 사용. 나머지 오류가 나는 부분은 str로 변환
  - 여기에 기본 적용되는 HTTP Request  메소드(GET, HEAD, OPTIONS) 외에 추가할 수 있다: `@app.route('/', methods=[i for i in methods]` 그리고 이렇게 하면 다른 메서드는 사용할 수 없다.
  - decorator에 `defaults={'arg': 0}` 등으로 기본값을 정해줄 수 있음.
  - 블루프린트를 이용해서 라우트를 기능별로 나누어줌.
    - bp file: `bp = Blueprint('{name}', __name__, url_prefix='/{name}')`
    - `__init__.py`: `app.register_blueprint(bpfile.bp)`
- 파일 여러개일 때 앱을 동시에 사용하거나 하는 문제나 파일 일부분만 필요할 때 오류가 발생할 수 있다고 한다. `if __name__ == '__main__': 본문` 등으로 하면 해결. 본문이라고 표기했지만 대부분 구성은 함수나 클래스 썼던 것을 호출하는 위치이다.
- html 파일 불러오기. 기본적으로 `templates`라는 폴더를 기본 경로로 설정.
  ```python
  from flask import render_template
  
  @app.route('{dir}')
  def index():
    return render_template('my_index.html')
  ```
  - 만약 다른 위치라면 `'my_index.html'` 대신 `{otherdir}/my_other_index.html'`.
  - jinja: 웹 템플릿 엔진. 맞춤형 웹 페이지 자동 생성을 도와준다고 함.
    - `{{...}}`: 템플릿 결과에 출력할 `표현`
    - <code>{&#37;...&#37;}</code>: 구문 such as `if`, `for`
      ```html
      {% if True %}
      <h1>It is True</h1>
      {% endif %}
      ```
    - `{#...#}`: 주석
    - html 안에 위와 같이 입력하고 해당 변수 이름을 `render_template` 안에 `.html`과 함께 `**kwargs`로 입력. [공식문서](https://jinja.palletsprojects.com/en/2.11.x/templates/#variables)
- jinja 상속: 반복적으로 사용할 부분 하나로 묶기
  - <code>{&#37; extends &#37;}</code>, <code>{&#37; block something &#37;}...{&#37; endblock &#37;}</code>
  - <code>{&#37; extends "parent.html" &#37;}</code> 첫 줄에 적어준다.
  - <code>{&#37; block &#37;}</code>은 parent file안에 있는 형식을 부분적으로 가져온다
  - 마찬가지로 templates 폴더가 default

bootstrap
- 최소한의 지식으로 다양하게 예쁘게 꾸밀 수 있도록 도와주는 도구. 이미 스타일링 된 내용을 가져오는 구조.
- `<head>` 태그 안에 넣으면 작동한다.
- https://getbootstrap.com/

## 오전 세션
- django 장단점
  - 무겁지만 기능이 많이 들어있다
- flask 장단점
  - 가볍다. 이것저것 추가할 수 있음: 자율성.
- `__name__`: 파일 이름 저장하는 것
- `app.route()`: endpoint 해당하는 경로 연결
- ip:포트/라우트 - 주소:호수/방

## 오후
- uri 형태들
  - /user?username
  - /user/name
  - body에 json