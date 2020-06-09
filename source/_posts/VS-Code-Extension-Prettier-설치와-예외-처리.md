---
title: VS Code Extension Prettier 설치와 예외 처리
disqusId: tunas-blog-1
tags:
  - VS Code Plugin
  - VS Code Extension
  - Prettier
  - Prettier 예외 처리

toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
  - type: adsense
    position: right
sidebar:
  right:
    sticky: true
date: 2020-05-21 21:13:56
categories: VS Code
---

### Prettier 란?

VS Code Extension 중의 하나인 Prettier는 코드 스타일을 자동으로 정리해주는 도구입니다.

들여쓰기(공백 문자), 세미콜론(;)이 빠진곳, 작은따옴표('')등을 자동으로 추가 및 변경해줍니다.

<!-- more -->

------
### Prettier 설치

VS Code 마켓플레이스에서 Prettier를 검색하여 설치해 줍니다.

![Prettier](/images/Prettier.png)

설치 후 정리를 원하는 파일에서 F1을 누르고 format을 입력한다음 엔터를 누르면 Prettier가 자동으로 코드를 정리해 줍니다.

------
#### 저장할 때 자동으로 코드 정리하기

매번 F1을 눌러 format을 입력하거나 단축키를 이용해 정리하는 것보다 편한 저장할 때 정리하게 만드는 법이 있습니다.

VS Code의 설정에 들어가 format on save를 검색하여 체크 박스를 체크해주면 됩니다.

![Prettier 저장할 때 자동으로 코드 정리](/images/Prettier_OnSave.png)

------
### Prettier 예외 설정

편하게 사용하려고 format on save 설정을 켜놨더니 원치 않은 경우에도 적용되어 곤란해졌습니다.

저의 경우에는 블로그 작성시 사용하는 MarkDown(.md)파일들의 공백이 원치 않게 변경되어 예외 처리가 필요해졌습니다.

* 예외 처리

예외 처리할 폴더의 root 폴더에 .prettierignore 파일을 만들어 줍니다.
저의 경우 `..\..\myblog\source\_posts` 경로와 `..\..\myblog\scaffolds` 경로의 .md 파일들을 예외 처리 하기위해 myblog 내부에 .prettierignore 파일을 만들었습니다.

.prettierignore 파일 안에 *.md를 작성하면 끝입니다.

![Prettier MarkDown예외 설정](/images/prettierignore.png)

이제 더이상 Prettier가 MarkDown 파일을 변경하지 않습니다.