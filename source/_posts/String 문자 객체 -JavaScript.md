---
title: String(문자) 객체 -JavaScript
date: 2020-03-02 12:33:36
disqusId: tunas-blog-1
categories: JavaScript
tag: 
  - JavaScript
  - String
  - String method


toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
  - type: tags
    position: right
  - type: adsense
    position: right
sidebar:
  right:
    sticky: true
---


* * *

#### 문자 객체 메서드 및 속성

| 종류                           | 설명                                                                                                                             |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| charAt(index)                  | 문자열에서 인덱스 번호에 해당하는 문자를 반환                                                                                    |
| indexOf(“찾을 문자”)           | 문자열 왼쪽부터 찾을 문자와 일치하는 문자를 찾아 최초로 일치하는 문자의 **인덱스 번호**를 반환                                   |
| lastindexOf(“찾을 문자”)       | 문자열 오른쪽부터 찾을 문자와 일치하는 문자를 찾아 최초로 일치하는 문자의 인덱스 번호를 반환 (만일 찾을 문자가 없으면 -1을 반환) |
| match(“찾을 문자”)             | 문자열 왼쪽부터 찾을 문자와 일치하는 문자를 찾아 최초로 찾은 **문자**를 반환 (찾는 문자가 없으면 null 반환)                      |
| replace(“바꿀 문자”,”새 문자”) | 문자열 왼쪽부터 바꿀 문자와 일치하는 문자를 찾아 최초로 찾은 문자를 **치환**                                                     |
| search(“찾을 문자”)            | 문자열 왼쪽부터 찾을 문자와 일치하는 문자를 찾아 최초로 일치하는 인덱스 번호 반환                                                |
| slice(a,b)                     | **a번째 까지 문자를 자르고 b번째 이후에 문자를 자른후** 남은 문자를 반환                                                         |
| substring(a,b)                 | **a 인덱스부터 b 인덱스 이전 구간**의 문자를 반환                                                                                |
| substr(a,문자 갯수)            | 문자열에 **a인덱스 부터 지정된 문자 개수**만큼 문자열을 반환                                                                     |
| split(“문자”)                  | **지정한 문자를 기준으로 문자 데이터를 나누어 배열에 저장**하여 반환                                                             |
| toLowerCase()                  | 문자열에서 **영문 대문자를 모두 소문자**로 바꿉니다.                                                                             |
| toUpperCase()                  | 문자열에서 **영문 소문자를 모두 대문자**로 바꿉니다.                                                                             |
| length                         | 문자열에서 **문자의 총 개수**를 반환합니다.                                                                                      |
| concat(“새로운 문자”)          | 문자열에 새로운 **문자열을 결합**합니다.                                                                                         |
| charCodeAt(“찾을 문자”)        | 찾을 문자의 아스키 코드 값을 반환                                                                                                |
| fromCartCode(아스키 코드 값)   | 아스키 코드 값에 해당하는 문자를 반환                                                                                            |
| trim()                         | 문자의 앞 또는 뒤에 공백 문자열을 삭제합니다.                                                                                    |

<!-- more -->

사용 예제

```js
var t="Hello Thank you good luck to you";  
  
document.write(t.charAt(16),"<br />");   
// 인덱스 16에 저장된 문자를 불러옵니다 (g)  
  
document.write(t.indexOf("you"),"<br />");   
// 문자열 왼쪽부터 최초로 발견된 "you"의 인덱스 값을 반환 (12)  
  
document.write(t.indexOf("you",16),"<br />");  
// 문자열 인덱스 16 위치부터 최초로 발견된 "you"의 인덱스 값 반환 (29)  
  
document.write(t.lastIndexOf("you"),"<br />");  
// 문자열 오른쪽 부터 왼쪽 방향으로 최초로 발견된 "you"의 인덱스 값 반환 (29)  
  
document.write(t.lastIndexOf("you",25),"<br />");  
// 문자열 인덱스 25부터 왼쪽 방향으로 최초로 발견된 "you"의 인덱스 값 반환 (12)  
  
document.write(t.match("luck"),"<br />");  
// 문자열 왼쪽부터 최초로 발견된 "luck"과 일치하는 문자를 찾아 반환 (luck)  
  
document.write(t.search("you"),"<br />");  
// 문자열 왼쪽부터 최초로 발견된 "you"의 인덱스 값 반환 (luck)  
  
document.write(t.substr(21,4),"<br />");  
// 문자열 인덱스 21부터 네 글자를 가져옵니다 (luck)  
  
document.write(t.substring(6,12),"<br />");  
// 문자열 인덱스 6부터 12 이전까지 문자를 가져옵니다. (Thank_)  
  
document.write(t.replace("you","me"),"<br />");  
// 문자열 왼쪽부터 최초에 발견된 "you"를 "me"로 치환 (Hello Thank me good luck to you)  
  
document.write(t.toLowerCase(),"<br />");  
// 문자열의 영문자를 모두 소문자로 바꿉니다. (hello thank you good luck to you)  
  
document.write(t.toUpperCase(),"<br />");  
// 문자열의 영문자를 모두 대문자로 바꿉니다. (HELLO THANK YOU GOOD LUCK TO YOU)  
  
document.write(t.length,"<br />");  
// 문자열의 총 문자 개수를 반환합니다. (공백포함 32)  
  
var s=t.split(" ");  
//  " "(공백) 문자를 기준으로 문자를 분리하여 s에 저장합니다.  
  
document.write(s[0],"<br />");  
// s 인덱스 0에 저장된 문자열을 출력합니다. (Hello)  
  
document.write(s[4],"<br />");  
// s 인덱스 4에 저장된 문자열을 출력합니다. (luck)  
```

* * *

#### 사용자에게 입력받은 이메일 유효성 검사 예제

```js
var userEmail=prompt("당신의 이메일 주소는?","");  
    
var check1=false; //초기 값 저장  
var check2=false; //초기 값 저장  
    
//이메일 주소에 뒷부분 형식을 배열로 저장  
var arrUrl=[".co.kr",".com",".net",".or.kr",".go.kr"];  
  
/*방문자가 입력한 이메일 주소에 "@"포함되어 있으면 변수  
check1에 true가 저장*/  
if(userEmail.indexOf("@")>0) check1=true; //indexOf는 찾는 문자가 없는경우만 -1을 반환 합니다.  
    
 //이메일에 배열 데이터 포함여부 검사  
for(var i=0; i<arrUrl.length; i++){  
 if(userEmail.indexOf(arrUrl[i])>0) check2=true;  
}  
  
    
//AND(&&)연산자는 모두 피연산자가 모두 true여야 true를 반환합니다.  
if(check1&&check2){  
 document.write(userEmail);  
}else{  
 alert("이메일 형식이 잘못되었습니다.");  
}  
```