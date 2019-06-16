---
layout: post
title:  "Promise와 Callback Hell[Part 1]"
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
---
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
---
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
---
Promise의 기능은 callback을 callback안에서 실행이 아닌 위에서 아래로 순차적으로 실행 한다.
```javascript
var doSomething = function(resourceId) {
  return new Promise(function(resolve, reject) {
    if (resourceId) {
      resolve(resourceId);
    } else {
      reject(new Error('실패!'));
      // reject 수행시 catch 실행
    }
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
---
![흐름](https://javascript.info/article/promise-basics/promise-resolve-reject@2x.png)*출처: https://javascript.info/promise-basics*

## Promise 상태와 수행
---
- unsettled(성공 or 실패 작업에 대해 결론이 없는 상태)
  - 대기(초기 상태, 결과가 없거나 진행중인 상태, new Promise())
- settled(성공 or 실패 작업에 대해 결론이 있는 상태)
  - 실행(fulfilled or resolved): 작업 수행 성공
  - 거부(rejected): 작업 수행 실패

위 상태라는 이론을 기초로 하여 다음과 같이 Promise 사용하면 어떻게 될까...?
```javascript
// q1
console.log('0');
const doSomething = new Promise((resolve, reject) => {
  resolve('1');
  reject('2');
  console.log('3');
});
doSomething.then((res) => {
  console.log(res);
});
doSomething.catch((error) => {
  console.log(error);
});
console.log('4');
```
- q1<br/>
0 -> 3 -> 4 -> 1
### 해설
상기 흐름에 그림을 참조하면 state가 존재한다. Promise가 resolve or reject가 실행하기 위해서는 Promise의 state가 반드시 unsettled(pending)상태이어야 한다. 그리고 resolve or reject 수행 후 settled로 변경되어 이후에 아무리 resolve or reject가 아무리 실행되어도 무시된다. <br/>
#### 순서(실행순서 기준로만 작성)
1. console.log('0') -> 0
2. new Promise 작업 수행: Promise state 상태 unsettled
3. resolve('1'): Promise state 상태 settled
4. reject('2'): Promise state 상태 settled, reject무시됨
5. console.log('3') -> 3
6. Promise이 then수행하면서 큐에 들어감
7. Promise이 catch수행하면서 큐에 들어감
8. console.log('4') -> 4
9. resolve가 먼저 실행되었으므로 Promise.then 통하여 큐에 들어간 작업이 수행됨 -> 1
만약 reject가 먼저 수
행되었다면 resolve 무시되며, catch가 수행되며 2번이 찍혔을것이다. <br/>
0 -> 3 -> 4 -> 2

## 결론
---
Promise이해는 자바스크립트 개발자라면 반드시 이해해야한다. 위 글에서 Promise가 아직 이해가 안된다고 생각한다면, 자바스크립트에서 동기와 비동기 개념에 대해서 다시한번 이해할 필요가 있다고 본다.

## 출처
---
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
[https://javascript.info/promise-basics](https://javascript.info/promise-basics)