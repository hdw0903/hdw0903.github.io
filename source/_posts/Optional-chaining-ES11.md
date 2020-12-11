---
title: 에러 없이 객체 속성값에 접근 Optional chaining -ES11
tags:
  - ES11
  - Optional chaining
  - 에러 없이 참조 값에 접근
toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
sidebar:
  right:
    sticky: true
date: 2020-12-12 01:05:59
categories: ES11
---

ES11에 추가된
* <mark>Optional chaining 연산자를 이용해 에러 없이 속성값에 접근해보자</mark>

<!-- more -->

------

```js person 객체 예시 코드
const person1 = {
  name: "Dongwon",
  from: {
    country: "Republic of Korea",
    city: 'Seoul'
  }
}
const person2 = {
  name: "Jason"
}
function info(person) {
  console.log(person.from.country)
}
info(person1);
info(person2);
```

`person.from.country`를 출력하는 함수 예제입니다.
`info(person2)`를 실행했을 때는 참조하는 값이 없으니 `TypeError`가 발생할 수밖에 없습니다.

<mark>이러한 **없을 수도 있는 값**에 대한 에러 처리를 위해선 아래처럼 작성해야만 했습니다.</mark>

```js
function info(person) {
  console.log(person.from ? person.from.country : undefined)
}
// 또는
function info(person) {
  console.log(person.from && person.from.country)
}
```

이런 방식은 깊은 참조로 들어갈수록 코드가 더러워 질 수밖에 없겠죠..

### Optional chaining 사용법

`Optional chaining`를 사용하면 

```js
function info(person) {
  console.log(person.from?.country)
}
```
`?.` 방식으로 접근할 수 있습니다.
`person.from`이 있다면? `country`에 해당 하는 값을 출력하고
`person.from`이 없다면? undefined를 출력합니다. <mark>(TypeError가 발생하지 않습니다.)</mark>


그 외에 함수를 호출할 때 사용하거나

```js 함수 호출
const isInfo = info?.(person); // info 함수가 존재하면 실행
```

배열에 접근할 때도

```js 배열에 접근
const index0 = arr?.[0];  // 배열이 존재하면 할당
```
사용할 수 있습니다.

* [MDN Optional chaining](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining)