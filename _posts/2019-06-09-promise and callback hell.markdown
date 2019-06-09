---
layout: post
title:  "Promise와 Callback Hell"
date:   2019-06-09
categories: javascript
---

> Callback Hell(골백지옥) <br/>
Promise 정의<br/>
Promise 사용<br/>
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
						// 상황에 따라 더 생길지도....
					}, id);
				}, id);
			}, id);
		}, resource.id);
	}
);
```
