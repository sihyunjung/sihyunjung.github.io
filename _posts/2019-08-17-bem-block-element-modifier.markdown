---
layout: post
title: "CSS 방법론(BEM)"
date:   2019-08-17
categories: HTML, CSS
---

## 기본
---
프론트엔드 개발시 불과 10년, 15년전 이전으로만 거슬러 올라가더라도 대부분의 css의 스타일은 inline형태로 tag에 직접 작성하였다. 그리고 대부분은 table tag로 구성이 되었으며, 재사용이나 확장에 대한 개념이 대부분 없었다. 또한 많은 사이트들이 이미지로 구성되어 있기도 하였다. 하지만 시간이 지나고 HTML, JAVASCRIPT CSS발전에 따라 관리와 유지 보수 등 이 중요해지기 시작하였고, 특히 선택자, animation등 다양하게 지원이 되며 CSS코드량이 폭팔적으로 늘어나기 시작한다. 그래서 CSS파일을 효율적으로 관리하거나 유지보수를 위해서 나오게된 CSS방법론에 대해서 이야기해보려한다. CSS방법론에는 대표적으로 다음과 같다. 이 방법론들은 유지보수를 쉽고, 재사용이 가능하게한다. 최근에 나는 회사에서 적용하고 있는 BEM(Block Element Modifier)에 대해서 다뤄보려한다.

> BEM<br>
> SMASS<br>
> OOCSS

위 방법론들은 기본적으로 아래 사항들을 지향하고 있다.
> 쉬운 유지보수<br>
> 코드의 재사용<br>
> 손쉬운 확장<br>
> 명시적인 네이밍

이전의 inline과 table을 사용하여 개발 예
```html
<table style="margin:0; padding:0; background-color:red; font-size: 13px;">
  <thead>...</thead>
  <tbody>...</tbody>
</table>
```

## BEM이란?
---
BEM은 Block, Element, Modifier를 말한다. 깔끔하고 읽기 쉬운 CSS를 작성하기 위한 CSS 명명 규칙이다. 또한 손쉬운 확장과, 재사용을 위해 독립적인 CSS 블록작성을 위해 지향합니다. 작명규칙은 다음과 같다.
> CSS 선택자 이름을 가능한 명시적으로 작명한다.<br>
> 소문자, 숫자만을 이용하며, 여러단어의 조합은 하이픈(-)을 이용한다.<br>
> ID는 사용할 수 없고, 클래스만 사용한다.

```html
<!--일반적인 클래스명이 사용된다.-->
<div class="block"></div>

<!--block뒤에 2개의 underscore와 선언 된 element-->
<div class="block__element"></div>

<!--block뒤에 선언된 2개의 dash와 선언 된 modifier-->
<div class="block--modifier"></div>

<!--element와 modifier-->
<div class="block__element--modifier"></div>

```


## Block(블록)
---
재사용이 가능한 독립적 페이지 단위의 구성요소이다.

* 가장 큰 조각이며, 독립적이고, 재사용이 가능해야한다. <br>
* 다른 block이나 element에 의존하지 않습니다. <br>
* block은 환경에 영향을 받아서는 안되므로, 여백이나 위치를 설정해서는 안된다. <br>
* block은 서로 중첩이 가능하며, 다양한 레벨로 중첩하여 사용이 가능하다. <br>
* block은 elements와 modifiers가 포함된다. <br>
* 상태(red, big)가 아닌 목적(`menu`, `button`)에 맞게 설계되어야한다.
<br>
위 사항들이 지켜진다면 블록을 재사용하거나, 다른 장소에서도 독립성이 보장된다.

가장 대표적으로 `<header>`, `<nav>`, `<article>`, `<section>`, `<form>`, `<footer>` 등이 있다.

![구조](https://user-images.githubusercontent.com/15857404/63205842-6d947380-c0e5-11e9-9435-0d0047540c70.png)

**Ex )**
```html
<div class="content">
  <section class="section content__section"></section>
  <article class="article content__article"></article>
</div>

```
**추가 설명**
위 예제에서 block이 block의 element로 사용되는 경우 block-name__block-name으로 사용된다. 즉 section과 article은 다음과 같다.
> content의 element는 section과 article은 각각은 block이지만 content의 element가 될 수 있다.


## Elements(요소)
---
별도로 사용이 불가능한 block의 요소
* element는 block의 구성이며, 여러개의 elements를 가질 수 있다. <br>
* `.content__section` 형태로 사용되며 double underscore로 구성된다. `block-name__element-name`<br>
* 다른 block의 elements에서 사용 할 수 없다. 즉 하나의 block만을 가질 수 있다.<br>
* 상태(red, big)가 아닌 목적(`item`, `text`, etc...)에 맞게 설계되어야한다.
* 아래와 같이 사용 할 수 없다.

```html
<div class="content">
  <div class="content__section">
    <!--good-->
    <div class="content__item"></div>

    <!--bad-->
    <div class="content__section__item"></div>
  </div>
</div>
```

* 요소는 항상 블록의 일부이므로 블록과 별도로 사용이 불가능하다

```html
 <!--good-->
<div class="content">
  <div class="content__section">
    <div class="content__item"></div>
  </div>
</div>

<!--bad-->
<div class="content">
...
</div>
<div class="content__section">
...  
</div>
<div class="content__item">
...
</div>
```

## Modifiers(수식어)
---
block or element의 모양, 상태, 또는 동작을 정의한다.
* `block--modifier or block__element--modifier`형태로 사용된다.
* modifier단독으로는 사용 할 수 없다.
* modifier는 Boolean type과 key-value type이 존재한다.
* Boolean type의 경우 true라고 가정한다.

```html
<!--boolean type-->
<div class="content content--focused">
  <div class="content__section content__section--disabled"></div>
</div>

<!--key-value type-->
<div class="content content--size-10">
  <div class="content__section content__section--color-red"></div>
</div>
```

## 참조
---
https://en.bem.info/methodology/quick-start <br>
http://getbem.com/naming <br>
https://codeburst.io/understanding-css-bem-naming-convention-a8cca116d252

