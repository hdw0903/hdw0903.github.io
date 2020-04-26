---
title: Destructuring -ECMAScript
date: 2020-03-20 12:16:44
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---

Destructuring 은 배열의 값 또는 객체의 속성을  
별개의 변수로 압축 해제 할 수있는 JavaScript 표현식입니다.

```js 형태
let a, b, rest;  
  
[a, b] = [10, 20];  
console.log(a); // 10  
console.log(b); // 20  
  
[a, b, ...rest] = [10, 20, 30, 40, 50];  
console.log(a); // 10  
console.log(b); // 20  
console.log(rest); // [30, 40, 50]  
  
({ a, b } = { a: 10, b: 20 });  
console.log(a); // 10  
console.log(b); // 20  
  
  
// Stage 4(finished) proposal  
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});  
console.log(a); // 10  
console.log(b); // 20  
console.log(rest); // {c: 30, d: 40}  
```
변수에 배열의 엘리먼트를 할당한다는 표현보다  
배열의 엘리먼트를 변수에 할당한다는 표현이 더 정확합니다.  
배열의 엘리먼트 값을 변수에 할당하기 위해서는  
먼저, 배열의 엘리먼트를 분할하고 분할된 엘리먼트 값이 변수에 할당되기 때문입니다.

<!-- more -->

* * *

## Array 분할 할당

```js
let one, two, three, four, five;  
const values = [1, 2, 3];  
  
[one, two, three] = values; //== [1, 2, 3]  
console.log("A:", one, two, three); // A: 1 2 3  
  
[one, two] = values; //== [1, 2, 3]  
console.log("B:", one, two);// B: 1 2  
  
[one, two, three, four] = values; //== [1, 2, 3]  
console.log("C:", one, two, three, four); // C: 1 2 3 undefined  
  
[one, two, [three, four]] = [1, 2, [73, 74]];   
console.log("D:", one, two, three, four); // D: 1 2 73 74  
//배열차원이 같지 않으면 에러가 발생합니다. [0,1,[0,1]] = [0,1,[0,1]] 같은 차원 형태  
```

```js spread 사용시
let one, two, three, four, other;  
  
[one, ...other] = [1, 2, 3, 4];  
console.log(other);  
/*  
0: 2  
1: 3  
2: 4  
length: 3  
*/  
// [2, 3, 4]  
```

*   1 값은 one에 할당되고 할당되지 않은 [2, 3 ,4]는 other에 할당됩니다.

* * *

## Object 분할 할당

```js
1.   
let {one, two} = {one: 1, nine: 9};  
console.log(one, two); // 1 undefined  
  
2.   
let three, four;  
({three, four} = {three: 3, four: 4});  
console.log(three, four); // 3 4  
```

*   오른쪽이 오브젝트이면 왼쪽도 오브젝트 여야 합니다.

1.  let {one,two} 형태는 변수 선언과 할당을 한번에 하는 형식입니다.  
    오른쪽 nine이 왼쪽 오브젝트에 없으므로 값을 할당하지 않습니다.  
    왼쪽 two 변수에 값을 할당하지 않았으므로 초기값인 undefined가 유지됩니다.
    
2.  오브젝트 할당에서 사전에 선언된 변수를 사용하려면  
    소괄호() 안에 할당 코드를 작성합니다.
    

* * *

## 파라미터 분활 할당

호출하는 function 파라미터에 오브젝트를 작성하고  
호출받는 function 파라미터를 오브젝트 분할 할당 형태로 작성하면  
함수 블록에서 직접 프로퍼티 이름을 사용할 수 있습니다.

```js
//호출 받는 function  
function total( { one, plus: { two, five } } ) {  
 console.log(one + two + five); // 8  
};  
  
//function 호출  
total({one: 1, plus: {two: 2, five: 5}});  
```