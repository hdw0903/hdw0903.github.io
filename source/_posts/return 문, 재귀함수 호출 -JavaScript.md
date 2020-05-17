---
title: return 문,재귀 함수 호출 -JavaScript
date: 2020-03-09 10:25:56
disqusId: tunas-blog-1
categories: JavaScript
tag: 
  - JavaScript
  - return
  - 재귀 함수

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

`return` 문 이란 함수에서 결과값을 되돌려 줄 때 사용합니다.  
함수에서 `return` 문이 실행되면  
반복문에 `break`문과 비슷하게 실행문이 강제 종료됩니다.

>기본형
function 함수명(){
    실행문;
    return 데이터(값);
}

```js 예시1
function calc(){  
 var result=100+200  
 return result; // calc값에 300을 반환합니다.  
}  
var num = calc();  
document.write(num);  
```

```js 예시2
function test(){  
 document.write("html");  
 return;  
 // 다음 실행문은 실행되지 않습니다.  
 document.write("javascript");  
}  
myFnc();  
```
<!-- more -->

* * *

### 재귀 함수 호출

함수 정의문 내에서 실행문으로 함수를 다시 호출하는 것을  
재귀 함수 호출 이라고 합니다.  
**함수를 반복문 처럼 여러번 호출하기 위해 사용합니다.**

>기본형
    function myFnc(){
    실행문;
    myFnc();
}
myFnc();

```js 예제
var num=0;  
function testFnc(){  
 num++; //num의 데이터가 1씩 증가됩니다.  
 document.write(num, "<br />"); //출력  
 if(num==10) return; // num값이 10이라면 종료됩니다.  
    
 testFnc(); // 아니라면 다시 testFnc함수를 호출합니다.   
}   
testFnc(); //testFnc 함수를 호출합니다.  
```

