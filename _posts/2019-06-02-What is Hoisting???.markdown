---
layout: post
title:  "호이스팅에대해서..."
date:   2019-06-02
categories: javascript
---

## 자바스크립트에서 호이스팅이란?
자바스크립트에서 호이스팅(Hoisting)이란 아주 중요한 개념중 한 가지이다. 호이스팅을 번역을 하면 네이버사전에서는 다음과 같이 해석하고 있다.

 ![ex_screenshot](./../images/hoisting.png)
 
말 그대로 자바스크립트에서도 똑같이 적용되며, 적용대상은 변수와 함수에 적용된다.

## 호이스팅
자바스크립트 엔진이 자바스크립트 토큰(token)단위의 코드를 만나게되면, 자바스크립트는 해석기는 실행콘텍스트(Execution Context)를 생성하며, 이때 실행콘텍스트 종류에 따라 크게 다음과 같이 나뉜다.
> 이글은 호이스팅에 대해서만 언급을 하니 자바스크립트 해석기가 코드를 실행콘텍스트와 실행 대해서 어떻게 다루는지 추후에 다시한번 다뤄보겠다.

1. GlobalContext
2. FunctionContext
3. EvalContext 

이와 같은 실행콘텍스트들은 함수가 실행 될 때, 생성이 되는데, 실행콘텍스트는 다시 다음과 같은 구조를 가지고 있다.
1. **변수객체(Variable Object)**
2. 스코프 체인(Scope chain)
3. This
 
위 구조에서 호이스팅이 일어나는 부분은 변수객체에서 발생한다. 이 코드를 단계별로 설명하면 다음과 같다.
1. `코드` 레벨
우리가 평소 코딩하는 코드 레벨이다. 자바스크립트 해석기가 코드를 해석하기 전 단계이기도 하다

```js
var frontText = '나의 개발장비는 ';

function getName() {
  var localName = 'MacBookPro';
  return localName;
}

var backText = '이다.';

console.log(frontText + getName() + backText);
```


2. 실행콘텍스트 `생성` 단계
생성단계는 자바스크립트 해석기가 자바스크립트를 코드를 만나서 실행콘텍스트가 해석하는 단계이다. 이때, 변수객체는 콘텍스트 종류에 따라 다음과 같이 동작한다.
* 내장함수와 함수의 매개변수가 초기화.
* 콘텍스트내에서 변수객체의 변수 property value 모두 undefined 초기화.
* 함수(함수선언식)는 모든 함수 property value 값이 reference 를 참조.

```js
var frontText = undefined;
var backText = undefined;

function getName() {
  var localName = 'MacBookPro';
  return localName;
}

```


3. 실행콘텍스트 `실행` 단계
실행단계에서 실행콘텍스트에서의 변수객체의 변수 property value 값이 할당된다.
```js
var frontText = '나의 개발장비는 ';
var backText = '이다.';

function getName() {
  var localName = 'MacBookPro';
  return localName;
}

console.log(frontText + getName() + backText);

```

## 정리
1. 자바스크립트해석기는 코드레벨에서 함수와 변수를 찾아 변수객체의 변수에 property value에 변수는 undefined, 함수는 property value 값이 reference 초기화 된다.
2. 실행단계에서 변수객체에 변수객체의 변수 property value에 값을 할당한다.
> ES6에서도 호이스팅은 발생합니다. 이때 실행단계에서 TDZ(Temporal Dead Zone)에 의해서 상단에 변수를 선언되지 않으면 에러가 출력된다.(나중에 자세히 다뤄보는걸로...)


## 결론
위와 같은 동작으로 내가 의도치 않은 동작이 발생하는 경우가 생기는 경우가 종종있다. 특히 같은 scope가진 코드가 일정 라인이 넘어가면 중복된 변수를 찾기란 더더욱 힘들며 동작의도를 해석하기도 어렵다.
따라서 이러한 자바스크립트코드는 변수와 함수는 콘텍스트 최상단에 선언하는것을 권장한다.

