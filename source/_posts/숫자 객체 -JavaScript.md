---
title: Number(숫자) 객체 -JavaScript
date: 2020-03-02 09:47:09
disqusId: tunas-blog-1
categories: JavaScript
tag: 
- JavaScript
- Number Object
- Number Object method
toc: true
widgets:
  - type: toc
    position: right
  - type: category
    position: right
  - type: tagcloud
    position: right
  - type: adsense
    position: right

sidebar:
  right:
    sticky: true
---

* * *

>기본형 
    var num1 = new Number(값);
    또는 var num1 = 값;
//new 키워드 없이 값만 입력해도 객체 생성가능.

* * *

<!-- more -->

### Number 객체의 속성

| 속성               | 설명                     |
|--------------------|--------------------------|
| MAX_VALUE          | 표현 가능한 가장 큰수    |
| MIN_VALUE          | 표현 가능한 가장 작은 수 |
| POSITIVE_INFINITY | 무한대 수 표기           |
| NEGATIVE_INFINITY | 음의 무한대 수 표기      |
| NaN                | 숫자가 아닌 경우 표기    |

* * *

### Number 객체의 메서드

| 속성             | 설명                                                   |
|------------------|--------------------------------------------------------|
| toExponential(n) | 지수 표기법으로 소수점 n자리만큼 문자형 데이터로 반환  |
| toFixed(n)       | 소수점 n자리만큼 반올림하여 문자형 데이터로 반환       |
| toPrecision(n)   | 유효 숫자 n의 개수만큼 반올림하여 문자형 데이터로 반환 |
| toString()       | 숫자형 데이터를 문자형으로 반환                        |
| valueOf()        | 객체의 원래 값을 반환                                  |
| parselnt(값)     | 데이터를 정수로 변환하여 반환                          |
| parseFloat(값)   | 데이터를 실수로 변환하여 반환                          |


* * *

**사용 예제**

```js  
// javascript로 표현가능한 최댓값을 지수 표기법으로 반환  
document.write("표현 가능한 가장 큰 수:"+Number.MAX_VALUE, "<br />");  
  
// javascript로 표현가능한 최솟값을 지수 표기법으로 반환  
document.write("표현 가능한 가장 작은 수:"+Number.MIN_VALUE, "<br />");  
  
// javascript로 숫자로 표기할 수 없을때 반환되는 값 (Not a Number)  
document.write("숫자가 아닌경우의 표기:"+Number.NaN, "<br />");  
  
// javascript로 반환되는 무한댓 값  
document.write("무한대 수 표기:"+Number.POSITIVE_INFINITY, "<br />");  
  
// javascript로 반환되는 음의 무한댓값  
document.write("음의 무한대 수 표기:"+Number.NEGATIVE_INFINITY, "<br />");  
    
  
var num1=3.456789;  
var num2=700000;  
var num3="30.5px";  
var num4=40;  
  
// num2에 저장된 값에 지정된 소수점 자리만큼 지수표기법으로 반환  
document.write(num2.toExponential(1), "<br />"); // = 7.0 * 100000 = 7.0e+5  
  
// num1에 저장된 값에 소수점 2째 자리까지 반올림하여 표기  
document.write(num1.toFixed(2), "<br />"); // 3.46  
  
// num1에 저장된 값에 지정된 숫자 2개에 반올림하여 표기  
document.write(num1. toPrecision(2), "<br />"); // (3.4)56789 > 3.5  
  
// num1에 저장된 값을 문자형 데이터로 반환 ""  
document.write(num1.toString(), "<br />"); // "3.456789"  
  
// num4에 저장된 값 반환  
document.write(num4.valueOf(), "<br />"); // 40  
  
// num3에 저장된 "30.5px"에 정수만 남겨 반환하고 num4를 더합니다  
document.write(parseInt(num3)+num4, "<br />"); // 30 + 40 = 70  
  
// num3에 저장된 "30.5px"에 실수만 남겨 반환하고 num4를 더합니다  
document.write(parseFloat(num3)+num4, "<br />"); // 30.5 + 40 = 70.5  
```