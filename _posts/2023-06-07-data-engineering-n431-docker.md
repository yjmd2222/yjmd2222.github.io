---
title:  "AIB 데이터 엔지니어링 431 Docker"
excerpt:
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering, docker]
date: 2023-06-07 09:00
last_modified_at: 2023-06-07T09:00:00-09:00
---

# 추가학습
- [ ] Dockerfile
- [ ] Docker network
- [ ] docker-linux 컨테이너 관계
- [ ] Kubernates
- [ ] 가상머신
- [ ] docker inspect
- [ ] docker lifecycle

서버. 특별한 역할이 있는 컴퓨터이다. 서비스를 제공. 인터넷에 연결되어 있어서 어떤 기능이 필요하면 클라이언트로서 다른 서버에 정보를 요청할 수 있다. 보통 클라우드에 올려놓음

IP: 네트워크 끝단의 주소. 이동하면 바뀔 수 있다. IPv4: 0~255. 46억개 조합할 수 있는데, 부족함. 공인과 사설로 나누어 공인 내에서 기기마다 사설 아이피를 부여해보기. 사설 아이피만 있다면 서버로 구축할 수 없다. 외부에서 접근해오기 위해 공인아이피만 쓰거나 포트포워딩/DMZ 사용해야 한다. 고정, 유동아이피가 있다. 서버는 바뀌지 않도록 고정. 인터넷에 사용중인 아이피만 할당해주는 유동아이피. 해킹에 안전할 수 있는 부분도 있다. DDNS 동적 DNS. 아이피 고정시키기. IPv6 16진수 4자리 8개로 이어짐.

Docker: 키보드 명령어로 작동. 무거운 일을 대싱 수행한다는 의미의 단어. 리눅스에서 돌아가는 프로그램을 다른 PC에서도 수행할 수 있다.
- 미리 제작된 개발 환경을 제공하여 호환성의 고생을 덜어준다. 쉽다는 것: Dockerfile. 설명서를 제공
- 용량이 가볍고 빠름: Container. 리눅스 컨테이너를 본따서 만든 컨테이너.
- 쉽게 삭제하고 복구할 수 있다. 개발 환경을 여러 개 만들어서 사용하고 배포하고 수정하기 때문에 테스트가 쉽다.

Container
- 리눅스 container는 리눅스에서 필요한 라이브러리나 앱을 모아서 별도의 서버처럼 구성한 것. 컨테이너가 각각의 컨테이너에 대한 네트워크, 환경 변수 등의 시스템 자원을 독립적으로 소유하고 있다.
  - 프로세스 구획화: 기본적으로 특정 컨테이너 안에서만 작업하고 액세스하고, 다른 컨테이너의 프로세스에 간섭할 수 없다.
  - 네트워크 구획화: IP 주소 한 개가 할당되어 있다.
  - 파일시스템 구획화: 컨테이너에서의 명령이나 파일 액세스를 제한할 수 있다.
  - 즉 가상머신이랑 비슷하다.
- 도커 container: 인프라 / 프로그램을 어떤 환경에서나 실행 가능하도록 만들어주는 개체. 하나의 컨테이너 안에 필요한 부분을 다 넣어서 가져온다. 
  - 가상머신: 하나의 OS 안에 또 다른 OS를 만드는 것. 하나의 OS를 유지하기 위해 자원 너무 사용하게 되어 효율성과 속도가 느리다.

도커 구성
- Registry_acc/Repo_name:Tag
  - 레지스트리: 도커 이미지 관리되는 공간. 깃허브에서 깃을 허브에서 관리하는 것처럼 도커 이미지를 허브에서 관리.
  - 리포: 도커 이미지 저장되는 공간. 깃허브 리포와 비슷한 개념
  - 태그: 버전별로 이미지가 다를 수 있다는 부분. default `latest`
- 간단한 명령어들
  - `docker image pull`: 이미지 가져오기
  - `docker image ls`: 이미지 확인
  - `docker container run`: run new container from image
  - `docker container ls`: container list
  - `docker container rm`: rm container
  - `docker image rm`: rm image
  - argument `-it`: interaction
  - `-p 818:80` 호스트 818포트와 container의 80포트를 연결
  - `docker container prune`: active 아닌 container 삭제

프로그램과 프로세스, 도커허브로 설명을 하자면 도커허브 안에 도커 이미지가 있어서 이를 로컬로 가져오면 프로그램, 실행 중인 프로세스로서의 컨테이너

서버 활용
- 서버는 컨테이너로 사용하고 필요한 파일은 별도로 두는 방법이 있다. 이 방법으로 서버의 문제를 별도로 관리하고 새로운 서버 재구동에 용이하다.
- `docker container cp` 등으로 가능.
  `cp` 대신 `scp` 서버와 로컬 간 빠름. 예: `scp /home/example.txt dhj@141.211.xx.xxx:/home/test`

이미지 만들기
- 이미지로 commit `docker container commit [from] [to:version]`
- 빌드파일 만들기. 설명서를 통해 이미지 만들어서 사용하는 방법 안내. `Dockerfile`
  - `docker build --tag [DOCKERNAME]`

yaml: config. docker-compose에서 yml 사용하는 이유는 둘을 연결할 때 많은 과정이 필요함. yml이 있다면 쉽게 구성할 수 있다.