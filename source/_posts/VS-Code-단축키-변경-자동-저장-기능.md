---
title: 'VS Code 단축키 변경, 자동 저장 기능'

tags:
  - VS code 단축키
  - VS code 자동 저장 기능

toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right

sidebar:
  right:
    sticky: true
date: 2020-05-21 20:00:28
categories: VS Code
---

VS Code의 제안 기능? (Ctrl +Space) 단축키가 불편하여 변경했다.

추가로 자동 저장 기능도 설정한다.

<!-- more -->

------
### 단축키 설정

단축키 설정은 ![VS Code 단축키 설정](/images/단축키.png)

VS Code 에디터 좌측 하단 톱니바퀴를 누르거나 Ctrl+k Ctrl+S로 들어갈 수 있다.

내가 변경 하고자한 단축키의 이름은 제안 항목 트리거 라고 한다..

![제안 항목 트리거 단축키 변경](/images/제안항목트리거.png)


단축키 부분을 더블 클릭하여 기존의 Ctrl + Space를 Shift + 2로 변경해 줬다.
새끼 손가락이 덜아파 행복하다.

~~특수문자 @를 쓸때마다 아주 곤란하다... 하지만 자주 쓸일이 없으니 대충 쓴다.~~

------
### VS Code 자동 저장 기능

한번 설정해 놓으면 아주 편한 자동 저장 기능

이제 매번 Ctrl + S로 저장해줄 필요가 없다.

마찬가지로 VS Code 에디터에서 톱니바퀴를 누르고 이번에는 `설정`으로 들어간다.

![AutoSave](/images/autoSave.png)

* 일반적으로 사용되는 설정 > onFocusChange 항목에서 원하는 항목으로 설정한다.
 ~~개인적으로 onFocusChage가 편해서 사용중~~

* Auto Save Delay
  afterDelay로 설정시 지연시간 (ms) 설정