---
title: String 오브젝트 -ECMAScript
date: 2020-03-29 05:33:24

categories: ECMAScript6
tag: 
 - ECMAScript6
 - JavaScript
 - String
 - String method
 - Unicode
 - fromCodePoint
 - codePointAt
 - includes
 - starsWith
 - endsWith
 - repeat
 - normalize

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

*   String 오브젝트
    *   [Unicode](/2020/03/29/String%20오브젝트%20-ECMAScript/#Unicode)
        *   fromCodePoint(): 코드 포인트 문자 반환
        *   codePointAt(): 코드 포인트 값 반환
    *   [includes(): 문자열 포함 여부](/2020/03/29/String%20오브젝트%20-ECMAScript/#includes)
    *   [startsWith(): 문자열 시작 여부](/2020/03/29/String%20오브젝트%20-ECMAScript/#starsWith)
    *   [endsWith(): 문자열 종료 여부](/2020/03/29/String%20오브젝트%20-ECMAScript/#endsWith)
    *   [repeat(): 문자열 복제](/2020/03/29/String%20오브젝트%20-ECMAScript/#repeat)
    *   [normalize(): 유니코드 정규화 형식 변환](/2020/03/29/String%20오브젝트%20-ECMAScript/#normalize)

<!-- more -->

* * *

<h2 id="Unicode">Unicode</h2> 

유니코드는 `“U+”`를 작성하고 이어서 코드 포인트(`codepoint`)를 작성합니다.

코드 포인트는 4자리 이상의 `UTF-16` 진수 형태로 `U+0000` 에서 `U+10FFFF`까지 약 110만 개 정도를 사용할 수 있습니다.

코드 포인트 전체는 17개 평면(`plane`)으로 나누어져 있으며, 하나의 평면은  
65535(U+FFFF)개 입니다. 첫 번째 평면을 `BMP`(Basic Multilingual Plane)라고 부르며 일반적인 문자가 이 평면에 속합니다. (ex 한글)

BPM를 제외한 코드 포인트가 속한 평면을 `Supplementary plane` 또는 `Astral plane`이라고 부릅니다. 5자리 이상의 코드 포인트가 여기에 속합니다.

```js Unicode
// 16진수 이스케이프 시퀀스  
1. console.log("1:", "\x31\x32\x33");  
  
// 유니코드 이스케이프 시퀀스  
2. console.log("2:", "\u0031\u0032\u0033");  
  
// 유니코드 코드포인트 이스케이프  
3. console.log("3:", "\u{34}\u{35}\u{36}");  
  
// U+FFFF보다 큰 코드 포인트: 코끼리  
//http://unicode-table.com/en/1F418/  
4. console.log("4:", "\u{1f418}");  
  
//서로게이트 페어(Surrogate pair)  
5. console.log("5:", "\uD83D\uDC18");  
```

1.  “\x31\x32\x33”를 변환하면 123이 반환됩니다. \에 이어서 16진수로 값을 작성합니다. 이 형태를 16진수 이스케이프 시퀀스(`Escape Sequence`)라고 합니다.


2.  “\x31\x32\x33” 를 유니코드로 작성하면 “\u0031\u0032\u0033” 형태가 됩니다.  
    이를 유니코드 이스케이프 시퀀스라고 합니다. UTF-16 진수 형태로 U+0000에서 U+FFFF까지 사용할 수 있습니다.


3.  U+FFFF 보다 큰 코드 포인트 (유니코드 이스케이프 시퀀스 범위를 넘어가는)는  
    ES6에서 “\u{34}”와 같이 중괄호 안에 코드 포인트를 작성합니다.  
    이를 유니코드 코드 포인트 이스케이프라고 합니다.  
    \u{1f418}과 같이 5자리로도 작성할 수 있습니다.


4.  코끼리 이모지를 출력하는 유니코드 코드 포인트 이스케이프 값 입니다.  
    브라우저 마다 이모지 모습이 조금씩 차이가 있을 수 있습니다.


5.  \u{1F418} 형태는 ES5에서 사용할 수 없습니다.  
    ES5에서 사용할 수 있는 형태를 `Surrogate pair` 라고 하며 “\uD83D\uDC18”와 같이 두 개의 유니코드 이스케이프 시퀀스를 사용 합니다.
    

* * *

### fromCodePoint(): 코드 포인트 문자 반환

유니코드의 코드 포인트에 해당하는 문자를 반환합니다.

> String.fromCodePoint(param);

```js
// #$%&  
console.log("1:", String.fromCodePoint(35, 36, 37));  
  
// 16진수로 지정, 49, 50, 51로 지정한 것과 같음  
console.log("2:", String.fromCodePoint(0x31, 0x32, 0x33));  
  
// 44032 = 가, 44033 = 각 = 가각  
console.log("3:", String.fromCodePoint(44032, 44033));  
  
// 코끼리 이모지  
//http://unicode-table.com/en/1F418/  
console.log("4:", String.fromCodePoint(0x1F418));  
  
/* fromCharCode는 ES5 함수이며,   
4자리 까지만 작성할 수 있으므로 코끼리 이모지가 표시되지 않음 */  
console.log("5:", String.fromCharCode(0x1f418));  
  
/* fromCharCode를 사용하려면   
Surrogate pair 형태로 값을 두 개 작성해야됨. */  
console.log("6:", String.fromCharCode(0xD83D, 0xDC18));  
```

* * *

## codePointAt(): 코드 포인트 값 반환

문자열에서 파라미터에 지정한 인덱스 번째 문자의 코드 포인트 값을 반환합니다.  
파라미터의 디폴트 값은 0 입니다.  
해당 인덱스에 문자가 없으면 `undefined`를 반환합니다.

```js
console.log("가".codePointAt(0));  
// "가"의 코드 포인트 값 44032 반환  
  
let values = "ABC";  
for (var value of values){  
 console.log(value, value.codePointAt(0));  
// A 65   
// B 66   
// C 67  
};  
```

* * *

<h2 id="includes">includes(): 문자열 포함 여부</h2>

> str.includes(searchString[, position])

**매개변수**

*   searchString:  
    이 문자열에서 찾을 다른 문자열.
    
*   position (선택적 파라미터)  
    searchString을 찾기 시작할 위치. 기본값 0.
    

**반환값**  
문자열을 찾아내면 `true`. 실패하면 `false`.

```js
let target = "123가나다라456";  
1. console.log("1: ", target.includes(2));  
  
2. console.log("2: ", target.includes("가나"));  
  
3. console.log("3: ", target.includes("12", 5));  
```

1.  `target.includes(2)`가 숫자 값이지만 문자열로 변환하여 비교합니다. `true`
    

2.  유니코드의 코드 포인트 값으로 체크하기 때문에 한글을 체크할 수 있습니다. `true`
    

3.  `target` 안에 12가 있지만 6번째 이후 부터 체크 하므로 `false`
    

* * *

<h2 id="starsWith">starsWith(): 문자열 시작 여부</h2>

`startsWith()` 메소드는 어떤 문자열이 특정 문자로 시작하는지 확인하여 결과를 `true` 혹은 `false`로 반환합니다. 대소문자를 구분합니다.

> str.starsWith(searchString[, position])

**매개변수**

*   searchString:  
    이 문자열에서 찾을 다른 문자열.
    
*   position (선택적 파라미터)  
    searchString을 찾기 시작할 위치. 기본값 0.
    

**반환값**  
문자열이 검색 문자열로 시작하면 `true`. 아니면 `false`.

```js
let target = "123가나다";  
1. console.log("1:", target.startsWith(123));  
  
2. console.log("2:", target.startsWith("23"));  
  
3. console.log("3:", target.startsWith("가나", 3));  
```

1.  123으로 대상 문자열이 시작하므로 `true` 반환.  
    두 번째 파라미터를 작성하지 않았으므로 비교 시작 인덱스는 0
    

2.  23으로 시작하지 않으므로 `false`
    

3.  두 번째 파라미터에 인덱스 값 3을 작성하여 4 번째 부터 비교합니다.  
    대상 문자열 4번째에 “가나”로 시작하므로 `true`
    

* * *

<h2 id="endsWith">endsWith(): 문자열 종료 여부</h2>

`endsWith()` 메서드를 사용하여 어떤 문자열에서 특정 문자열로 끝나는지를 확인할 수 있으며, 그 결과를 `true` 혹은 `false`로 반환합니다.

> str.endsWith(searchString[, position])

*   searchString  
    대상 문자열의 끝이 특정 문자열로 끝나는지를 찾기 원하는 문자열입니다.
    
*   position (선택적 파라미터)  
    찾고자 하는 문자열의 길이값이며, 기본값은 문자열 전체 길이입니다.  
    문자열의 길이값은 문자열 전체 길이 보다 길 수 없습니다.
    
```js
let target = "123가나다";  
1. console.log(target.endsWith("가나다"));  
  
2. console.log(target.endsWith("가나"));  
  
3. console.log(target.endsWith("가나", 5));  
```

1.  대상 문자열이 “가나다”로 끝나므로 `true`
    

2.  “가나”로 끝나지 않으므로 `false`
    

3.  두 번째 파라미터에 길이 값 5를 지정했으므로 대상 문자열의 5번째 까지만 비교합니다. “가나”로 끝나게 되므로 `true` 반환
    

* * *

<h2 id="repeat">repeat(): 문자열 복제</h2>

`repeat()` 메서드는 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열을 반환합니다.

> str.repeat(count);

*   count  
    문자열을 반복할 횟수. 0과 양의 무한대 사이의 정수([0, +∞)).
    
*   반환값  
    현재 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열.
    
*   예외  
    RangeError: 반복 횟수는 양의 정수여야 함.  
    RangeError: 반복 횟수는 무한대보다 작아야 하며, 최대 문자열 크기를 넘어선 안됨.
    
```js
let target = "123";  
1. console.log("1:", target.repeat(3));  
  
2. console.log("2:", target.repeat(0));  
  
3. console.log("3:", target.repeat(2.7));  
```

1.  “123” 문자열을 3번 반복하여 123123123 반환
    
2.  파라미터에 0을 작성하면 빈 문자열을 반환합니다. “”
    
3.  2.7과 같이 소수를 작성하면 소수를 버리고 정수만 사용하여 복제합니다.  
    123123 이 반환됩니다.
    

* * *

<h2 id="normalize">normalize(): 유니코드 정규화 형식 변환</h2>

대상 문자열을 파라미터에 작성한 유니코드 정규화 형식으로 변환하여 반환합니다.  
만약 주어진 값이 문자열이 아닐 경우에는 우선 문자열로 변환 후 정규화합니다.

> str.normalize([form])

*   form  
    유니코드 정규화 방식을 지정합니다. “NFC”, “NFD”, “NFKC”, “NFKD” 중 하나이며, 생략되거나 undefined 일 경우 “NFC”가 디폴트 값 입니다.
    
    *   NFC — 정규형 정준 결합(Normalization Form Canonical Composition).
    *   NFD — 정규형 정준 분해(Normalization Form Canonical Decomposition).
    *   NFKC — 정규형 호환성 결합(Normalization Form Compatibility Composition).
    *   NFKD — 정규형 호환성 분해(Normalization Form Compatibility Decomposition).

*   반환 값  
    주어진 문자열을 유니코드 정규화 방식에 따라 정규화된 문자열로 반환합니다.

`form`이 위에서 명시된 값 중 하나가 아닐 경우 `RangeError` 에러가 발생합니다.

`normalize()` 메서드는 문자열을 유니코드 정규화 방식에 따라 정규화된 형태로 반환합니다. 문자열의 값 자체에는 영향을 주지 않습니다.

```js
console.log("1:", "ㄱ".charCodeAt(0));  
// 12593 = (0x3131)  
console.log("2:", "ㅏ".charCodeAt(0));  
// 12623 = (0x314F)  
  
// "ㄱ" 과 "ㅏ"의 코드 포인트값을 연결 하여 작성  
let jamo = "\u3131\u314F";  
1. console.log("3:", jamo.normalize("NFC"));  
 console.log("4:", jamo.normalize());  
  
2. console.log("5:", jamo.normalize("NFD"));  
3. console.log("6:", jamo.normalize("NFKD"));  
 console.log("7:", jamo.normalize("NFKC"));  
```

1.  “ㄱ” 과 “ㅏ” 가 연결된 “가” 모습이 아닌 “ㄱㅏ” 형태가 됩니다.  
    파라미터에 형식 값을 작성하지 않아도 디폴트 값 NFC 변환 형식이 적용됩니다.
    

2.  NFC와 같이 NFD도 “ㄱㅏ” 형태로 출력됩니다.
    
    
3.  NFKD 와 NFKC는 “가” 와 같이 글자 하나로 출력됩니다.
    