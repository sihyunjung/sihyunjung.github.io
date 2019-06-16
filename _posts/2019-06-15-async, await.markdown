---
layout: post
title:  "async, await 사용하기[Part 2]"
date:   2019-06-15
categories: javascript
---
async, await은 Promise를 보다 쉽고 직관적인 사용을 위해 ES8 스펙에 추가되었다. 따라서 이전에 Promise에 대해서 잘 이해했다면, async, await에 대해 어렵지 않게 이해하게 될것이다. 즉 Promise의 이해와 자바스크립트 동기/비동기 개념에 대해서 이해가 사전적으로 필요한 부분이다.

## 특징
---
* async로 정의된 일반 함수는 Promise instance를 리턴한다.

```javascript
const myFn = async (data) => data; 

myFn('MacBook');

// Promise instance 객체를 반환하므로 다음과 같이 사용이 가능하다
myFn('MacBook').then((name) => {
  console.log(name); // MacBook
});

```
![결과1](https://user-images.githubusercontent.com/15857404/59558689-d18db380-9032-11e9-8c01-830794a2b3a9.png)

* await은 오직 async로 정의된 함수안에서만 사용이 가능하다.

```javascript
// as-is
const myFn = (data) => {
  // 일반함수
  await fn();
  return data;
};

// to-be
const myFn = async (data) => {
  // async 함수
  await fn();
  return data;
};

myFn('MacBook');
```

![결과2](https://user-images.githubusercontent.com/15857404/59558731-cdae6100-9033-11e9-9d5a-b60ef63445fb.png)

* await을 사용하는 함수는 반드시 Promise 객체를 리턴해야 기대한 기능이 적용된다.

```javascript
// as-is (일반 함수)
const normalFn = (name, time) => {
  setTimeout(() => {
    console.log(name);
  }, time);
};
const myFn = async () => {
  await normalFn('1', 3);
  await normalFn('2', 2);
  await normalFn('3', 1);
};
// 결과: 3 -> 2 -> 1

// to-be(promise 객체 적용)
const doSomething = (name, time) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(name);
      // reject();
    }, time);
  });
};
const myFn = async () => {
  await doSomething('1', 3).then((res) => console.log(res));
  await doSomething('2', 2).then((res) => console.log(res));
  await doSomething('3', 1).then((res) => console.log(res));
}
// 결과: 1 -> 2 -> 3
```

### 해설
as-is를 보면 myFn으로 async와 await을 사용하더라도 우리가 기대한 동기적 동작을 하지 않는다. 따라서 to-be에서 Promise 객체를 리턴하여 우리가 기대한 동기적 실행결과 값을 얻게된다.즉 await을 지정한 함수는 Promise를 리턴해야 작업 수행을 (Promise의 resolved)종료 전까지 다음 작업을 수행하지 않는다.<br/>
이전 [Promise](https://yoosoo-won.github.io/javascript/2019/06/09/promise-and-callback-hell.html). 상태와 수행으로 await 동작을 보자면 다음과 같다.
1. await은 Promise객체를 품은 함수 상태가 unsettled이면 async함수 실행을 중단시킨다.
2. Promise 상태가 settled(resolved, rejected인경우 함수 실행 중단 상태 유지)으로 변경되면 async함수 실행이 재개된다.

## 결론
---
Promise도 마찬가지지만 async, await으로 콜백지옥을 탈출 시켜주지는 못한다. 상황에 따라서 적절히 사용하는것이 제일 중요하다고 생각한다. 이를 결정하는 요인은 Promise, async, await에 대한 이해와 깊은 자바스크립트 개발 경험이 중요하다고 믿는다.

## 참조
---
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await