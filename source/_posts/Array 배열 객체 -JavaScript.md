---
title: Array(배열) 객체 -JavaScript
date: 2020-03-02 11:23:53

categories: JavaScript
tag: 
- JavaScript
- Array
- Array method
- join()
- reverse()
- splice()
- slice()
- concat()
- pop()
- push()
- shift()
- unshift()
toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right

sidebar:
  right:
    sticky: true
---

여러 개의 데이터를 하나의 저장소에 저장하는 `Array(배열)` 객체

사용법 3가지

>1. var a =new Array();
    a[0]=30;
    a[1]="홍길동";
    a[2]=true;
>2. var b =new Array(30, "홍길동", true);
>3. var c =[30, "홍길동", true];

<!-- more -->

* * *

#### 배열 객체의 메서드 및 속성

| 종류                  | 설명                                                                   |
|-----------------------|------------------------------------------------------------------------|
| join(연결문자)        | 배열 객체에 데이터를 연결 문자 기준으로 1개의 문자형 데이터로 반환     |
| reverse()             | 배열 객체에 데이터의 순서를 거꾸로 바꾼 후 반환                        |
| sort()                | 배열 객체에 데이터를 오름차순으로 정렬                                 |
| slice(index1, index2) | 배열 객체에 데이터 중 원하는 인덱스 구간만큼 잘라서 배열 객체로 가져옴 |
| splice()              | 배열 객체에 지정 데이터를 삭제하고 그 구간에 새 데이터를 삽입          |
| concat()              | 2개의 배열 객체를 하나로 결합                                          |
| pop()                 | 배열에 저장된 데이터 중 마지막 인덱스에 저장된 데이터를 삭제           |
| push(new data)        | 배열 객체 마지막 인덱스에 새 데이터 삽입                               |
| shift()               | 배열 객체에 저장된 데이터 중 첫 번째 인덱스에 저장된 데이터를 삭제     |
| unshift(new date)     | 배열 객체의 가장 앞의 인덱스에 새 데이터를 삽입                        |
| length                | 배열에 저장된 총 데이터의 개수를 반환                                  |

* * *

#### join() 과 reverse() 사용 예제

```js
var num=["사당","교대","방배","강남"];  
  // 배열 객체를 출력합니다.  
 document.write(num,"<br />"); //사당,교대,방배,강남  
  
 // 배열 객체의 형(type) 출력합니다.  
 document.write(typeof num,"<br/>"); //object  
    
 // "-" 문자를 기준으로 하나의 문자형 데이터로 결합합니다.  
 document.write(num.join("-"),"<br/>"); // 사당-교대-방배-강남  
  
 // join으로 결합된 데이터의 형이 문자형 데이터인걸 확인할 수 있습니다.  
 document.write(typeof num.join("-"),"<br/>"); //string  
  
 // 배열 객체 값 순서가 역으로 출력됩니다.  
 document.write(num.reverse(),"<br/>"); // 강남,방배,교대,사당  
  
 // 배열 객체 값들을 오름차순으로 정렬 후 출력합니다.  
 document.write(num.sort(),"<br/>"); // 강남,교대,방배,사당  
```

* * *

#### splice() 와 slice() 사용 예제

```js
var greenLine=["사당","교대","방배","강남"];  
  
/* greenline 배열 객체 인덱스 2에 저장된 데이터 1개를 삭제하고   
 "서초","역삼" 데이터 삽입 */  
 greenLine.splice(2,1,"서초","역삼"); //사당,교대,서초,역삼,강남  
 document.write(greenLine,"<br/>");  
    
 // greenline 객체의 인덱스1 부터 3이전 까지 일부 데이터만 반환합니다.  
 document.write(greenLine.slice(1,3),"<br/>"); //교대,서초  
```

* * *

#### concat(), pop(), push(), shift(), unshift() 사용 예제

```js
var greenLine=["사당","교대","방배","강남"];  
var yellowLine=["미금","정자","모란","수서"];  
  
 var twoLine=greenLine.concat(yellowLine);  
 document.write(twoLine,"<br/>"); // 2개의 배열 객체가 하나가 되어 twoLine에 저장됩니다.  
  
 greenLine.pop();  
 document.write(greenLine,"<br/>"); // 마지막 인덱스 값인 "강남" 삭제됩니다.  
  
 greenLine.push("삼성");  
 document.write(greenLine,"<br/>"); // 마지막 인덱스에 "삼성" 값을 삽입합니다.  
  
 greenLine.shift();  
 document.write(greenLine,"<br/>"); // 첫 번째 인덱스 값인 "사당" 삭제됩니다.  
  
 greenLine.unshift("신도림");  
 document.write(greenLine,"<br/>"); // 인덱스 가장 앞에 "신도림" 값을 삽입합니다.  
```
