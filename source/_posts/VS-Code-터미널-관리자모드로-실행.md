---
title: VS Code 터미널 관리자모드로 실행

tags:
  - VS Code
  - VS Code 터미널 관리자모드
  - 이 시스템에서 스크립트를 실행할 수 없으므로

toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right

sidebar:
  right:
    sticky: true
date: 2020-05-21 20:21:07
categories: VS Code
---

### 이 시스템에서 스크립트를 실행할 수 없으므로 해결법

VSCode 의 터미널(Terminal)로 Hexo server등을 실행하면 
이 시스템에서 스크립트를 실행할 수 없으므로 어쩌구 저쩌구 오류가 나온다.

1. windows PowerShell 프로그램을 관리자 권한으로 실행한다.


2.  Set-ExecutionPolicy RemoteSigned 입력후 엔터

![스크립트 실행 규칙 변경](/images/powerShell.png)

3. y 엔터

이제 VS Code로 돌아가 터미널에서 실행해보면 잘 작동한다.