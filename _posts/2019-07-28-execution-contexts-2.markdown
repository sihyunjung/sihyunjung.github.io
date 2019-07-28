---
layout: post
title:  "자바스크립트 실행콘텍스트와 동작원리 -2부- (콘텍스트 코드 실행 단계 - 코드 진입)"
date:   2019-07-28
categories: javascript, execution contexts
---

## 기본
---
지난 시간에 이어 이번 시간에는 실행 콘텍스트의 코드 실행의 2가지 단계에 대해서 다뤄본다.

> 1. 실행 콘텍스트 진입<br/>
> 2. 코드 실행

## 전역 객체에 대해서...
---
자바스크립트 코드가 실행 콘텍스트로 진입하기 이전에 전역 객체를 생성한다. 이 때(전역 객체 생성단계에서) 전역 객체는 초기화되면서 Math, String, Date, parseInt와 BOM 객체를 가진 property가 설정된다.(실행 환경에 따라 달라 질 수 있다)

> 전역 객체는 단일 사본으로 존재하며, 이 객체는 프로그램의 어떠한 곳에서도 접근할 수 있다.

전역 객체가 생성된 이후 전역 코드의 경우 전역 실행 콘텍스트가 ECStack에 쌓이고, 함수이면 함수 실행 콘텍스트가 ECStack 쌓이게 된다.

> 전역 실행 콘텍스트가 ECStack에 쌓이고 전역 실행 콘텍스트 코드가 실행 될 때, 함수 실행 콘텍스트가 ECStack에 쌓이게 된다. 이 때 실행 콘텍스트 진입과 코드 실행이 반복된다.

## 실행 콘텍스트 진입
---
실행 콘텍스트가 ECStack에 쌓이고 나면 다음과 같은 처리를 한다.

> 스코프 체인의 생성과 초기화
> Variable Object 초기화
> this 결정

#### 스코프 체인의 생성과 초기화
스코프 체인의 생성과 초기화 과정에서 전역콘텍스트와 함수콘텍스트에 따라 조금 다르다.
##### 전역 콘텍스트
전역 콘텍스트는 최초에 ECStack 쌓이므로 ScopeChain(SC)에는 자기 자신만 쌓이게 된다.

![전역 콘텍스트의 스코프 체인 생성과 초기화](https://user-images.githubusercontent.com/15857404/62003979-1e989580-b15a-11e9-95d4-936db7862ebb.png)
그림 전역 콘텍스트의 스코프 체인 생성과 초기화

##### 함수 콘텍스트

![함수 scope chain 생성과 초기화](https://user-images.githubusercontent.com/15857404/62004207-0bd39000-b15d-11e9-9e73-20222e35065d.png)
그림 함수 scope chain 생성과 초기화

![scope chain에 push](https://user-images.githubusercontent.com/15857404/62004210-0f671700-b15d-11e9-8a3d-867d3bd5a959.png)
그림 scope chain에 push

#### Variable Object 초기화
스코프 체인의 생성과 초기화(함수의 경우 scope chain에 push까지) 후 VO가 초기화 되는데 전역 코드의 경우 GO(Global Object), 함수 코드의 경우 AO (Activation Object)를 가르킨다. 이 때 함수의 경우 매개변수가 전달 받은 인자값으로 값이 결정된다. 그리고 함수 호이스팅과 변수 호이스팅이 일어난다.

![전역 코드의 경우 GO](https://user-images.githubusercontent.com/15857404/62004250-800e3380-b15d-11e9-9893-cfb2899d3ff1.png)
그림 전역 코드의 경우 GO를 가르킨다.

![함수 코드의 경우 AO](https://user-images.githubusercontent.com/15857404/62004258-a338e300-b15d-11e9-9a98-6daf6a148e49.png)
그림 함수 코드의 경우 AO를 카르킨다.

#### this 결정
변수 초기화까지 끝나면 최종적으로 [this를 결정](https://yoosoo-won.github.io/javascript,/this/2019/06/23/this.html)하게 되는데, this의 값은 함수 호출 방법에 결정된다. 기본적으로 함수 이름 앞에 식별자가 존재하지 않는다면 모든 콘텍스트는 GO를 가르키게된다.

## 결론
---
지금까지 콘텍스트가 EC에 쌓이고 진입단계의 대해서 다루었다. 다음에 마지막으로 실행에 대해서 언급하고자한다.

## 참조
---
http://dmitrysoshnikov.com/ecmascript/chapter-1-execution-contexts/
http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/
http://dmitrysoshnikov.com/ecmascript/chapter-3-this/
http://dmitrysoshnikov.com/ecmascript/chapter-4-scope-chain/
http://dmitrysoshnikov.com/ecmascript/chapter-5-functions/
http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/
https://www.codebyamir.com/blog/javascript-execution-contexts