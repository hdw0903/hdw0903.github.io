---
title: Template 리터널 -ECMAScript
date: 2020-03-29 07:27:24
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---

템플릿 리터럴(Template Literal)은 문자열 처리를 위한 템플릿을 제공합니다.

```js 구문
`string`  
`string text line 1  
 string text line 2`  
`string ${expression} string`  
tag `string ${expression} text`  
```

<!-- more -->

역따옴표(``) 안에 `AB${표현식}`과 같이 문자열과 표현식을 작성할 수있습니다. 
AB는 문자열 그대로 출력되고 ${표현식}은 표현식을 평가하고 결과를 문자열로출력합니다.

문자열과 표현식의 결과를 연결하여 문자열로 표현하는 것이 템플릿 리터널입니다.

```js 
console.log("1:", `123ABC가나다`);  
  
console.log("2:", '라인 1\n라인 2');  
console.log("3:", `첫 번째 줄  
두 번째 줄`);  
  
let one = 1, two = 2;  
console.log("4:", `1 + 2는 ${one + two}이다`);  
/*  
1: 123ABC가나다  
2: 라인 1  
라인 2  
3: 첫 번째 줄  
두 번째 줄  
4: 1 + 2는 3이다  
*/ 
```

2.  \n이 작성된 위치에서 줄을 바꿉니다.
    
3.  \n을 사용하지 않고 에디터에서 줄을 바꾸면 됩니다.  
    줄 바꿈을 한 다음 앞에 공백을 작성하면 공백이 삽입됩니다.
    
4.  “1 + 2는” 그대로 출력되고 ${one + two} 표현식을 평가합니다.  
    one 변수 값 1이 one 에 할당되고 변수 값 2가 two에 할당되고  
    one + two 의 값을 계산합니다.  
    {1 + 2 는 3이다} 가 반환됩니다.
    

* * *

## tagged Template

템플릿 앞에 tag를 작성한 형태를 태그드(tagged)템플릿 이라고 합니다.

> tag `string ${expression} text`

tag 위치에 호출할 함수 이름을 작성합니다.

함수를 호출하기 전에 템플릿에서 문자열과 표현식을 분리하고 이를 파라미터 값으로 넘겨줍니다. 함수 이름이 작성된 템플릿을 테그드 템플릿이라고 하고 호출되는 함수를 태그 함수라고 합니다.

```js
let one = 1, two = 2;  
2. function tagFunction(text, value) {  
 3. console.log("1:", text[0]);  
 4. console.log("2:", value);  
 5. console.log("3:", text[1]);  
 6. console.log("4:", typeof text[1]);  
}  
1. tagFunction `1+2=${one + two}`;  
```

1.  템플릿에서 문자열과 표현식을 분리합니다. 1+2는 문자열이고 ${one + two}는 표현식입니다. tagFunction()을 호출하면서 분리된 문자열을 배열로 넘겨줍니다.  
    표현식은 평가 결과인 3을 넘겨줍니다. 표현식 다음에 문자열이 없으면 빈 문자열을 배열에 추가합니다.
    
2.  호출받는 함수 파라미터에 문자열과 표현식 값을 분리하여 작성합니다.  
    배열로 넘겨 받은 문자열 [“1+2=”,””]가 text에 설정되고 표현식 값인 3이 value에 설정됩니다.
    
3.  text의 첫 번째 엘리먼트 값 1+2= 이 출력됩니다.
    
4.  value는 표현식 값 3이 출력됩니다.
    
5.  text[1]은 빈 문자열(“”)이며 표현식 다음에 문자열이 없을때 엔진이 빈 문자열을 추가 시킨 값입니다.
    
6.  text[1]의 type인 string이 출력됩니다.
    

* * *

호출하는 함수에서 문자열을 배열로 넘겨주므로 태그 함수의 파라미터 이름은 하나만 작성하면 됩니다.  
표현식 평과 결과 값은 배열이 아닌 개별로 넘겨주므로 이에 맞춰 파라미터 이름을 작성해야 합니다.

```js 다수의 파라미터 형태
let one = 1, two = 2;  
function tagFunction(text, plus, minus) {  
 1. console.log(text[0], plus, text[1]);  
 2. console.log(minus, text[2], text[3]);  
}  
tagFunction `1+2=${one + two}이고 1-2=${one - two}이다`;  
```

표현식을 분리하여 평가하면 `${one + two}`는 3이고,`${one - two}`는 -1 입니다.  
따라서 호출 받는 함수의 파라미터 형태가 ([“1+2=”,”이고 1-2=”,이다],3,-1)  
형태가 됩니다.

*   text에 호출한 함수에서 문자열을 배열로 넘겨준 값이 설정됩니다.
*   plus에 표현식 값 3이 설정됩니다.
*   minus에 표현식 값 -1이 설정됩니다.

1.  text에 배열로 넘겨준 값 인덱스0번과 plus에 넘겨준 표현식 값 3  
    그리고 text에 배열로 넘겨준 값 인덱스1 번이 연결되어 출력됩니다.  
    1+2= 3 이고 1-2=
    
2.  minus에 넘겨준 표현식값 -1 과 text에 배열로 넘겨준 인덱스 2 번과 3번이 연결되어 출력됩니다. text[3]인덱스는 없으므로. undefined값이 출력됩니다.//-1 “이다” undefined
    

* * *

## String.raw

템플릿의 표현식은 변환하지만 특수 문자와 유니코드는 문자열로 인식합니다.

String.raw를 작성하고 이어서 템플릿 리터럴을 작성합니다.  
가급적 템플릿을 적용하지 않고 문자열로 표현하려는 경향이 강합니다.

```js
let one = 1, two = 2;  
console.log("1:", String.raw`1+2=${one + two}`);  
  
console.log("2:", `줄 바꿈-1\n줄 바꿈-2`);  
console.log("3:", String.raw`줄 바꿈-1\n줄 바꿈-2`);  
  
console.log("4:", `Unicode \u0031\u0032`);  
console.log("5:", String.raw`Unicode \u0031\u0032`);  
/*  
1: 1+2=3  
2: 줄 바꿈-1  
줄 바꿈-2  
3: 줄 바꿈-1\n줄 바꿈-2  
4: Unicode 12  
5: Unicode u0031\u0032  
*/  
```

1.  템플릿 앞에 String.raw를 작성하면 문자열과 표현식 평가 결과를 연결하여 반환합니다.
    
2.  템플릿에 \n을 작성하면 줄 바꿈을 하는 것과 달리  
    String.raw 템플릿에 \n을 작성하면 \n을 문자열로 출력합니다.
    
3.  \u0031과 같이 템플릿에 유니코드를 작성하면 코드 포인트 값으로 변환하여 사용합니다. 변환값인 12가 출력됩니다.
    
4.  String.raw 템플릿에 유니코드를 작성하면 코드 포인트로 변환하지 않고 문자열로 사용합니다. \u0031\u0032 가 출력됩니다.
    

* * *

## String.raw(): 문자열 전개, 조합

첫 번째 파라미터의 raw 프로퍼티 값인 문자열을 문자 하나씩 전개하면서 두 번째 이후의 파라미터를 조합하며 반환합니다.

첫 번째 파라미터에 {raw: “문자열”}형태로 작성합니다.  
raw가 아닌 다른 이름을 사용할 수 없습니다.  
두 번째 파라미터에 {raw: “문자열”}에 “문자열”과 조합할 값을 작성합니다.  
String.raw()를 템플릿의 태그 함수로 사용합니다.

```js
let one = 1, two = 2;  
let result = String.raw({raw: "ABCDE"}, one, two, 3);  
console.log(result);  
// A1B2C3DE  
```

1.  String.raw() 의 두 번째 파라미터인 one 변수에 1이 할당됩니다.
2.  String.raw() 의 세 번째 파라미터인 two 변수에 2가 할당됩니다.

3.  {raw: “ABCDE”}에서 A를 반환할 버퍼에 추가합니다. //A
4.  String.raw() 의 두 번째 파라미터 one 변수 값 1을 반환 버퍼 끝에 추가합니다. //A1

5.  {raw: “ABCDE”}에서 B를 반환 버퍼 끝에 추가합니다. //A1B
6.  String.raw() 의 세 번째 파라미터 two 변수 값 2을 반환 버퍼 끝에 추가합니다. // A1B2

7.  {raw: “ABCDE”}에서 C를 반환 버퍼 끝에 추가합니다. //A1B2C
8.  String.raw() 의 네 번째 파라미터 값 3을 반환 버퍼 끝에 추가합니다. //A1B2C3

9.  {raw: “ABCDE”}에서 D를 반환 버퍼 끝에 추가합니다. //A1B2C3D
10.  String.raw() 의 다섯 번째 파라미터가 없으므로 추가하지 않습니다.

11.  {raw: “ABCDE”}에서 E를 반환 버퍼 끝에 추가합니다. // A1B2C3DE
12.  String.raw() 의 여섯 번째 파라미터가 없으므로 추가하지 않습니다.

13.  반환 버퍼를 반환합니다. // A1B2C3DE
