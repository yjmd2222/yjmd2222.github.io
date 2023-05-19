---
title:  "AIB 데이터 엔지니어링"
excerpt: "개발 환경 구축 시작"
header:
  # teaser:
categories: [AIB_Section_4]
tags: [AIB, data_engineering]
date: 2023-05-19 11:00
last_modified_at: 2023-05-19T11:00:00-09:00
---

개발 환경 구축을 시작했다. 예전에 코랩 램 초과가 생겨서 VSCode를 로컬에서 돌린 적이 있다. 그래서 VSCode는 괜찮은데 Anaconda를 설치하려니까 애 먹음. 컴퓨터 시스템에 설치하면 Anaconda 경로가 `C:\ProgramData`로 들어가서 user 아래에 있지 않다 보니 `source`에 문제가 있었다. 혹시나 잘 모르는데 다른 명령어들이 이 경로에서 실행될까? 하는 마음에 삭제하고 다시 설치했는데, 지금 다시 생각해보면 source를 하는 순간 상대경로에서 실행이 가능하기 때문에 재설치할 필요는 없었겠다. 재설치하니 다시 시작해야 하고, 다시 시작해도 이전 설치의 찌꺼기가 남아서 처음 실행에 오류가 있었다. 여러 번 Navigator와 git bash를 껐다 켜고 명령어를 입력하다 보니 잘 진행 됨. **이래서 주의사항 잘 읽어봐야 한다.**
