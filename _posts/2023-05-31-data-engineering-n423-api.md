---
title:  "AIB 데이터 엔지니어링 423 API"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, api]
date: 2023-05-31 09:00
last_modified_at: 2023-05-31T09:00:00-09:00
---

API: how a program interacts with something. application programming interface. interface: 추상적인 단계에서 이야기를 할 때 사용되는 용어. 시스템 단계에서의 시스템의 입력값과 출력값의 모음. API는 앱 단계에서.
- 클라이언트: 무언가를 요청함. 결과를 받아올 수 있도록 API를 통해 요청.
- API Server: 클라이언트로부터 요청을 받아 Service Server로 전달하고 여기서 반환된 결과를 클라이언트에게 전달해준다. 사용하는 이유: 클라이언트와 서비스 서버를 직접 연결시킨다면 중간에 변환해야 되는 내용 등이 너무 많을 것이므로 간단하게 처리해주는 중간 단계가 있어서 매우 편리하다.
- Service Server: API 서버로부터 받은 요청을 처리해주는 서비스를 구동한다.

json: 보통 API 데이터를 받아올 때의 형식은 json이다. 그 외에도 csv, 문자열, xml 등이 있긴 하지만 특히 Web API는 JSON을 많이 사용한다.
- javascript object notation. javascript의 object. Dictionary와 비슷하게 생겼다.

HTTP: HyperText Transfer Protocol. 컴퓨터 간 소통할 때의 규약 중 하나. 웹에서 통신할 때 사용되는 규약. (이메일에 관련된 작업의 규약은 POP3, SMTP, IMAP) 프로토콜은 API보다는 더 자세한 기술적인 내용으로 구성되어 있다. 알고리즘, 데이터형식, 계층구조 등. HTTP API는 Web API해 해당함.
- HTTP Request: 한 컴퓨터가 다른 컴퓨터에 리소스 요청. 여러 메소드가 있는데, CRUD
  - 메소드
    - GET: 클라이언트가 서버에게 특정 리소스를 달라고 하는 요청. 페이지 로딩 등. READ
    - POST: 클라이언트에서 서버에게 많은 정보를 전달. 회원가입 정보 등. CREATE
    - PUT/PATCH: 클라이언트가 서버의 특정 리소스를 업데이트. PUT은 전체, PATCH는 부분. UPDATE
    - DELETE: 클라이언트가 서버에게 특정 리소스 삭제 요청. DELETE
  - REST 가이드라인에 따라 메소드들을 사용하는 것이 적합함
- HTTP Response: 요청에 응답하기. 상태 코드는 100번대에서 500번대까지가 있다.
  - 100: 정보 응답
  - 200: 성공 응답
  - 300: 리디렉션 메시지
  - 400: 클라이언트 에러 응답
  - 500: 서버 에러 응답

F12 눌러서 네트워크 탭에 들어가면 HTTP API에 대해 여러 정보를 확인할 수 있다.

Rest API: 요청 모습 자체로 어떤 동작인지 알 수 있는 것. 주소 만으로도 무엇인지 알 수 있도록. 혼자만 사용하는 것이 아니고, 또 혼자 개발한다 하더라도 시간이 오래 걸리기 때문에 쉽게 알아볼 수 있는 형태로 구성하는 것이 중요하다.
- Representational State of Transfer. 소프트웨어의 아키텍쳐를 어떻게 형성할 지에 대한 가이드라인. 이 가이드라인을 참고하여 API를 만들면 REST API라고 부른다. 6개의 모든 원칙을 지키면 RESTful API
- 앞서 언급했던 메소드나 Response는 REST 가이드라인에 따라서 그 기능을 수행하는 것이 대부분이지만, 꼭 그렇게 지키지 않은 API들도 있긴 있다. 하지만 강력한 권고사항임.

각 API마다 작동 방식 찾아보기. endpoint ? 뒤에 사용할 parameter에 대한 설명이 있다. 필수 제공해야 하는 정보 확인해보기.

웹스크레이핑 vs. API: API는 유저에게 필요한 부분을 잘 정리해서 요청할 방법도 안내되어 있음. 반면 웹스크레이핑은 추가적으로 작업해주어야 하는 내용이 있는 반면에 자유도가 높음.

POSTMAN: 프로그램. 이 프로그램으로 데이터 쉽게 요청할 수 있다.

### 추가 학습내용
- [ ] 로드 밸런싱
- [x] REST API 세부내용
- [ ] Web API로 데이터 적재
- [x] github API: PyGithub로 Github API 파이썬에서 사용 가능. 내가 이해한게 맞다면 PyGithub으로 Github API와 소통하고 Github API로 Github와 소통한다.
- [x] 공개키 암호화 방식
- [ ] OAuth 2.0 방식

### REST API 성숙도
4 단계로 나누어볼 수 있다.
- 0: HTTP 사용할 수만 있다면 0단계이다. 어떠한 엔드포인트를 사용하든지, 응답으로 무엇을 받든지간에 통신이 이루어지면 0단계
- 1: 개별 리소스와의 통신 준수. 개별 리소스에 맞는 엔드포인트 사용할 것과 서버는 받은 정보를 응답으로 전달해야 한다.
  - 예: 병원 예약 관련해서 리소스를 조회할 때 특정 날짜에 `허준`이라는 의사의 예약 가능한 시간을 알아보고자 한다면 `/doctors/허준`이라는 endpoint를 사용하고 날짜 정보를 입력할 수 있고, 실제 예약할 때 `slots/123`라는 endpoint를 통해 `id=123` 시간대에 예약하는 것을 생각해볼 수 있다. 예약을 성공한다면 예약 정보와 함께 성공 여부를 알려주고, 실패한다면 예약 정보와 함께 실패사유 또한 알려줄 것.
- 2: CRUD를 준수할 것
  - 예: POST를 통해 조회와 예약 모두 할 것이 아니라 GET으로 예약 가능 시간 알아보고 POST로 예약할 것. GET은 body 정보가 없기 때문에 URI에 query parameter를 사용해서 리소스를 전달해야 한다. POST 예약에 대한 응답으로는 성공하면 `201` 응답 정보를 반환해야 한다.
- 3: HATEOAS Hypertext as the Engine of Application State. 응답에 리소스의 URI를 포함한 링크 요소를 같이 반환하여 이 응답으로부터 어떤 액션을 취할 수 있는지를 확인 가능하다.
  - 예: 예약 시간 조회 후에 그 시간대에 예약할 수 있는 링크를 삽입하고, 예약 후에는 그 예약을 조회하거나 변경/삭제할 수 있는 링크를 포함할 수 있을 것이다.

이 4 단계 안에 RESTful API req 6개 중 uniform interface는 포함되어 있다고 이해된다. 나머지 5개에 대한 내용도 나중에 살펴보기.
- https://restfulapi.net/rest-api-design-tutorial-with-example/
- https://restfulapi.net/rest-architectural-constraints/

### 공개키 암호화
보안 정보에 사용자의 키를 사용해서 암호화하고 복화할텐데, 여기에 두 가지 키를 사용한다. 공개키는 정보를 암호화하고 개인키는 암호화된 정보를 복구한다.

사용자의 공개키는 모두에게 공개되어있고, 사용자의 개인키는 개인에게 귀속되어 있다. 이 둘이 한 쌍이다. 이것을 활용하는 방법은 특정 사용자에게 해당하는 정보를 줄 때 공개키로 암호화하여 보낸다면 다른 어떤 사람도 이 암호화된 정보를 복구할 수 없어야 하고, 이 특정 사용자만 자신의 개인키로 복구하여 그 정보를 확인할 수 있어야 한다.

### 추가
머신러닝/딥러닝 모델도 API로 제공되는 경우가 많음

모델 학습기간 매우 김. 에러 언제 날 지 모를 수도 있다. 그래서 epoch 하나 끝날때마다 notify하는 notification API를 사용할 수 있다. `Slack` - `WebHooks`

[무료 APIs](https://github.com/public-apis/public-apis)