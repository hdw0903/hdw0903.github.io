---
title: Math(수학) 객체 -JavaScript
date: 2020-03-02 10:38:36
disqusId: tunas-blog-1
categories: JavaScript
tag: 
- JavaScript
---

Javascript에서 더하기, 곱하기, 나누기 등등 은 산술연산자를 사용하면 됩니다.

**하지만 최댓값, 최솟값, 반올림 값 등은 산술연산자로 구할수 없습니다**

수학 객체는 이러한 수학과 관련한 작업을 처리할수 있게 해줍니다.

<!-- more -->

* * *

#### 수학 객체의 메서드 및 상수

| 종류                                     | 설명                                           |
|------------------------------------------|------------------------------------------------|
| Math.abs(숫자)                           | 숫자의 절댓값을 반환                           |
| Math.max(숫자 1, 숫자 2, 숫자 3, 숫자 4) | 숫자 중 가장 큰 값을 반환                      |
| Math.min(숫자 1, 숫자 2, 숫자 3, 숫자 4) | 숫자 중 가장 작은 값 반환                      |
| Math.pow(숫자, 제곱값)                   | 숫자의 거듭제곱한 값을 반환                    |
| Math.random()                            | 0~1 사이에 난수를 반환                         |
| Math.round(숫자)                         | 소숫점 첫째 자리에서 반올림하여 정수 반환      |
| Math.ceil(숫자)                          | 소숫점 첫째 자리에서 무조건 올림해서 정수 반환 |
| Math.floor(숫자)                         | 소숫점 첫째 자리에서 무조건 내림해서 정수 반환 |
| Math.sqrt(숫자)                          | 숫자의 제곱근 값을 반환                        |
| Math.PI                                  | 원주율 상수를 반환                             |


* * *

사용 예제

```js
var num=2.1234;  
  
var maxNum=Math.max(10, 5, 8, 30); // 최댓값 반환 (30)  
var minNum=Math.min(10, 5, 8, 30); // 최솟값 반환 (5)  
var roundNum=Math.round(num); // num에 저장된 값에 반올림 하여 반환 (2)  
var floorNum=Math.floor(num); // num에 저장된 값에 소수점에서 무조건 버림 (2)  
var ceilNum=Math.ceil(num); // num에 저장된 값에 소수점에서 무조건 올림(3)  
var rndNum=Math.random(num); // 0~1 사이에 난수가 발생합니다.  
var piNum=Math.PI; // 원주율 상수를 반환  
```
