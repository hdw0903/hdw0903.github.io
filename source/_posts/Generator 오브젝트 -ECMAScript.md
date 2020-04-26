---
title: Generator 오브젝트 -ECMAScript
date: 2020-03-31 08:54:39
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---

* * *

함수를 호출하면 함수 블록의 코드를 한 번에 실행하지만,  
제너레이터(Generator)오브젝트는 나누어서 실행할 수 있습니다.

*   Generator
    *   [function* 선언문](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#function*_선언문)
    *   [function* 표현식](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#function*_표현식)
    *   [GeneratorFunction():제너레이터 함수 생성](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#GeneratorFunction)
    *   [yield: 제너레이터 함수 실행,멈춤](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#yield)
    *   [next():yield 단위로 실행](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#next)
    *   [next() 활용 예제](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#next_ex)
    *   [throw(): Error 발생](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#throw)
    *   [yield* 키워드](/2020/03/31/Generator%20오브젝트%20-ECMAScript/#yield*)

<!-- more -->

제너레이터 함수 작성 형식은 3가지가 있습니다.

1.  function* 선언문
    
2.  function* 표현식
    
3.  GeneratorFunction
    

function* 선언문과 표현식은 기존의 function과 형태가 같습니다,  
“*”를 사용하는 형태만 다릅니다.  
GeneratorFunction은 new Function()과 같으며  
파라미터에 지정한 문자열로 제너레이터 함수를 생성하여 반환합니다.

### 중요 포인트

제너레이터 함수를 호출하면 제너레이터 오브젝트를 생성하여 반환합니다.  
function()을 호출하면 함수 블록을 실행하지만, 제너레이터 함수는 함수 블록을 실행하지 않고 제너레이터 오브젝트를 생성하여 반환합니다.  
생성한 제너레이터 오브젝트에 호출한 함수에서 넘겨 준 파라미터 값이 설정됩니다.

**생성된 제너레이터 오브젝트는 이터레이터 오브젝트입니다.**  
이터레이터 오브젝트의 메서드를 호출했을 때 제너레이터 함수 블록을 실행합니다.  
제너레이터 함수 블록에 yield 키워드를 작성하면 함수블록의 코드를 모두 실행하지 않고 yield 키워드 단위로 나누어 실행합니다.

제너레이터 함수는 new 연산자를 사용할 수 없으며 사용시 TypeError가 발생합니다.

* * *

<h2 id="function*_선언문">function* 선언문</h2>

선언문 형태로 제너레이터 함수를 정의합니다.

> function* name([param[, param[, … param]]]) {  
> statements  
> }

*   name  
    함수명.
    
*   param  
    함수에 전달되는 인수의 이름. 함수는 인수를 255개까지 가질 수 있다.
    
*   statements  
    함수의 본체를 구성하는 구문들.
    
*   반환 값  
    Generator 객체를 반환합니다.
    
```js
1. function* sports(one, two){  
 console.log("함수 블록");  
 yield one + two;  
};  
2. console.log(typeof sports);  
  
  
3. let genObj = sports(1, 2);  
4. console.log(typeof genObj);  
// function  
// object  
```

1.  function* sports(one, two){} 형태를 제너레이터 선언문이라고 합니다.  
    처음 제너레이터 함수를 호출하면서 넘겨주는 파라미터 값이 sports()의 파라미터에 작성한 이름(one, two)에 설정됩니다.

2.  console.log(typeof sprts) 제너레이터 함수의 typeof는 function입니다.

3.  let genObj = sports(1,2)  
    sports(1,2)로 호출하면 sports 함수 블록을 수행하지 않고 제너레이터 오브젝트를 생성하여 반환합니다. 함수 블록을 수행하면 console.log(“함수 블록”)이 실행되어야 하는데 실행되지 않습니다.  
    sports(1,2)에서 넘겨 준 파라미터 값이 function* sports(one,two){}의 파라미터 one 과 two에 설정됩니다. 따라서 제너레이터 오브젝트를 사용하여 제너레이터 함수를 호출했을 때 추가 처리를 하지 않아도 파라미터 값을 사용할 수 있습니다.

4.  생성된 제너레이터 오브젝트의 type 인 object가 출력됩니다.

Generator 설명  
Generator는 빠져나갔다가 나중에 다시 돌아올 수 있는 함수입니다. 이때 컨텍스트(변수 값)는 출입 과정에서 저장된 상태로 남아 있습니다.

Generator 함수는 호출되어도 즉시 실행되지 않고, 대신 함수를 위한 Iterator 객체가 반환됩니다. Iterator의 next() 메서드를 호출하면 Generator 함수가 실행되어 yield 문을 만날 때까지 진행하고, 해당 표현식이 명시하는 Iterator로부터의 반환값을 반환합니다. yield* 표현식을 마주칠 경우, 다른 Generator 함수가 위임(delegate)되어 진행됩니다.

이후 next() 메서드가 호출되면 진행이 멈췄던 위치에서부터 재실행합니다. next() 가 반환하는 객체는 yield문이 반환할 값(yielded value)을 나타내는 value 속성과, Generator 함수 안의 모든 yield 문의 실행 여부를 표시하는 boolean 타입의 done 속성을 갖습니다. next() 를 인자값과 함께 호출할 경우, 진행을 멈췄던 위치의 yield 문을 next() 메서드에서 받은 인자값으로 치환하고 그 위치에서 다시 실행하게 됩니다.

* * *

<h2 id="function*_표현식">function* 표현식</h2>

표현식 형태로 제너레이터 함수를 정의합니다.

> function* [name]([param1[, param2[, …, paramN]]]) {  
> statements  
> }

*   name  
    함수명. 생략하면 익명 함수가 됩니다. 함수명은 함수내에만 한정됩니다.

*   paramN  
    함수에 전달되는 인수의 이름. 함수는 최대 255 개의 인수를 가질 수 있습니다.

*   statements  
    함수의 본체를 구성하는 구문들.

*   반환 값  
    Generator 객체

*   function* expression 은 function* statement 과 매우 유사하고 형식도 같습니다. function* expression 과 function* statement 의 주요한 차이점은 함수명으로, function* expressions 에서는 익명 함수로 만들기 위해 함수명이 생략될 수 있습니다.

```js function* expression
1. let sports = function*(one, two){  
 4. console.log("함수 블록");  
 yield one + two;  
};  
2. let genObj = sports(10, 20);  
  
3. console.log(genObj.next());  
//함수 블록  
// Object {value: 30, done: false}  
```

1.  function 이름이 없는 무명(혹은 익명) 함수입니다.  
    함수를 변수에 할당해 줌으로써 sports를 함수 이름으로 사용할 수 있습니다.  
    function_에 직접 함수 이름을 작성할 수 있지만, 외부에서 함수를 호출할 때는 sports()로 호출해야 합니다.  
    function_ 함수 이름은 함수 안에서 자신을 호출할 때 사용됩니다.(재귀 함수 호출)  
    ~~하지만 변수에 할당하며 작성한 함수 이름으로 재귀 호출할 수 있으므로  
    function* 에 직접 함수이름을 작성하는 방법은 잘 사용하지 않습니다.  
    자바스크립트 초기 버전에서 사용했습니다.~~

2.  sports(10, 20)으로 호출하여 제너레이터 오브젝트를 생성하고, 10을 파라미터 one에 설정하고 20을 two에 설정합니다.  
    이때, 함수 블록의 코드를 실행하지 않고 생성한 제너레이터 오브젝트를 반환합니다.

3.  생성된 제너레이터 오브젝트는 이터레이터 오브젝트입니다. 제너레이터 오브젝트의 next()를 호출하면 이터레이터 오브젝트와 같은 처리를 수행합니다.  
    next()를 호출하여 sports 제너레이터 함수의 함수 블록을 수행합니다.

4.  제너레이터 함수 블록의 코드 입니다.  
    console에 “함수 블록”을 출력합니다.  
    sports(10, 20)으로 호출했을 때, one과 two에 값을 설정했으므로  
    yield 키워드에서 파라미터 이름으로 값을 구할 수 있습니다.  
    yield 키워드는 yield 오른쪽의 표현식을 평가하고,  
    평가 결과를 {value: 30, done: false} 형태로 반환합니다.

* * *

**개발자 도구에서 sports 제너레이터 함수** (== 크롬브라우저)

<img src="/images/sportsConsole.JPG">

엔진이 function* 키워드를 만나면 제너레이터 함수 오브젝트를 생성하여  
sports 변수에 할당 합니다.

sports(10, 20)을 호출하면 sports.prototype에 연결된 프로퍼티로 제너레이터 오브젝트를 생성하여 반환합니다.

&#95;&#95;proto&#95;&#95;.constructor는 생성자 함수로 이름이 GeneratorFunction입니다.

반환된 제너레이터 오브젝트에  
&#95;&#95;proto&#95;&#95;.&#95;&#95;proto&#95;&#95;에 next()가 있으므로  
genObj.next()형태로 호출할 수 있습니다.

* * *

<h2 id="GeneratorFunction">GeneratorFunction(): 제너레이터 함수 생성</h2>

제너레이터 함수를 생성하여 반환합니다.

GeneratorFunction 생성자는 새로운 generator function 객체를 생성합니다. JavaScript 에서 모든 generator function 은 실제로 GeneratorFunction object 입니다.

주의할 점은, GeneratorFunction 이 전역 객체(global object)가 아니란 점입니다. GeneratorFunction은 다음의 코드를 실행해서 얻을 수 있습니다.

> Object.getPrototypeOf(function*(){}).constructor

new 연산자로 GeneratorFunction() 함수를 호출할 수 없습니다. 이름 없는 제너레이터 함수를 생성하고, 여기에 연결된 constructor를 사용하여 제너레이터 함수를 생성합니다.

```js
1. let GenConst = Object.getPrototypeOf(function*(){}).constructor;  
2. let sports = new GenConst(  
 "one", "two",  
 "console.log('함수 블록'); yield one + two"  
);  
3. let genObj = sports(3, 4);  
  
4. console.log(genObj.next());  
// 함수 블록  
// Object {value: 7, done: false}  
```

1.  제너레이터 함수를 생성하기 위한 생성자(constructor)를 구합니다.  
    function*(){}으로 익명 제너레이터 함수를 생성하여 Object.getPrototypeOf()의 파라미터 값으로 지정합니다. 익명 제너레이터 함수의 prototype 오브젝트가 반환됩니다. prototype에 constructor가 있으므로 이를 반환하고 GenConst에 할당합니다. (Genconst 변수가 생성자 함수가 되는 것 입니다.)

2.  new 연산자로 GenConst 생성자를 호출하여 제너레이터 함수를 생성합니다.  
    (GenConst의 파라미터에 생성될 제너레이터 함수에서 사용할 파라미터와 함수 블록 코드를 문자열로 작성합니다.)  
    첫 번째 one 과 두 번째 two가 제너레이터 함수의 파라미터가 되고,  
    세 번째 파라미터가 함수 블록 코드가 됩니다.

파라미터의 문자열을 parsing(문자열 해석?)하면 다음과 같은 형태가 되는 것입니다.

```js new GenConst parsing
(function*(one,two){  
 console.log('함수 블록');  
 yield one + two  
})  
```

3.  sports(3,4)로 호출하면 제너레이터 오브젝트를 생성하여 반환합니다.  
    function*(one, two){}에서 3이 one에 4가 two에 설정됩니다.

4.  next()를 호출하면 함수 블록 코드를 실행합니다.  
    Object {value: 7, done: false}를 반환합니다.

* * *

<h2 id="yield">yield: 제너레이터 함수 실행, 멈춤</h2>

yield 키워드는 제너레이터 함수를 멈추게 하거나 다시 실행하는데 사용됩니다.

> [returnValue] = yield [expression];

*   expression  
    제너레이터 함수에서 제너레이터 프로토콜을 통해 반환할 값을 정의합니다(표현식). 값이 생략되면, undefined를 반환합니다.

*   returnValue  
    제너레이터 실행을 재개 하기 위해서, optional value을 제너레이터의 next() 메서드로 전달하여 반환합니다.

*   yield의 표현식 평가 결과를 왼쪽의 returnValue에 할당하지 않습니다.  
    제너레이터 오브젝트의 next()를 호출하면 next() 파라미터 값이 returnValue에 할당됩니다.

*   next()로 제너레이터 함수를 호출하면 yield 작성에 관계없이 “{value: 값, done: false/true}” 형태로 반환합니다.

*   yield를 수행하면 표현식 평가 결과가 value 값에 설정되고, yield를 수행하지 못하면 undefined가 설정됩니다.

```js
function* sports(one){  
 let two = yield one;  
 let param = yield two + one;  
 yield param + one;  
}  
1. let generatorObj = sports(10);  
  
2. console.log(generatorObj.next());  
3. console.log(generatorObj.next());  
4. console.log(generatorObj.next(20));  
//Object {value: 10, done: false}  
//Object {value: NaN, done: false}  
//Object {value: 30, done: false}  
```

1.  sports(10)으로 호출하여 제너레이터 함수의 one 파라미터에 10이 설정되고  
    제너레이터 오브젝트를 생성하여 반환합니다. (함수 블록의 코드는 실행하지 않습니다.)

2.  generatorObj.next()를 호출하면 sports 제너레이터 함수 블록의 첫 줄부터 첫 번째 yield까지 수행합니다. == (let two = yield one;)  
    yield의 표현식 평과 결과 {value: 10, done: false}형태를 반환합니다.  
    할당연산자(=)는 오른쪽 값을 왼쪽 변수에 할당하지만, yield 의 할당연산자(=)는 할당하지 않습니다.

3.  next()를 다시한번 호출하면 파라미터 값을 (let two = yield one)에서 two 변수에 설정합니다. 파라미터 값이 지정되지 않았으며 undefined가 two 변수에 설정됩니다. 그리고 아래 코드를 실행합니다.  
    two 변수에 undefined가 설정되어 있고, one 변수에 10이 설정되어 있으므로  
    Object {value: NaN, done: false}를 반환합니다. NaN을 param 변수에 설정하지 않습니다.

4.  next(20)으로 호출하면 파라미터 값 20을 (let param = yield two + one)에서 param 변수에 설정합니다. param 변수 값이 20이고 one 변수 값이 10이므로  
    {value: 30, done: false}를 반환합니다.

```js
function* sports(one){  
 yield one;  
 4. let check = 10;  
}  
1. let genObj = sports(10);  
  
2. console.log(genObj.next());  
3. console.log(genObj.next());  
//Object {value: 10, done: false}  
//Object {value: undefined, done: true}  
```

1.  sports(10) 으로 호출하면 제너레이터 함수의 one 파라미터에 10이 설정되고  
    제너레이터 오브젝트를 생성하여 반환합니다.

2.  next()를 호출하면 함수 블록의 첫 줄부터 첫 번째 yield의 표현식까지 수행합니다. yield one; = {value: 10, done: false} 형태로 반환합니다.

3.  next()를 호출하면 파라미터 값을 바로 앞 yield 왼쪽에 있는 변수에 할당합니다.  
    그런데 왼쪽의 변수가 없으므로 값을 할당하지 않습니다.  
    이후에 아래 코드를 실행합니다.

4.  check에 10을 할당하는 코드이지만 더 이상 수행할 yield는 없고  
    함수 안에 더 처리해야할 코드도 없습니다. 반환할 값이 없습니다.  
    value 프로퍼티 값에 undefined를 설정하고, done 프로퍼티 값에 true를 설정합니다. {value: undefined, done: true} 형태로 반환됩니다.

* * *

<h2 id="next">next(): yield 단위로 실행</h2>

제너레이터 함수에서 yield 단위로 실행합니다.

next()를 호출하면 yield를 기준으로 이전 yield의 다음 줄부터 yield까지 수행합니다.

제너레이터 함수에 yield가 여러개 작성되어 있으면, yield 수만큼 next()를 작성해줘야 제너레이터 함수 전체를 실행하게 됩니다.

파라미터는 선택으로 제너레이터 함수가 멈춘 yield의 왼쪽 변수에 설정합니다.

```js
let gen = function*(value){  
 value = value + 10;  
 yield ++value;  
 value = value + 7;  
 yield ++value;  
};  
1. let genObj = gen(1);  
  
console.log(genObj.next());  
console.log(genObj.next());  
console.log(genObj.next());  
//Object {value: 12, done: false}  
//Object {value: 20, done: false}  
//Object {value: undefined, done: false}  
```

1.  gen(1)로 호출하면 제너레이터 함수의 value 파라미터에 1이 설정되며 제너레이터 오브젝트를 생성하여 반환합니다.

2.  next()를 호출하면 제너레이터 함수 첫 줄부터 yield의 표현식까지 수행합니다.  
    즉, 다음 코드를 실행합니다.  
    value = value + 10;  
    yield ++value;  
    파라미터로 받은 1에 10을 더해 value에 할당합니다.  
    다음으로 yield ++value;를 실행하여 value값에 1을 더합니다.  
    {value: 12, done: false}가 반환됩니다.

3.  다시 next()를 호출하면 yield ++value에서 yield 왼쪽에 파라미터 값을 설정합니다. 그런데 왼쪽에 변수가 없으므로 다음 코드를 실행합니다.  
    value = value + 7;  
    yield ++value;  
    value 변수 값이 12이므로 7을 더해 19를 value에 할당합니다.  
    다음으로 yield ++value;을 실행하여 {value: 20, done: false}를 반환합니다.

4.  다시 next()를 호출하면 제너레이터 함수에 yield가 없으므로  
    {value: undefined, done: false}를 반환합니다.

* * *

<h2 id="next_ex">next() 활용 예제</h2>

청구 금액과 할인 금액 계산하여 반환

1.  청구 금액을 계싼하는 제너레이터 함수와 할인 금액을 계산하는 일반 함수를 정의합니다.
    
2.  청구 금액 계산 제너레이터 함수는 수량과 단가를 파라미터로 받아 금액을 계산합니다.
    
3.  계산한 금액을 yield로 반환합니다.
    
4.  할인 금액 함수를 호출하면서 yield로 반환된 값을 파라미터 값으로 넘겨 줍니다.
    
5.  파라미터의 금액에 따라 할인 금액을 계산하여 반환합니다.
    
6.  청구 금액 계산 제너레이터 함수를 호출하면서 할인 금액을 파라미터로 넘겨줍니다.
    
7.  합계 금액에서 할인 금액을 빼서 청구 금액을 계산합니다,
    
8.  계산된 청구 금액을 반환합니다.
    

```js next(), yield 활용 예제
// 청구 금액을 계산하는 제너레이터 함수 getAmount  
// 파라미터 qty = 수량 , price = 단가  
let getAmount = function*(qty, price){  
// 함수가 처음 호출될 때 수량에 단가를 곱해 합계 금액을 구하고 소숫점은 버립니다.  
 let amount = Math.floor(qty * price);  
// 두 번째 호출될 때 할인 금액(discount)을 받아   
// 합계 금액에서 할인 금액을 빼서 반환합니다.  
 let discount = yield amount;  
 return amount - discount;  
};  
// 할인 금액을 구하는 함수 getDiscount  
// 파라미터 값인 amount가 1000 보다 크면 0.2를 곱하고  
// 아니면 0.1을 곱합니다.  
let getDiscount = function(amount){  
 return amount > 1000 ? amount * 0.2 : amount * 0.1;  
};  
// getAmount(10, 60)으로 호출하여 qty값 10 price값 60이 설정됩니다.  
// 제너레이터 오브젝트를 생성하여 반환합니다.  
let amountObj = getAmount(10, 60);  
// next()를 호출하여 제너레이터 함수를 실행시키고 반환된 결과를  
// result 변수에 할당합니다.  
 /*  
 let amount = Math.floor(qty * price);  
 10 * 60 = 600  
 amount에 600이 할당됩니다.  
 {value: 600, done: false} 반환  
 */  
let result = amountObj.next();  
console.log(result);  
//할인 금액을 구하는 함수  
//앞의 next()에서 value:600을 반환했으므로 파라미터 값으로 600을 넘겨줍니다.  
//호출된 dcAmount에서 60을 반환합니다.  
let dcAmount = getDiscount(result.value);  
console.log(dcAmount);  
//next(dcAmount)로 호출하여 파라미터 값 60을 넘겨줍니다  
// let discount = yield amount 에서 60이 discount에 설정됩니다.  
// 그리고 return amount - discount 를 실행하여  
// {value: 540, done: true}를 반환합니다.  
console.log(amountObj.next(dcAmount));  
```

* * *

### next()의 다양한 형태

next()와 yield를 조합한 다양한 형태를 살펴 봅니다.  
제너레이터 함수를 계속 호출 하려면 yield가 이에 대응할 수 있어야 합니다.  
이를 위해 한 줄에 다수의 yield를 작성할 수도 있고,  
배열 안에 다수의 yield를 작성할 수도 있습니다.

```js next-while
let gen = function*(value) {  
 let count = 0;  
 while (value){  
 yield ++count;  
 }  
};  
let genObj = gen(true);  
  
1. console.log(genObj.next());  
2. console.log(genObj.next());  
// Object {value: 1, done: false}  
// Object {value: 2, done: false}  
```

제너레이터 함수에 while문을 작성하고 그 안에 yield를 작성한 형태입니다.

1.  처음 next()를 호출하면 gen()함수의 첫 줄부터 yield까지 수행한 후 그 값을 반환합니다.  
    (let count = 0;)는 처음 한 번만 실행되고 다음부터는 실행하지 않습니다.  
    while(value){}에서 value 파라미터 값이 true이므로 블락을 실행합니다.  
    yield ++count가 실행되어 {value: 1, done: false}를 반환합니다.

2.  다시 next()를 호출하면 앞에서 증가된 count 변수 값이 유지되므로 값을 누적할 수 있습니다. yield ++count를 실행하여 {value: 2, done: false}를 반환합니다.  
    이와 같이 while문 안에 yield를 작성하면 next()를 호출할 때마다 yield가 수행됩니다.

```js next-return-yield
let gen = function*(){  
 return yield yield yield;  
}  
let genObj = gen();  
  
1. console.log(genObj.next());  
2. console.log(genObj.next(10));  
  
3. console.log(genObj.next(20));  
4. console.log(genObj.next(30));  
// Object {value: undefined, done: false}  
// Object {value: 10, done: false}  
// Object {value: 20, done: false}  
// Object {value: 30, done: true}  
```

제너레이터 함수의 return 문에 다수의 yield를 작성한 형태입니다.  
return 문의 표현식에 yield가 3개 작성되어 있습니다.

1.  처음 next()를 호출하면 첫 번째 yield를 수행합니다.  
    yield에 반환 값이 없으므로 {value: undefined, done: false}를 반환합니다.

2.  두 번째로 next(10)을 호출하면 두 번째 yield를 수행합니다.  
    왼쪽에 파라미터 값을 받을 변수가 없으므로 파라미터로 넘겨준 값 그대로 반환합니다. 따라서 {value: 10, done: false}를 반환합니다.

3.  세 번째로 next(20)을 호출하면 세 번째 yield를 수행합니다.  
    {value: 20, done: false}를 반환합니다.

4.  마지막으로 next(30)을 호출하면 수행할 yield가 없습니다.  
    {done: true}를 반환하고, return을 작성했으므로 파라미터로 넘겨준 값을  
    반환 합니다. 즉 {value: 30, done: false}이 반환됩니다.  
    return을 작성하지 않으면 {value: undefined, done: true}를 반환합니다.

```js next-array-yield
let gen = function*(){  
 return [yield yield];  
};  
let genObj = gen();  
  
1. console.log(genObj.next());  
2. console.log(genObj.next(10));  
3. console.log(genObj.next(20));  
// Object {value: undefined, done: false}  
// Object {value: 10, done: false}  
// Object {value: Array[1], done: true}  
```

제너레이터 함수의 return 문에 배열안에 다수의 yield를 작성한 형태입니다.

1.  2.  는 return문에 yield을 연속으로 작성한 것과 같습니다.  
        yield가 2개 뿐이므로 더 이상 수행할 yield가 없는 상태가 됩니다.

3.  마지막으로 next(20)을 호출하면 (return [yield yield])에서 yield를 제외한  
    []안에 파라미터로 넘겨준 값을 작성합니다.  
    {value: Array[1], done: true} 형태로 반환됩니다.  
    Array[1]은 {0:20} 입니다.

```js for-of-yield
let gen = function*(start){  
 let value = start;  
 while (true){  
 yield ++value;  
 }  
};  
  
for (var count of gen(10)){  
 console.log(count);  
 if (count > 12){  
 break;  
 }  
};  
// 11  
// 12  
// 13  
```

for-of() 문에 제너레이터 함수를 호출하는 코드를 작성한 형태입니다.

*   처음 for-of 문을 시작하면 gen(10)을 호출하여 제너레이터 함수의 start 파라미터에 10을 설정하고 제너레이터 오브젝트를 생성하여 반환합니다.  
    호출한 gen(10) 위치로 돌아오면 반환받은 오브젝트를 할당할 변수가 없으므로 엔진 내부에 저장합니다.

*   gen() 함수를 호출하며 이는 next()를 호출한 것과 같습니다.  
    let value = start;를 수행한 후 while (true){yield ++value;}를 수행합니다.  
    {value: 11, done:false}를 반환하게 되며 value 프로퍼티 값이 count 변수에 설정됩니다.

*   for-of 문의 블록에 작성한 코드를 수행하며 콘솔에 11이 출력됩니다.  
    count 변수 값이 11이므로 다시 for-of 문을 반복하게 됩니다.  
    count 변수 값이 12보다 클때 까지 for-of문을 반복합니다.

* * *

<h2 id="throw">throw(): Error 발생</h2>

throw() 메서드는 Generator의 실행을 재개시키고 Generator 함수의 실행 문맥 속으로 error를 주입합니다.

제너레이터 함수를 호출하여 받은 제너레이터 오브젝트의 throw()를 호출하면  
에러가 발생합니다. 에러가 발생하면 제너레이터 함수의 catch()문에서 에러를 받습니다.

```js
let gen = function*(){  
 try {  
 yield 10;  
 } catch (message) {  
 yield message;  
 }  
 yield 20;  
}  
let genObj = gen();  
  
1. console.log(genObj.next());  
2. console.log(genObj.throw("에러 발생"));  
3. console.log(genObj.next());  
// Object {value: 10, done: false}  
// Object {value: "에러 발생", done: false}  
// Object {value: 20, done: false}  
```

1.  next()를 호출하면 try문 의 (yield 10;)을 실행합니다.  
    에러가 발생하지않으며 {value: 10, done: false}를 반환합니다.

2.  throw(“에러 발생”)를 호출하면 제너레이터 함수의 catch(message)가 실행됩니다.  
    throw()의 파라미터 값이 catch(message)의 message 파라미터에 설정됩니다.  
    catch() 블록{}의 (yield message;)가 실행되어 {value: “에러 발생”, done: false}가 반환됩니다.  
    중요한 점은 done: false를 반환한다는 점입니다. 즉, 에러는 발생했지만 다음에 next()를 호출할 수 있습니다.
    *   genObj.throw(Error(“에러 발생”)); 과 같이 파라미터에 Error 오브젝트를 작성할 수도 있습니다.

3.  앞에서 throw()를 호출하며 catch()블록을 수행했지만 이터레이터가 종료된 것은 아닙니다. 따라서 다시 next()를 호출할 수 있으며 함수의 (yield 20;)을 수행하여  
    {value: 20, done: false}를 반환합니다.

```js 제너레이터 함수에서 에러가 발생
let gen = function*(){  
 throw "에러 발생";  
 yield 20;  
};  
let genObj = gen();  
  
1. try {  
 let result = genObj.next();  
} catch (error) {  
 console.log(error);  
}  
2. console.log(genObj.next());  
//에러 발생  
// Object {value: undefined, done: true}  
```

1.  try문에서 next()를 호출하면 제너레이터 함수 첫 줄에서 throw문으로 인해 에러가 발생하며, catch(error)에서 에러를 받습니다.  
    (throw “에러 발생”)에서 “에러 발생”이 catch(error)의 error 파라미터에 설정됩니다.

2.  제너레이터 함수에서 에러가 발생하면 이터레이터가 종료됩니다.  
    따라서 next()를 실행하면 제너레이터 함수내에 throw “에러 발생” 밑에  
    yield 20;이 있지만 실행되지 않습니다.  
    {value: undefined, done: true}가 반환됩니다.

* * *

<h2 id="yield*">yield* 키워드</h2>

> yield* [[expression]]

표현식(expression)에 이터러블 오브젝트를 작성합니다. next()를 호출할 때 마다  
이터러블 오브젝트를 하나씩 실행하며, 결과 값을 yield의 반환 값으로 사용합니다.

표현식에 제너레이터 함수를 작성할 수 있습니다.  
표현식으로 호출된 함수에 다수의 yield가 있으면 호출된 함수의 yield를 전부 처리한 후 yield* 아래에 작성한 코드를 실행합니다.

```js yield* 표현식에 제너레이터 함수 작성 형태
let plusGen = function*(value) {  
 yield value + 5;  
 yield value + 10;  
};  
let gen = function*(value) {  
 yield* plusGen(value);  
 yield value + 20;  
};  
let genObj = gen(10);  

1. console.log("1:", genObj.next());  
2. console.log("2:", genObj.next());  
3. console.log("3:", genObj.next());  
// Object {value: 15, done: false}  
// Object {value: 20, done: false}  
// Object {value: 30, done: false}  
```

1.  console.log로 genObj.next()를 호출하면 실행되는 순서가 다음과 같습니다.
    
    1.  gen() 함수의 yield* plusGen(value)을 실행합니다.
        
    2.  yield*를 작성했으므로 plusGen(value)을 호출하면서 파라미터 값으로 10을 넘겨줍니다.
        
    3.  plusGen()이 제너레이터 함수이므로 제너레이터 오브젝트를 생성하여 반환합니다.
        
    4.  next()로 호출헤야 plusGen() 함수의 yield를 수행하지만, 이때는 자동으로 plusGen()의 첫 번째 (yield value + 5)를 수행하며 {value: 15, done: false}를 반환합니다.
        
    5.  plusGen()을 호출한 곳에서 다시 yield를 수행하므로 plusGen()에서 반환된 값을 반환합니다.
        
    6.  콘솔에 Object {value: 15, done: false}를 출력합니다.
        

2.  next()를 호출하면 plusGen()에서 수행하지 않은 (yield value + 10;)을 실행하며 Object {value: 20, done: false}를 반환합니다.

3.  next()를 호출하면 plusGen()에 더 이상 실행할 yield가 없으므로  
    plusGen()을 호출한 gen() 함수 내의 코드 아래의 코드를 수행합니다.  
    (yield value + 20;) 을 실행하게 되며 Object {value: 30, done: false}를 반환합니다.

```js yield* 표현식 재귀 호출 형태
let gen = function*(value) {  
 yield value;  
 yield* gen(value + 10);  
}  
let genObj = gen(1);  
  
1. console.log(genObj.next());  
2. console.log(genObj.next());  
3. console.log(genObj.next());  
// Object {value: 1, done: false}  
// Object {value: 11, done: false}  
// Object {value: 21, done: false}  
```

1.  처음 next()를 호출하면 제너레이터 함수 첫째 줄의 (yield value;)를 실행하며  
    {value: 1, done: false}를 반환합니다.

2.  두 번째로 next()를 호출하면 (yield* gen(value + 10);)을 실행합니다.  
    그런데 yield* 표현식에서 자신 함수(gen)를 호출합니다. 파라미터 값으로 11을 넘겨주며 next()가 없지만 엔진에서 반환받은 오브젝트의 제너레이터 함수를 호출합니다. (yield value;)가 실행되며 {value: 11, done: false}를 반환합니다.  
    <mark>이때, yield value;가 없다면 계속해서 자신을 호출하게 되므로 무한 루프를 돌게됩니다.</mark>

3.  세 번째로 next()를 호출하면 (yield* gen(value + 10);)을 실행하고  
    자신을 호출합니다. 위와 같이 진행되어 {value: 21, done: false}를 반환합니다.
