---
title: Global Variable(전역변수)와 Local Variable(지역변수)
date: 2020-03-09 11:13:01
disqusId: tunas-blog-1
categories: JavaScript
tag: 
- JavaScript
- Global Variable
- Local Variable
- scope

toc: true
widgets:
  - type: toc
    position: right
  - type: categories
    position: right
  - type: tags
    position: right
  - type: adsense
    position: right
sidebar:
  right:
    sticky: true
---

* * *

변수는 유효범위에 따라

전역변수(`Global Variable`)와 지역변수(`Local Variable`)로 구분할 수 있습니다.

이러한 유효범위를 `scope`라고 합니다.

각각의 `function`은 각각의 `scope`를 만듭니다.

*   전역변수는 함수 외부에서 선언된 변수로, `javascript` 전체에서 접근할 수 있는 변수입니다.
    
*   지역변수는 함수 내부에서 선언된 변수로, 함수가 실행되면 만들어지고 함수가 종료되면 소멸하는 변수입니다. 함수 외부에서는 접근할 수 없습니다.
    
```js
var num =200; // 전역 변수  
  
function myFnc(){ // 지역 변수 함수  
 var num=500;  
}  
myFnc(); // 함수를 호출합니다.  
  
document.write(num);  
/* 함수 내에 작성한 var(변수)는 지역 변수이므로   
 함수 밖에서는 불러올 수 없습니다.  
 따라서 전역 변수로 선언된 num값 200이 출력됩니다.  
*/  
```

**의도하지 않는 전역 변수를 작성하게 된다면**  
**전역 변수 및 함수를 덮어쓸 수 있으므로 주의 하여 사용 해야 합니다.**
