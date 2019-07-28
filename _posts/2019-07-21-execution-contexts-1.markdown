---
layout: post
title:  "자바스크립트 실행콘텍스트와 동작원리 -1부- (실행콘텍스트의 3가지 종류)"
date:   2019-07-21
categories: javascript, execution contexts
---

## 기본
---
실행콘텍스트는(execution contexts)는 지금까지 정리한 [클로저(closure)](https://yoosoo-won.github.io/javascript,/closure,/%ED%81%B4%EB%A1%9C%EC%A0%80/2019/06/30/closure.html), [함수(function)](https://yoosoo-won.github.io/javascript,/function,/function/declarations,/expressions/2019/07/14/function-declarations-vs-function-expressions.html), [호이스팅(hoisting)](https://yoosoo-won.github.io/javascript/2019/06/02/What-is-Hoisting.html), [스코프(scope)](https://yoosoo-won.github.io/javascript,/scope,/%EC%8A%A4%EC%BD%94%ED%94%84/2019/07/07/scope.html), [this](https://yoosoo-won.github.io/javascript,/this/2019/06/23/this.html) 등과 아주 밀접한 관련이 있으며, 따라서 실행콘텍스트에 대한 이해가 아주 중요하다. 어쩌면 그 동안 모호하게 이해가 되었던 부분이 모두 연결이 될 것이다.

실행콘텍스트는 실행가능한 코드가 실행 되기 위한 반드시 필요한 환경이라고 보면 되겠다. 그리고 콘텍스트는 다음과 같이 분류하며, [ECMA-262 스펙](https://developer.mozilla.org/ko/docs/Web/JavaScript/%EC%96%B8%EC%96%B4_%EB%A6%AC%EC%86%8C%EC%8A%A4)에서 정의하는 추상적인 개념이며, 실제로 자바스크립트 엔진이 Stack처럼 동작하는지는 모른다.

* 전역코드(Global Context)
* 함수코드(Function Context)
* Eval코드(Eval Context)

```javascript
var a = 'aaa';

function outer(k) {
  var i = 'iii';
  function inner() {
    var j = 'jjj';
  }

  inner();
}

outer('kkk');
```

위 코드를 실행콘텍스트 Stack으로 시각화 하면 다음과 같다.
![ECStack 실행](https://user-images.githubusercontent.com/15857404/61588069-fc3dcf80-abcf-11e9-8186-1163fac8dc76.png)

자바스크립트 엔진은 자바스크립트 코드를 만나면 다양한 종류의 실행 콘텍스트가 들어와서 쌓이거나 나가게된다. 
1. ECStack이 비어있는 상태는 자바스크립트 엔진이 코드를 실행하기 전 단계이거나 애플리케이션이 종료될 경우이다.
2. 자바스크립트 엔진은 전역 코드를 만나게되면 제일 먼저 Global Context 생성되며 ECStack에 쌓이게 된다.
3. 다음으로 함수가 실행되면 다시 Function Context가 ECStack에 쌓이게 되며, 코드에 따라 이를 반복하며 계속 ECStack에 쌓이게 된다.
4. 함수의 실행이 종료가 되면 pop이 발생하여 ECStack에서 사라지게 된다.
5. 마찬가지로 각 실행이 종료가 된다면 pop이 발생하며 결국 ECStack은 GC만 남게 될 것이며, 브라우저 창을 닫게되면 결국 처음과 같이 ECStack은 비어있는 상태가 된다.

## 실행콘텍스트(Execution Contexts) 3가지 종류
---
실행콘텍스트의 구조는 아래 그림과 같다.
![실행콘텍스트 구조](https://user-images.githubusercontent.com/15857404/61588471-129b5980-abd7-11e9-916c-48be00d4d3e7.png)

### VO
자바스크립트 엔진은 실행에 필요한 여러가지 담을 정보를 실행콘텍스트에 생성하여 
자바스크립트 엔진은 실행콘텍스트가 생성되면, 이곳에 실행에 필요한 여러가지 정보를 담게되는데 이것이바로 변수객체(VO/Variable Object)된다.

* 변수
* parameter, arguments(단 전역 콘텍스트의 경우 제외)
* 함수 선언식(표현식 제외)

> 전역 콘텍스트의 경우 변수와 함수 선언식으로만 되어있으며,
함수 콘텍스트의 경우 arguments 객체가 추가된다.

#### 전역 콘텍스트
전역 콘텍스트에서의 VO(Variable Object)는 유일하며 항상 최상위 객체이다. 즉 모든 전역 변수, 전역 함수를 포함하는 전역 객체를 가르킨다.
![전역콘텍스트](https://user-images.githubusercontent.com/15857404/61589314-29e04400-abe3-11e9-908b-f51ec9bcc6cd.png)

#### 함수 콘텍스트
함수 콘텍스트에서 VO는 Activation Object(AO)를 가르키며, 자바스크립트 완벽가이드에서는 호출 객체(활성 객체)라고도 불린다.
![함스콘텍스트](https://user-images.githubusercontent.com/15857404/61588750-817ab180-abdb-11e9-82ec-391ca66bca91.png)

### Scope Chain
스코프 체인은 일종에 배열 형태로 구성되며, 중첩된 콘텍스트의 레퍼런스 정보를 가지고 있다.
![Scope Chain](https://user-images.githubusercontent.com/15857404/62002404-424de280-b13e-11e9-9c30-3c6f00e0de1d.png)

> `Global EC`의 scope chain 0번과 `<outer> EC` scope chain 1번은 서로 같은 객체이다.

### This
일반적인 함수 실행인 경우 모든 콘텍스트의 this는 `Global EC`의 Global Object를 가리키게되며, 함수 이름앞에 식별자가 존재하는 경우 식별자의 객체가 this가 된다.
좀 더 자세한 내용은 [이곳](https://yoosoo-won.github.io/javascript,/this/2019/06/23/this.html)에서 확인하기 바란다.


## 결론
---
다소 내용이 길어지는것 같아 1부와 2부로 나눠서 작성하였다. 자바스크립트 동작원리를 이해를 하며, 코딩하는것과 못하고 코딩하는것은 매우 많은 차이가 있을것이다. 그래서 이번 주제를 이해를 하는것은 매우 중요하다고 생각한다. 

## 참조
---
http://dmitrysoshnikov.com/ecmascript/chapter-1-execution-contexts/
http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/
http://dmitrysoshnikov.com/ecmascript/chapter-3-this/
http://dmitrysoshnikov.com/ecmascript/chapter-4-scope-chain/
http://dmitrysoshnikov.com/ecmascript/chapter-5-functions/
http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/
https://www.codebyamir.com/blog/javascript-execution-contexts
