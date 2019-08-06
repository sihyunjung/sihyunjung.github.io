---
layout: post
title:  title:  "자바스크립트 실행콘텍스트와 동작원리 -2부- (콘텍스트 코드 실행 단계 - 코드 실행)"
date:   2019-08-04
categories: javascript, execution contexts
---

## 기본
---
지난 시간에는 콘텍스가 Stack쌓이고 Execution Context가 초기화하는 과정을 중심으로 작성하였다. 오늘은 초기화 후 실행에 대해서 언급하려한다.
지난 시간에 이어 다시 한번 복습하자면
1. 제어가 전역 코드를 만나기전 GO(Global Object)를 생성한다.
2. Scope Chain 생성 및 초기화
3. Scope Chain에 실행 콘텍스트 type에 따라 Go or AO(Activation Object)가 쌓이는 리스트 형태로 추가
4. VO(Variable Object)을 실행 콘텍스 type에 따라 GO or AO를 가르킴
5. AO의 경우 VO에 함수 parameter가 추가된다.
6. this 결정(전역 실행 콘텍스트의 경우 항상 전역을 가르키며, 함수는 실행 방법에 따라 this가 결정된다)

아래를 반복
> 1. Scope Chain 생성, 초기화 및 GO or AO 리스트 형태로 생성
> 2. VO(Variable Object)을 VO or AO 객체에 연결
> 3. this 결정(전역은 항상 GO이며, function의 경우 실행 방식에 따라 this가 결정됨)

## Execution Context의 실행
---
위 3단계가 끝나고나면 콘텍스트가 비로소 실행이 되는데 이때 다음을 수행한다.
1. VO property에 변수 값 할당
2. 함수가 존재시 다시 콘텍스트 실행

## 총 정리
---
![JS 동작 최종](https://user-images.githubusercontent.com/15857404/62422753-5a030900-b6f2-11e9-84a0-13161fb3ee22.png)
## 결론
---
지금까지 JS동작원리에 대해서 살펴보았다. 이러한 내용은 자바스크립트 개발하는데 있어서 많은 도움이 될 것이다.