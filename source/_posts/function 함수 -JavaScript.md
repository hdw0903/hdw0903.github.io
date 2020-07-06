---
title: function (함수) -JavaScript
date: 2020-03-09 08:53:26

categories: JavaScript
tag: 
  - JavaScript
  - function
  - function Hoisting
  - Self-invoking functions
  - Arrow function
  - 함수
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

데이터를 저장할 때는 변수를 선언하여 저장했습니다.  
하지만 변수에는 데이터만 저장할 수 있고, 실행문을 저장할 수 없습니다.  

함수를 사용하면 실행문을 메모리에 저장했다가 필요할 때 마다 호출하여 사용할 수 있습니다.

<!-- more -->

* * *

### 함수 정의문

함수를 사용하여 실행문을 저장한 것을 함수 정의문이라고 합니다.

함수를 정의하는 여러 방법이 있습니다:

* * *

#### 1.1 함수 선언 (function 문)

> function name([param[, param[, … param]]]) {  
> statements  
> }

*   name  
    함수 이름.
    
*   param  
    함수에 전달되는 인수의 이름. 함수는 255개까지 인수를 가질 수 있습니다.
    
*   statements  
    함수의 몸통을 구성하는 문.
    
*   세미콜론은 실행 가능한 JavaScript 문을 분리하는 데 사용됩니다. 함수 선언은 실행 문이 아니므로 세미콜론으로 끝나는 것은 일반적이지 않습니다.
    

* * *

#### 1.2 표현식

표현식을 사용하여 정의할 수 도 있습니다.  
함수 표현식은 변수에 저장될 수 있습니다

> var x = function (a, b) {return a * b};  
> var z = x(4, 3); // 12

함수 표현식이 변수에 저장된 후에는 변수를 함수로 사용할 수 있습니다.

위의 함수는 실제로 익명 함수 (이름이없는 함수)입니다.  
변수에 저장된 함수는 함수 이름이 필요하지 않습니다.  
변수 이름을 사용하여 항상 호출됩니다.

*   위의 함수는 실행 가능한 명령문의 일부이므로 세미콜론으로 끝납니다.

* * *

#### Self-invoking functions

함수 표현식은 자기 호출을 할수 있습니다.  
자체 호출 표현식은 호출되지 않고 자동으로 호출 (시작)됩니다.  
표현식 뒤에 ()가 있으면 함수 표현식이 자동으로 실행됩니다.

*   함수 선언을 자체 호출 할 수 없습니다. 함수 주위에 ()괄호를 추가하여 함수 표현식임을 표시해야합니다.

> (function () {  
> var x = “Hello!!”; // 자체 호출되어 실행됩니다.  
> })();

* * *

#### Function Hoisting

`Hoisting`은 변수 선언 및 함수 선언에 적용됩니다.  
이 때문에 `JavaScript 함수`는 선언되기 전에 호출 할 수 있습니다.

> myFunction(5);
>function myFunction(y) {  
return y * y;  
}

* * *

#### Arrow Functions

`Arrow` 함수는 함수 표현식 작성을위한 짧은 구문을 허용합니다.  
`function`(함수) 키워드,  
`return`(반환) 키워드 및 `{}`중괄호가 필요하지 않습니다.

> // ES5  
> var x = function(x, y) {  
> return x * y;  
> }  
> // ES6  
> const x = (x, y) => x * y;

*   `Arrow` 함수는 `hoisting` 되지않습니다.  
    반드시 사용하기 전에 정의해야합니다.  
    또한 IE 11 이전 버젼에서는 사용할 수 없습니다.

**함수 표현식은 항상 상수 값이므로**  
**const를 사용하는 것이 var를 사용하는 것보다 안전합니다.**

* * *

### 내장 함수

내장 함수는 자바스크립트 엔진에 내장된 함수 정의문을 말합니다.  
정의문 선언 없이 단지 함수 호출만으로 사용 가능합니다.

| 종류         | 설명                                                 | 예시                      |
|--------------|------------------------------------------------------|---------------------------|
| parseInt()   | 문자형 데이터를 정수형 데이터로 바꿉니다             | pareInt(“5.12”) = 5       |
| parseFloat() | 문자형 데이터를 실수형 데이터로 바꿉니다             | parseFloat(“5.12”) = 5.12 |
| String()     | 문자형 데이터로 바꿉니다                             | String(5) =”5”            |
| Number()     | 숫자형 데이토로 바꿉니다                             | Number(“5”) = 5           |
| Boolean()    | 논리형 데이터로 바꿉니다                             | Boolean(5) = true         |
| isNaN()      | 데이터에 숫자가 아닌 문자를 포함하면 true값 반환     |                           |
| eval()       | 문자형 데이터를 “”가 없는 스크립트 코드로 처리합니다 | eval(“15+5”) = 20         |
