---
title: Promise 오브젝트 -ECMAScript
date: 2020-04-14 09:44:27
disqusId: tunas-blog-1
categories: ECMAScript6
tag: 
- ECMAScript6
- JavaScript
---


Promise 오브젝트는 비동기(Asynchronous)처리를 위한 메커니즘을 제공합니다.  
ES5까지 없었던 개념으로 ES6에 추가되었습니다.

*   Promise 오브젝트
    *   [개요](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise)
        *   Promise 처리 순서
    *   [Promise 상태](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_상태)
        *   settled 상태
        *   fulfill (성공)
        *   reject (실패)
    *   [new Promise(): Promise 인스턴스 생성](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#newPromise)
    *   [then(): 성공, 실패 핸들러](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_then)
    *   [catch(): 실패 핸들러](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_catch)
    *   [resolve(): 성공 상태의 인스턴스 반환](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_resolve)
        *   thenable
    *   [reject(): 실패 상태의 인스턴스 반환](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_reject)
    *   [all(): 모두 성공이면 핸들러 실행](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_all)
    *   [race(): 처음 한 번만 핸들러 호출](/2020/04/14/Promise%20오브젝트%20-ECMAScript/#Promise_race)

<!-- more -->

* * *

<h2 id="Promise">개요</h2>

자바스크립트는 기본적으로 동기(Synchronous)로 실행합니다. 동기 실행이란 현재 코드가 실행을 완료해야 다음 코드가 실행되는 것을 의미합니다. 여러 줄의 코드가 있다고 했을 때, 첫째 줄의 코드가 실행을 완료해야 둘째 줄이 실행되며, 둘째 줄이 실행을 완료해야 셋째 줄이 실행되는 형태입니다.

반면, Promise는 비동기(Asynchronous)로 실행합니다. XMLHttp Request의 비동기 통신과 비슷합니다.  
클라이언트에서 서버로 보낸 요청(Request)에 서버가 응답(Response)할 때까지 통신이 연결된 상태에서 기다리지 않습니다. 따라서 서버가 처리하는 동안 다른 처리를 할 수 있습니다. 클라이언트에서 서버가 응답했을 때의 처리를 사전에 정의해 두면, 서버가 응답했을 때 정의한 코드가 자동으로 실행됩니다.  
Promise도 이와 개념이 비슷합니다.

코드 구현 관점에서 보면 Promise는 하나의 오브젝트입니다. Promise 오브젝트에서 비동기 처리 방법을 제공하므로 이에 맞추어 코드를 작성하면 됩니다.

Promise 오브젝트는 DOM(Document Object Model)에서 처음 제시되었으나 현재는 JavaScript 스펙에 포함되었습니다. 따라서 DOM에서도 사용이 가능하며 이는 DOM을 사용하는 다른 언어에서도 Promise를 사용할 수 있다는 것이 됩니다.

### Promise 처리 순서

Promise 개념을 이해하기 위해 Promise의 비동기 처리 흐름을 간단하게 살펴봅니다.

```js Promise 처리 흐름
1. function create(){  
 3. return new Promise(function(resolve, reject){  
 resolve();  
 console.log("1: resolve");  
 });  
};  
  
2. 4. 6. create().then(function(){  
 console.log("3: 성공");  
}, function(){  
 console.log("3: 실패");  
});  
5. console.log("2: 끝");  
// 1: resolve  
// 2: 끝  
// 3: 성공  
```

1.  엔진이 function 키워드를 만나면 create()를 호출할 수 있도록 Function 오브젝트로 생성합니다.  
    함수 안에 코드는 실행하지 않고 다음 줄로 이동합니다.

2.  create() 함수를 호출합니다. 함수 안에 코드가 실행됩니다.

3.  1.  return 문의 표현식을 평가하므로 new Promise()로 인스턴스를 생성합니다.
    2.  이때, Promise() 파라미터에 작성한 function(){}을 실행합니다. (function을 executer(실행자)라고 합니다.)
    3.  function(executer) 블록의 첫째 줄에 resolve()가 작성되어 있습니다. 그런데 호출을 받아서 처리할 같은 이름의 함수가 소스 코드에 없습니다. 단지 파라미터에 resolve가 작성되어 있을 뿐입니다.
    4.  resolve() 형태가 함수를 호출하는 형태이지만 호출하지 않습니다. 이에 대해서는 사전 설명이 필요하므로 뒤에서 다룹니다.
    5.  다음 줄에 console.log()를 실행하여 “1: resolve”를 출력합니다.
    6.  생성한 인스턴스를 반환합니다.  
        <mark>여기서 중요한 점이 executer가 실행된다는 것과 resolve()가 호출되지 않는다는 점입니다. resolve()를 바로 호출하지 않고 호출할 수 있는 환경이 되었을 때 호출합니다.<mark>

4.  create() 실행이 끝나면 생성한 Promise 인스턴스를 반환합니다. Promise 인스턴스에 then()이 있으므로 이어서 then()을 호출할 수 있습니다. 하지만 then()을 호출하지 않고, 아래 코드로 이동합니다.  
    <mark>앞에서 resolve()를 호출할 수 있는 환경이 되었을 때 호출하는 것과 then()을 실행하지 않고 아래 코드로 이동하는 것이 Promise 비동기 처리의 핵심 매커니즘 입니다.</mark>

5.  create() 실행 이후에 then()을 호출하지 않고 다음 줄로 이동했을 때 만나는 코드입니다.  
    “2: 끝”이 출력됩니다. 이제 더 이상 남아있는 코드가 없습니다.

6.  이제 남은 것은 then()에 작성한 function()의 실행입니다.  
    then()은 두 개의 파라미터를 갖고 있습니다.  
    위 코드에서는 첫 번째 파라미터의 function이 실행되어 “3: 성공”이 출력됩니다.  
    두 번째 파라미터의 function은 실행되지 않습니다. 첫 번째 파라미터 function이 실행된 이유는 뒤에서 다룹니다.

*   console.log 출력 순서를 보면
    1.  “1: resolve”는 new Promise()로 인스턴스를 생성할 때 executer에서 출력합니다.  
        Promise 인스턴스를 생성해야 메서드를 사용할 수 있으므로 먼저 인스턴스를 생성합니다.
    2.  “2: 끝”은 create()에 연결된 then()을 실행하지 않으므로 두 번째로 실행됩니다.
    3.  “3: 성공”은 소스 코드 전체를 끝까지 처리한 후 실행되어 세 번째로 출력됩니다.

* * *

<h2 id="Promise_상태">Promise 상태</h2>

Promise는 코드를 실행할 때마다 진행 상태를[[PromiseState]]에 저장합니다.  
상태를 저장하는 이유는 연속해서 코드를 실행하지 않고, 소스 코드 끝까지 내려갔다 다시 올라와서 실행하므로 진행 상태가 필요하기 때문입니다. 상태에 따라 다음 단계를 처리하기 위해서 입니다.

<img src="/images/promise.SVG">

Promise 진행 상태는 크게 두 가지로 나눌 수 있습니다.  
pending 과 settled로 나뉩니다.  
settled 상태는 다시 fulfill(성공) 과 reject(실패)로 나눌 수 있습니다.  
pending 과 settled는 상태이면서 발생 단계입니다. 먼저 pending 상태가 되었다가 settled 상태로 넘어갑니다.  
단계로 보면 두 단계 (pending, settled)이지만 상태 측면에서 보면 세 개이므로 세 개의 상태로 분류하기도 합니다.

### pending 상태

```js pending
function create(){  
 return new Promise(function(resolve, reject){  
 resolve();  
 console.log("1: resolve");  
 });  
};  
```

pending 상태(단계)에서는 위 코드와 같이 우선 new Promise()로 인스턴스를 생성합니다. 그리고 <mark>executer를 실행하여 성공과 실패에 따라 호출할 핸들러 함수를 바인딩 합니다.</mark>
바인딩이란 resolve()와 같이 바로 함수를 호출하지 않고 나중에 호출하므로, 그때를 위한 호출 환경을 설정하는 것을 의미합니다.

executer 블록의 코드를 실행하지 않고 소스 코드 끝까지 처리한 후 실행하므로 이 시점에서 Promise 처리의 성공과 실패를 알 수 없습니다. 따라서 성공 또는 실패가 발생했을 때, 이에 따라 함수가 호출될 수 있도록 환경 설정이 필요합니다.

### settled 상태

```js settled
create().then(function(){  
 console.log("3: 성공");  
}, function(){  
 console.log("4: 실패");  
});  
```

pending 상태가 종료되면 settled 상태로 변환됩니다. 이때 처리의 성공과 실패를 알 수 있습니다.  
settled 상태는 다시 fulfill(성공) 상태와 reject(실패) 상태로 구분됩니다. 상태에 따라 pending 단계에서 바인딩한 핸들러 함수가 호출됩니다.

#### fulfill (성공)

executer 불록의 코드가 성공적으로 실핸된 상태를 나타냅니다.  
then()의 첫 번째 파라미터의 핸들러(function)가 실행됩니다. - 핸들러 안에 성공에 따른 코드를 작성합니다.

#### reject (실패)

executer 블록의 코드 실행이 실패한 상태를 나타냅니다.  
then()의 두 번째 파라미터의 핸들러가 실행됩니다. - 핸들러 안에 실패에 따른 코드를 작성합니다.

* * *

<h2 id="newPromise">new Promise(): Promise 인스턴스 생성</h2>

Promise 인스턴스를 생성하여 반환합니다.

> new Promise()

```js
new Promise(function(resolve, reject){  
 resolve( );  
 reject( );  
 });  
```

*   executer에 두 개의 파라미터를 작성할 수 있습니다. 첫 번째 파라미터에 executer 블록에서 처리를 성공했을 때 호출할 핸들러 이름(resolve)를 작성합니다. 두 번째 파라미터에 실패했을 때 호출할 핸들러 이름(reject)를 작성합니다. resolve 와 reject는 가독성을 위한 것으로 다른 이름을 사용해도 됩니다.
    
*   executer 블록에 핸들러 함수를 작성하지 않으면, then()의 파라미터에 작성한 함수가 실행되지 않습니다.  
    핸들러 함수 이름과 executer의 파라미터에 작성한 이름과 같아야 하며, 같지 않으면 에러가 발생합니다.  
    예를 들어, resolve() 와 function(resolve)와 같이 resolve 이름이 같아야 합니다.
    
```js
function create(param){  
 2. return new Promise(function(resolve, reject){  
 3. if (param === "ok"){  
 resolve(param);  
 4. console.log("1: resolve");  
 } else {  
 reject(param);  
 }  
 });  
};  
1. 5. create("ok").then(function(param){  
 7. console.log("3: 성공,", param);  
}, function(param){  
 console.log("3: 실패,", param);  
});  
6. console.log("2: 끝");  
// 1: resolve  
// 2: 끝  
// 3: 성공, ok  
```

1.  create()를 호출하면서 “ok”를 파라미터 값으로 넘겨 줍니다. 호출받은 create() 함수의 파라마터 param에 설정됩니다.

2.  executer의 파라미터에 resolve 와 reject를 작성했습니다. Function 오브젝트를 생성하여 resolve 와 reject에 할당합니다. Promise 인스턴스를 생성하여 return 한 후, create()에 연결된 then()의 파라미터에 작성한 함수와 연결합니다.  
    이렇게 설정함으로써 executer 블록에서 resolve()를 호출하면 then()의 첫 번째 파라미터의 함수가 호출되고, reject()를 호출하면 then()의 두 번째 파라미터의 함수가 호출됩니다.  
    executer 파라미터의 resolve와 executer 블록의 resolve()와 then()의 첫 번째 파라미터 함수가 연결되고,  
    executer 파라미터의 reject와 executer 블록의 reject()와 then()의 두 번째 파라미터 함수가 연결되는 것입니다.

3.  param 값으로 받은 파라미터가 “ok”이므로 true가 되어 if 문 블록을 수행합니다.  
    resolve(param)가 함수를 호출하는 형태이지만, 지금 호출하지 않고 소스 코드를 끝까지 실행한 후 되돌아와서 호출합니다. 되돌아와서 resolve()를 호출하면 이를 받아 실행할 같은 이름의 함수가 없습니다.  
    <mark>이때 executer 파라미터의 resolve에 설정된 함수를 호출합니다. 그러면 resolve 와 then()의 첫 번째 파라미터의 함수와 연결되어 있으므로 then()의 첫 번째 함수가 연결되어 있으므로 then()의 첫 번째 함수가 실행됩니다. 이것이 Promise의 비동기 처리 메커니즘입니다.</mark>

4.  resolve(param) 다음 줄에 console.log을 실행하며 “1: resolve”를 출력합니다.  
    이제 남은 것은 생성한 인스턴스를 반환하는 것입니다.

5.  create(“ok”)의 호출이 완료되면 Promise 인스턴스를 반환하므로 “인스턴스.then()” 형태가 되어 then()을 실행할 수 있지만 바로 실행하지 않습니다. 우선 then()의 첫 번째 파라미터를 executer의 resolve에 바인딩하고, then()의 두 번째 파라미터를 executer의 reject에 바인딩 합니다.  
    이렇게 바인딩을 함으로써 executer 블록에서 resolve()를 호출했을 때 then()의 첫 번째 파라미터에 작성한 함수가 실행됩니다.

6.  소스 코드의 마지막 코드로 console에 “2: 끝”을 출력합니다. 모든 코드를 읽었으므로 이제 남은 것은 resolve()를 실행하는 것입니다.

7.  executer 블록에서 resolve(param)을 호출하면, then()의 첫 번째 파라미터 함수가 호출됩니다.  
    이때, resolve(param)에서 param 값인 “ok”가 핸들러 함수인 function(param)의 param에 설정됩니다.  
    따라서 console에 “3: 성공, ok”가 출력됩니다.

```js fail
function create(param){  
 return new Promise(function(resolve, reject){  
 if (param === "ok"){  
 resolve(param);  
 } else {  
 reject(param);  
 console.log("1: reject");  
 }  
 });  
};  
1. create("fail").then(function(param){  
 console.log("3: 성공,", param);  
}, function(param){  
 console.log("3: 실패,", param);  
});  
console.log("2: 끝");  
// 1: reject  
// 2: 끝  
// 3: 실패, fail  
```

*   바로 앞에서 다룬 코드는 성공 기준이며 위 코드는 실패 기준입니다.  
    Promise 처리 흐름은 같습니다. then()의 두 번째 파라미터에 작성한 함수가 실행된다는 점이 다릅니다.

1.  create()를 호출하면서 “fail”을 파라미터 값으로 넘겨 줍니다.  
    호출된 create() 함수에서 if 문의 else 블록 reject(param)과 console.log(“1: reject”)를 실행하게 됩니다.  
    물론 reject(param)은 이때 호출되지 않고 소스 코드를 끝까지 실행한 후 되돌아와서 호출 환경이 설정되있을 때 호출됩니다.  
    executer 블록에서 reject()가 호출되면 then()의 두 번째 파라미터에 작성한 함수가 실행됩니다.  
    따라서 콘솔에 “3: 실패, fail”이 출력됩니다.

* * *

<h2 id="Promise_then">then(): 성공, 실패 핸들러</h2>

성공과 실패 핸들러를 정의합니다.

> Promise.prototype.then(onFulfilled, onRejected)

첫 번째 파라미터에 Promise가 성공 상태가 되었을 때 실행될 핸들러 함수를 작성합니다.  
두 번째 파라미터에 Promise가 실패 상태가 되었을 때 실행될 핸들러 함수를 작성합니다.

*   executer 블록의 resolve() 와 reject()에서 다수의 파라미터 값을 넘겨주더라도 핸들러 함수에서 첫 번째 파라미터 하나만 받습니다. 따라서 여러 개의 파라미터 값을 넘겨 주려면 resolve()와 reject()의 파라미터에 배열과 같은 형태로 작성해야 합니다.

*   resolve(성공) 와 reject(실패) 핸들러 함수에서 return 문의 작성 여부와 관계없이 현재 실행 중인 Promise 인스턴스를 반환합니다. return 문을 작성하면 return 문의 표현식을 평가하고 그 결과를 [[PromiseValue]]에 undefined를 설정합니다.

*   then() 에서 Promise 인스턴스를 반환하므로 then(one).then(two)와 같이 then()을 연속해서 작성할 수 있습니다. 이때, 첫 번째 then()에서 [[PromiseValue]]에 설정한 값이 두 번째 then(two)의 파라미터인 two에 설정됩니다. 핸들러 함수에서 Promise 인스턴스를 반환하여 연속해서 메서드를 호출할 수 있도록 하고, return 문의 반환 값을 [[PromiseValue]]에 설정하여 다음 then()의 핸들러 함수의 파라미터에 설정합니다.

```js then()
1. function create(){  
 return new Promise((resolve) => resolve(100));  
};  
2. create().then(() => console.log("1:then"));  
  
  
3. create().then((param) => {  
 console.log("2:then,", param);  
 return param + 50;  
});  
  
  
4. create().then((param) => {  
 console.log("3:then,", param);  
 return param + 70;  
}).then((param) => console.log("4:then,", param));  
// 1:then  
// 2:then, 100  
// 3:then, 100  
// 4:then, 170  
```

1.  create()가 호출되면 executer 블록의 resolve()를 수행하게 됩니다.  
    하지만 호출받을 resolve 함수가 없으므로 호출 환경이 되었을 때 호출합니다.  
    즉, 소스 코드 끝까지 처리하고 되돌아와 호출합니다.  
    실패가 발생하지 않으면 executer의 파라미터와 블록에 reject를 작성하지 않아도 됩니다.

2.  then()의 첫 번째 파라미터에 핸들러 함수를 작성하여 연결합니다. executer 블록에서 resolve 파라미터 값으로 100을 넘겨 주지만 이 코드에서는 사용되지 않습니다. “1:then”을 출력합니다.

3.  두 번째로 create()를 호출하여 Promise 인스턴스를 생성합니다. executer 블록에서 resolve(100)으로 호출하면, then()의 첫 번째 파라미터의 핸들러 함수가 실행되며 100이 param에 설정됩니다.  
    return문의 표현식을 평가한 150을 [[PromiseValue]]에 설정만하고 반환하지 않으며, 실행 중인 Promise 인스턴스를 반환합니다.

4.  세 번째로 create()를 호출하여 Promise 인스턴스를 생성합니다. then()의 첫 번째 파라미터의 핸들러 함수가 호출되면 param 파라미터에 100이 설정됩니다. return 문의 표현식을 평가한 170을 [[PromiseValue]]에 설정하고 실행중인 Promise 인스턴스를 반환합니다.  
    then()에서 Promise 인스턴스를 반환하므로 두 번째 .then() 으로 호출할 수 있습니다.  
    [[PromiseValue]]에 설정된 170이 param 파라미터에 설정되며 “4: then, 170”이 출력됩니다.

코드 실행 순서 정리 :

1.  첫 번째 create() 함수를 호출합니다. Promise 인스턴스를 반환합니다.  
    연결된 then()의 핸들러 함수를 실행하지 않고 아래로 이동합니다.

2.  두 번째 create() 함수를 호출합니다. Promise 인스턴스를 반환합니다.  
    연결된 then()의 핸들러 함수를 실행하지 않고 아래로 이동합니다.

3.  세 번째 create() 함수를 호출합니다. Promise 인스턴스를 반환합니다.  
    연결된 then()의 핸들러 함수를 실행하지 않고 아래로 이동합니다.

4.  더 이상 처리할 코드가 없습니다.

5.  첫 번째의 executer 블록의 resolve()가 호출되며 then()의 핸들러 함수가 실행됩니다.  
    콘솔에 “1:then”이 출력됩니다.

6.  두 번째의 executer 블록의 resolve()가 호출되며 then()의 핸들러 함수가 실행됩니다.  
    콘솔에 “2:then, 100”이 출력됩니다.

7.  세 번째의 executer 블록의 resolve()가 호출되며 then()의 핸들러 함수가 실행됩니다.  
    콘솔에 “3:then, 100”이 출력됩니다.

8.  세 번째의 then()에 연결된 then()의 핸들러 함수가 실행됩니다.  
    콘솔에 “4:then, 170”이 출력됩니다.

<mark>then().then()과 같이 then()이 연결되어 있으면, 처음 then()의 핸들러 함수를 실행한 후, 두 번째 then()의 함수 코드를 실행합니다. 이때 처음 then()의 return 값이 두 번째 then()함수의 파라미터 값으로 설정됩니다.</mark>

* * *

<h2 id="Promise_catch">catch(): 실패 핸들러</h2>

실패(reject) 핸들러를 정의합니다.

> Promise.prototype.catch(onRejected)

*   파라미터에 Promise가 reject 상태가 되었을 때 실행될 핸들러 함수를 작성합니다.  
    then()의 첫 번째 파라미터에 함수를 작성하고, 두 번째 파라미터는 작성하지 않습니다.  
    대신 then().catch() 형태로 작성하여 then()의 두 번째 파라미터에 작성할 함수를 catch()의 파라미터에 작성합니다. then()은 성공했을 때 실행되며 catch()는 실패했을 때 실행됩니다.
    
*   catch() 의 핸들러 함수에 파라미터를 하나만 작성할 수 있습니다.  
    executer 블록의 reject() 에서 다수의 파라미터를 넘겨주려면 배열과 같은 형태로 작성해야 합니다.
    
*   catch()의 핸들러 함수에서 return 문의 작성 여부와 관계없이 현재 실행 중인 Promise 인스턴스를 반환합니다.
    
*   catch().then()과 같이 catch() 다음에 then()을 연결할 수 있으며, catch()에서 설정한 [[PromiseValue]] 값을 then()의 파라미터 값으로 넘겨줍니다.
    
```js
function create(param){  
 return new Promise((resolve, reject) =>{  
 1. param === "ok" ? resolve(param) : reject(param);  
 });  
};  
  
2. create("fail").then((param) => {  
 console.log("성공:", param);  
}).catch((param) => {  
 console.log("실패:", param);  
});  
// 실패 : fail  
```

1.  파라미터로 받은 param 값이 “ok”이면 resolve(param)을 호출하고, 아니면 reject(param)을 호출합니다.  
    “fail”값으로 create()를 호출합니다. 따라서 reject()를 호출하게 되며, catch()의 핸들러 함수가 실행됩니다.

2.  create(“fail”)로 호출하여 Promise 인스턴스를 생성합니다.  
    executer 블록에서 reject()가 호출되므로 catch()의 핸들러 함수가 실행됩니다.  
    reject(param)에서 param 값이 핸들러 함수의 파라미터인 param에 설정됩니다.  
    실패만 발생하므로 then()의 핸들러 함수는 실행되지 않고, catch() 핸들러 함수만 실행됩니다.

```js
function create(param){  
 return new Promise((resolve, reject) =>{  
 resolve("resolve");  
 });  
};  
  
1.   
create().then((param) => {  
 console.log("1:then,", param);  
 throw "에러 발생 시킴";  
2.  
}).catch((param) => {  
 console.log("2:catch,", param);  
3.  
}).then((param) => {  
 console.log("3:then,", param);  
}).catch((param) => {  
 console.log("4:catch,", param);  
});  
// 1: then, resolve  
// 2: catch, 에러 발생 시킴  
// 3: then, undefined  
```

1.  then()의 핸들러 함수가 실행되면 콘솔에 “1:then, resolve”가 출력됩니다.  
    이어서 throw 문으로 에러를 발생시킵니다. 그러면 then()에 이어서 작성한 catch()핸들러 함수가 실행됩니다.

2.  executer 블록에서 reject()를 호출해도 catch()의 핸들러 함수가 실행되지만,  
    then()에서 에러가 발생해도 catch()의 핸들러 함수가 실행됩니다.  
    이때, 앞 then()의 throw문에 작성한 “에러 발생 시킴”이 catch() 핸들러 함수의 param 파라미터에 설정됩니다. 콘솔에 “2:catch, 에러 발생 시킴”이 출력됩니다.  
    핸들러 함수에 return 문을 작성하지 않았으므로 [[PromiseValue]]에 undefined가 설정되며 실행 중인 Promise 인스턴스가 반환됩니다. catch()에서 에러가 발생하지 않으면 then().catch().then().catch() 형태에서 catch()에 연결된 두 번째 then()이 실행됩니다. 만약 에러가 발생하여 두 번째 catch()를 실행하더라도 소스 코드 전체가 종료되지 않습니다.

3.  then()이 실행되면 catch()에서 [[PromiseValue]]에 설정한 undefined가 param 파라미터에 설정됩니다.  
    콘솔에 “3:then, undefined”가 출력됩니다. then()에 이어서 catch()가 있지만, then()에서 에러가 발생하지 않았으므로 catch()가 실행되지 않습니다. then()의 핸들러 함수를 실행한 후, 소스 코드 전체가 종료됩니다.

* * *

<h2 id="Promise_resolve">resolve(): 성공 상태의 인스턴스 반환</h2>

fulfill(성공) 상태의 Promise 인스턴스를 반환합니다.

> Promise.resolve()

*   파라미터  
    value, promise, thenable
    
*   반환 값  
    파라미터 값에 따라 반환 형태가 다릅니다.
    

파라미터에 값을 작성하면 성공 상태의 Promise 인스턴스를 생성하여 반환합니다.  
이어서 then()을 작성하면 then()의 첫 번째 파라미터에 작성한 함수가 호출됩니다.  
파라미터에 Promise 인스턴스를 지정하면 인스턴스를 성공 상태로 변환하여 반환합니다.

```js
1. let promiseObj = Promise.resolve(  
 {sports: "스포츠", music: "음악"}  
);  
2. promiseObj.then((param) =>{  
 for (let name in param){  
 console.log(name, param[name]);  
 }  
});  
  
3. Promise.resolve(  
 ["sports", "music"]  
).then((param) => console.log(param));  
// sports 스포츠  
// music 음악  
// ["sports", "music"]  
```

1.  Promise.resolve()를 호출하면 Promise 인스턴스를 생성하고 Promise를 성공 상태로 설정하여 반환합니다.  
    then()의 핸들러 함수에 파라미터 하나만 작성할 수 있으므로 다 수의 파라미터 값을 넘겨주기 위해 Object 오브젝트로 작성했습니다.
    
    *   다음은 promiseObj 인스턴스 구조입니다.
    <img src="/images/resolvePromiseObj.JPG">
    
    1.  __proto__에 첨부된 프로퍼티가 Promise.prototype에 연결된 프로퍼티와 같습니다.  
        이는 new 연산자를 사용하지 않고 Promise.resolve()를 실행해도 Promise 인스턴스를 생성한다는 의미입니다.
        
    2.  [[PromiseState]] 값이 “resolve”로 설정되어 있습니다. 따라서 then()의 첫 번째 파라미터의 핸들러 함수가 실행됩니다.
        
    3.  Promise.resolve()의 파라미터 값이 [[PromiseValue]]에 설정되었으며 then()의 핸들러 함수의 파라미터에 설정됩니다.
        

2.  PromiseObj의 Promise 인스턴스가 성공 상태이므로 then()의 첫 번째 파라미터 함수가 실행됩니다.  
    이 시점에서 실행되지 않고 소스 코드에 작성된 코드를 끝까지 처리한 후 실행됩니다.  
    핸들러 함수의 param 파라미터에 {sports: “스포츠”, music: “음악”}이 설정됩니다.

3.  Promise.resolve() 가 Promise 인스턴스를 생성하여 반환하므로 then()을 연결하여 작성할 수 있습니다.  
    지금 then()의 핸들러 함수를 실행하지 않고, 위에 작성한 then()의 핸들러 함수를 먼저 실행한 후 실행합니다.  
    resolve()파라미터 값인 [“sports”, “music”]이 then()의 핸들러 함수의 param 파라미터에 설정됩니다.

```js
1. let oneObj = Promise.resolve(  
 {sports: "스포츠"}  
);  
2. Promise.resolve(oneObj).then((param) =>{  
 console.log(param);  
});  
// Object {sports: "스포츠"}  
```

*   promise.resolve() 파라미터에 promise.resolve()로 생성한 인스턴스를 지정한 형태입니다.

1.  Promise 인스턴스를 생성하여 반환합니다. 이때 resolve( )의 파라미터 값이 [[PromiseValue]]에 설정됩니다.

2.  Promise.resolve() 파라미터에 앞에서 생성한 Promise 인스턴스를 지정했습니다.  
    then()의 핸들러 함수가 실행되면 oneObj 인스턴스 [[PromiseValue]]에 설정된 값이 핸들러 함수의 param 파라미터에 설정됩니다. 즉, {sports: “스포츠”}가 설정됩니다.

### thenable

> let obj = {then(resolve,reject) {…} }와 같이 오브젝트 안에 then()을 작성한 형태를 thenable이라고 합니다.

```js
1. let oneObj = Promise.resolve({  
 then(resolve){  
 console.log("1: then");  
 resolve("thenable");  
 }  
});  
2. oneObj.then((value) => console.log("2:",value));  
// 1: then  
// 2: thenable  
```

1.  Promise.resolve()의 파라미터에 Object 오브젝트를 작성하고, 그 안에 then()을 작성했습니다.  
    이를 thenable이라고 합니다.이 시점에서는 Promise 인스턴스만 생성하고 then()을 실행하지 않습니다.  
    소스 코드의 마지막 코드까지 실행한 후 then()을 실행합니다.  
    oneObj에 생성한 인스턴스를 할당한 시점의 [[PromiseState]]값은 “pending”입니다.  
    then()의 resolve(“thenable”)을 호출하기 전까지 “pending”상태 이며 호출하면 “resolved”로 바뀝니다.

2.  oneObj 인스턴스에 then()이 포함되어 있으므로 위 코드를 연결하면 oneObj.then().then() 형태가 됩니다.  
    이 형태는 다음과 같은 순서와 방법으로 실행됩니다.
    1.  oneObj 인스턴스의 then(resolve)가 실행됩니다.
    2.  콘솔에 “1:then”을 출력합니다.
    3.  resolve(“thenable”)을 호출합니다. 이때[[PromiseValue]]에 “thenable”을 설정합니다.
    4.  oneObj.then().then() 형태에서 두 번째 then()이 호출됩니다.
    5.  두 번째 then()의 value 파라미터에 [[PromiseValue]] 값인 “thenable”이 설정됩니다.
    6.  콘솔에 “2:thenable”을 출력합니다.

```js
let thenable = {  
 then(resolve, reject){  
 resolve("resolve");  
 reject("에러");  
 }  
};  
1. let oneObj = Promise.resolve(thenable);  
  
2. oneObj.then(  
 (value) => console.log(value),  
 (value) => console.log("실행되지 않음")  
);  
// resolve  
```

*   Object 오브젝트에 then(resolve,reject)를 작성하였으며, resolve() 다음 줄에 reject()를 작성하였습니다. 작성 형태만 보면 resolve()를 호출하고 reject()를 호출할 것으로 보이지만,  
    <mark>성공 또는 실패 하나만 발생하므로 먼저 작성한 resolve()만 호출됩니다.  
    반대로 reject(), resolve() 순서로 작성하면 reject()만 호출되고 resolve()는 호출되지 않습니다.</mark>

1.  Promise.resolve() 파라미터에 thenable 오브젝트를 지정하여 Promise 인스턴스를 생성합니다. 이때 then()은 실행되지 않습니다.

2.  oneObj.then()에 두 개의 파라미터를 작성했습니다.  
    첫 번째 파라미터는 resolve()로 호출했을 때 실행되는 함수이고  
    두 번째 파라미터는 reject()로 호출했을 때 실행되는 함수입니다.  
    oneObj 인스턴스에 then()이 있으므로 oneObj.then()은 then().then() 형태가 됩니다.  
    첫 번째 then()에서 resolve(“resolve”)를 호출하면 [[PromiseValue]]에 “resolve”가 설정됩니다.  
    두 번째 then()의 첫 번째 파라미터 함수가 실행되며, value 파라미터에 “resolve”가 설정됩니다.  
    두 번째 파라미터 함수는 reject()를 호출하였을 때 실행되므로 위 코드에서는 실행되지 않습니다.

* * *

<h2 id="Promise_reject">reject(): 실패 상태의 인스턴스 반환</h2>

reject(실패) 상태의 Promise 인스턴스를 반환합니다.

> Promise.reject()

*   파라미터에 실패 사유를 작성합니다.
    
*   reject 상태로 변환된 Promise 인스턴스를 반환합니다.
    
```js
1. let promiseObj = Promise.reject("reject 처리");  
2. promiseObj.then(  
 (param) => console.log(param),  
 (param) => console.log("에러:", param));  
// 에러: reject 처리  
```

1.  Promise.reject()를 실행하면 reject 상태의 Promise 인스턴스를 생성하여 반환합니다.  
    파라미터 값이 [[PromiseValue]]에 설정되며, then()의 두 번째 파라미터 함수의 파라미터 값으로 설정됩니다.  
    다음은 promiseObj 인스턴스 구조입니다.
    <img src="/images/rejectPromiseObj.JPG">
    
    1.  Promise 인스턴스의 상태는 “reject”입니다. 따라서 then()의 두 번째 파라미터 함수가 호출됩니다.
    2.  Promise.reject() 파라미터에 작성한 “reject 처리”가 [[PromiseValue]]에 설정됩니다.

2.  promiseObj.then()에 두 개의 파라미터를 작성했습니다. 첫 번째 파라미터 함수는 resolve()로 호출했을 때 실행되고, 두 번째 파라미터 함수는 reject()로 호출했을 때 실행됩니다. 현재 reject 상태이므로 두 번째 파라미터 함수가 실행됩니다.

* * *

<h2 id="Promise_all">all(): 모두 성공이면 핸들러 실행</h2>

파라미터의 모든 Promise 인스턴스가 성공 상태이면 then()의 핸들러 함수를 실행합니다.

> Promise.all()

*   파라미터에 이터러블 오브젝트를 작성합니다.  
    이터러블 오브젝트에 작성한 순서로 Promise 인스턴스를 생성합니다.

<mark>생성한 모든 Promise 인스턴스가 성공 상태이면, then()의 첫 번째 파라미터 함수를 실행합니다.  
Promise 인스턴스가 하나라도 실패한다면, then()의 핸들러 함수를 실행하지 않습니다.</mark>

executer 블록에서 resolve()를 호출한 순서가 아닌 Promise 인스턴스를 생성한 순서로 파라미터 값을 배열에 첨부하여 [[PromiseValue]]에 설정합니다. then()의 첫 번째 파라미터 함수에서 파라미터 값으로 사용합니다.

```js
1. function order(mili) {  
 return new Promise((resolve) => {  
 setTimeout(() => {  
 console.log("실행", mili);  
 resolve(mili);  
 }, mili);  
 });  
};  
  
2. Promise.all([order(300), order(200), order(100)])  
 .then((milis) => console.log("호출", milis));  
// 실행 100  
// 실행 200  
// 실행 300  
// 호출 [300, 200, 100]  
```

1.  order(mili)가 호출되면 Promise 인스턴스를 생성하면서 setTimeout()을 실행합니다.  
    파라미터로 받은 mili 값을 지연 시간으로 사용합니다. 지연 시간이 경과한 후에 setTimeout의 콜백 함수가 실행됩니다. order()를 여러 번 호출했을 때, 호출한 순서가 아닌 mili 값에 따라 콜백 함수가 실행되므로 실행 순서가 달라질 수 있습니다. 즉, 콜백 함수에서 resolve() 호출 순서가 바뀔 수 있습니다.

2.  Promise.all()의 파라미터에 order() 호출을 배열로 작성했습니다.  
    따라서 첫 번째 엘리먼트부터 차례로 order() 함수를 호출하게 됩니다. 호출된 order() 함수에서 setTimeout()을 실행하며, 파라미터로 넘겨준 값을 지연 시간으로 사용합니다.  
    함수 호출에는 시간이 걸리지 않아 (0.1초 안에) 세 개의 order() 300,200,100 순서로 호출됩니다.  
    하지만 setTimeout()의 콜백 함수는 지연 시간으로 인해 100, 200, 300 순서로 실행되게 됩니다.

setTimeout()에서 지연 시간이 경과하면 콜백 함수에서 resolve(mili)를 호출하게 되며  
then()의 핸들러 함수가 실행됩니다. 이때, Promise.all()은 resolve(mili)를 호출할 때 마다 then()의 핸들러 함수가 실행되지 않습니다. Promise.all()의 파라미터에서 order() 호출로 생성한 Promise 인스턴스가 모두 성공적으로 처리되었을 때 한 번만 호출합니다.  
Promise.all()에서 order()함수를 총 세 번 호출하지만, 생성된 인스턴스가 모두 성공적으로 처리돼야 then()의 핸들러 함수를 실행하는 것입니다.

실행 결과 “실행 100”, “실행 200”, “실행 300”은 setTimeout의 콜백 함수가 실행한 출력 값이고,  
호출 [300, 200, 100]은 then()의 핸들러 함수에서 출력한 값입니다.

* * *

<h2 id="Promise_race">race(): 처음 한 번만 핸들러 호출</h2>

처음 한 번만 then()의 핸들러 함수를 실행합니다.

> Promise.race()

*   파라미터에 이터러블 오브젝트를 작성합니다. 이터러블 오브젝트에 작성한 순서로 Promise 인스턴스를 생성합니다. 처음 한 번만 Promise 인스턴스 성공과 실패에 따라 then()의 핸들러 함수를 호출하고, 그 다음 부터는 호출하지 않습니다.

```js
function order(mili) {  
 return new Promise((resolve, reject) => {  
 setTimeout(() => {  
 console.log(mili);  
 resolve(mili);  
 }, mili);  
 });  
};  
  
1. Promise.race([order(300), order(200), order(100)])  
 .then((milis) => console.log("then:", milis),  
 (error) => console.log(error));  
// 100  
// then: 100  
// 200  
// 300  
```

1.  Promise.race()에서 order(300), order(200), order(100) 순서로 order() 함수를 호출합니다.  
    하지만 Promise.race()는 처음 한 번만 then()의 핸들러 함수가 실행되므로  
    order(200), order(100)의 핸들러 함수는 실행되지 않습니다.
