---
layout: post
title:  "Promise와 Callback Hell"
date:   2019-06-09
categories: javascript
---

> Callback Hell(골백지옥) <br/>
Promise 정의<br/>
Promise 사용과 Promise Chaining<br/>
흐름<br/>
Promise 상태와 수행<br/>
출처<br/>

## Callback Hell(골백지옥)
나는 자바스크립트 개발을 하면서 다음과 같이 코딩을 해본적이 종종 있다.
```javascript
$.ajax(
  url: 'http://www.test.com/api/products/product/123',
  success: function(resource) {
    // 음 상품정보 얻어왔음!!
    doSomething(function (id) {
      // ok!!
      doSomething(function (id) {
        // 음.. 아직 뭐 ok..
        doSomething(function (id) {
          // 어라..? 또? 이젠 socpe이 해깔리네..?
          doSomething(function (id) {
            // 헐...-_-;;;
            // 경우에 따라 더 생길지도....
          }, id);
        }, id);
      }, id);
    }, resource.id);
  }
);
```

다소 극단적으로 작성하였지만, 위 코드는 $.ajax작업 수행이 성공적으로 이루어진 경우 success 안의 작업들이 동기적 실행으로 보장받기 위해서 작성한다. 그러면서 코드시선이 위에서 아래로가 아닌 위에서 우측으로 내려가는 느낌이들며, 수행하는 순서와 scope가 해깔리기 시작한다. 그래서 나온것이 Promise이다.

## Promise 정의
Promise는 ECMA Script6 스펙에 포함된 기능중 하나이다.
```javascript
const doSomething = new Promise(('성공동작함수', '실패동작함수'), => {
  // 동작구현
  if (동작조건) {
    // 성공동작함수
  } else {
    // 실패동작함수
  }
});
```

## Promise 사용과 Promise Chaining
Promise의 기능은 callback을 callback안에서 실행이 아닌 위에서 아래로 순차적으로 실행 한다.
```javascript
var doSomething = function() {
  return new Promoise(function(resolve, reject) {
    if (resource) {
      resolve(resource);
    } else {
      reject(new Error('실패!'));
      // reject 수행시 catch 실행
  });
}

// Promise Chaining (then함수 수행 후 return 값을 주면 새로운 Promise 생성시 새로운 parameter로 받을수있다)
doSomething(resource.id).then(function(id) {
  // ok!!
  return id;
}).then(function(id) {
  // ok!!
  return id;
}).then(function(id) {
  // ok!!
  return id;
}).then(function(id) {
  // ok!!
  return id;
}).catch(function(error) {
  console.log(error);
});

```

## 흐름
