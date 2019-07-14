---
layout: post
title:  "자바스크립트의 함수"
date:   2019-07-14
categories: javascript, function, function declarations, function expressions
---

## 기본
---
오늘은 함수에 대해서 이야기 해볼려고 한다. 자바스크립트에서 함수는 객체 중 하나이기도 한다. 

## 함수 선언식(Function Declaration)
---
함수 선언식은 다음과 같은 문법을 갖는다. 다른 프로그래밍언어와 선언방식이 유사하다.
* 함수에 이름이 붙는다.
* 변수 객체에 영향을 준다.
* 콘텍스트 진입 시점에 생성된다.

```javascript
  function 함수이름() {
    // ...
  }
```

## 함수 표현식(Function Expression)
---
자바스크립트에서는 함수는 객체이며 데이터 이기도 하다. 그래서 함수를 변수에 담을 수 있다.
* 함수 이름이 필수 요소가 아니다.
* 변수 객체에 영향이 없다.
* 코드 실행 시점에 생성된다.

```javascript
  var 함수이름 = function() {
    // ...
  }
```

## 선언식과 표현식의 차이
---
함수 선언식과 함수 표현식의 차이를 이해하기 위해서는 먼저 [호이스팅](https://yoosoo-won.github.io/javascript/2019/06/02/What-is-Hoisting.html)과 아주 밀접한 관련이 있다.
자바스크립트 해석기가 아래의 코드가 실행전 과 실행단계 로 나누어 보겠다.

### 실행전

```javascript
  var getMyName = function() {
    return 'Jack';
  };

  function getMyDevice() {
    return 'iPhone';
  }
```

### 실행단계

```javascript
  var getMyName; // undefined

  function getMyDevice() {
    return 'iPhone';
  }

  getMyName = function() {
    return 'Jack';
  }

```
위 두 단계를 보면 함수 선언식의 경우 콘텍스트 변수 객체에 함수를 참조한다. 즉 변수 객체에 영향이 있다. 하지만 함수 표현식은 일반 변수와 같이 함수를 변수에 담더라도 콘텍스트 생성시점에는 `undefined` 로 초기화가 되며, 실행시점에 비로서 함수값으로 채워진다. 이러한 영향으로 구조를 엉성하게 만든다라는 생각을 가지고 있어 더글락스 크라포드(JSON형식을 창안하신분)는 함수 선언식보다 함수 표현식을 사용하라고 권하고 있다. 즉 선언한 위치에 상관없이 모든 함수선언식문이 가장 상위로 이동하는것은 절대 좋은 현상이 아니라는 뜻으로 생각된다.

## 즉시 실행함수(IIFE, Immediately-invoked function expression)
---
기본 문법은 다음과 같다.
```javascript
  (function() {
    // ...
  })();

// or

  (function() {
    // ...
  }());
```

### 즉시 실행함수를 사용하는 이유
즉시 실행함수는 한 번의 실행만 필요로 할 때 사용한다. 자바스크립트 모든 함수와 변수 선언은 전역객체에 영향을 미친다. 그래서 최대한 전역으로 선언하는것을 피하기 위해서이다. 특히 전역콘텍스트는 콘텍스트의 모든 작업이 끝났더라도 브라우저를 닫지 않는다면, 사라지지 않는다. 그러면 모든 전역으로 선언된 함수와 변수는 전역변수로 남아있다.

```javascript
var myDevice = 'iPhone';

(function() {
  var myName = 'iMac';
})();
```
위 즉시 실행 함수 안에 선언된 `myName`은 함수가 종료가 된다면 GC(garbage collector)의해 사라질 것이다. 하지만 `myDevice`는 전역으로 선언되었으므로 브라우저 창을 닫기 전까지는 계속 메모리에 남게될 것이다.

## 결론
---
나는 비교적 함수 표현식을 선호하는 타입이다. 굳이 함수 선언식을 사용해야 할 필요성을 못느끼며, 호스팅의 영향으로 이것은 나를 많이 혼란스럽게 하기도했다. 결국은 이 둘을 정확히 이해하여 각 상황에 맞게 사용하는것이 제일 중요하다고 생각한다.