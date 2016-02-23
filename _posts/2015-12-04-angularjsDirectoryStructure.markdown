---
layout: post
title:  "AngularJS 폴더 구조 모범 사례"
date:   2015-12-03
categories: javascript, AngularJS
---

 
> 출처 및 참고 사이트<br>
https://scotch.io/tutorials/angularjs-best-practices-directory-structure <br>
<br>


## 문서를 만들기까지...
본 내용은 보다 좋은 AngularJS 개발 폴더 구조 설계를 위해 상기 블로그 내용들을 참고한 내용이며 중요도가 다소 떨어진다 싶은 부분은 생략하거나 영어가 짧아(-_-)번역이 안되거나 이상한글로 대체?한 부분과 제가 납득이 안되는 부분의 대해서 일부 생략된 부분이  있을 수 있습니다. 프로젝트 특성에 맞춰 적용하면 좋은 내용이 될것같습니다.
잘못된 내용이나 추가 사항이 있으신분 께서는 아래 메일로 의견 주시면 반영하도록 하겠습니다.

![logo](https://angularjs.org/img/AngularJS-large.png)


## 표준 구조
AngularJS를 포함한 많은 폴더가 아래와 같은 구조를 보이고 있습니다.
	
	app/
	---- controllers/
	-------- mainController.js
	-------- otherController.js
	---- directives/
	-------- mainDirective.js
	-------- otherDirective.js
	---- services/
	-------- userService.js
	-------- itemService.js
	---- js/
	-------- bootstrap.js
	-------- jquery.js
	---- app.js
	views/
	---- mainView.html
	---- otherView.html
	---- index.html
	
이러한 구조는 아주 일반적으로 많이 사용되고 있습니다. 겉보기엔 의미있고 MVC 프레임워크와 아주 유사해 보입니다. 관련된 폴더단위로 controller, view, etc 등으로 이루어져 있습니다.

소수인원이나 작은 단위의 프로젝트가 진행이 될때에는 이 구조가 크게 문제되는부분이 없을 수 있습니다. 시실, 이것은 튜토리얼을 작성하거나 작은 단위의 프로젝트, 애플리케이션을 개발할때 수행할때 바람직합니다.
이와 같은 구조는 개발자에게 보고 개념을 이해하는데 있어 아주 쉽게 접근할 수 있습니다.

하지만 이방법은 개발자가 기능의 대한 추가가 발생한다면 이러한 방법은 어려움이 따름니다. 일단 10개이상의 controllers, views 그리고 directives 필요한 폴더들을 펼치고 디렉토리 트리에서 스크롤의 압박을 받게 될것입니다.

예를 들어 AngularJS로 이루어진 블로그에 기사의 바닥글에 저자 정보를 추가 할 것을 생각한다면, 블로그의 전체적인 구조를 보고 directive, controller, 경우에따라 service 와 view 를 찾아 수정할 것입니다.

그리고 기능 추가하거나 이름을 변경하고, 다시 관련 디렉토리 구조를 통해 관련 파일을 찾아 편집하며 변경사항을 확인 및 적용합니다.

## 더 나은 구조 설계
모범사례와 구축 및 설계인하여 AngularJS 앱 유지보수 와 협업에 있어 동료 개발자들에게 있어 아주 좋은 작용하게 될것입니다.
AngularJS 앱 개발에 있어 모범사례와 구축을 하게된다면, 협업하는 동료 혹은 개발자들에게 아주 유리하게 작용하게 될것입니다. 또한 AngularJS 앱 구조는 기능이 분명한것의 대해서 모듈화가 이루어져야하며, 이를 구분 지을 수 있는 좋은 Directives의 활용을 원합니다.
아래 샘플을 살펴 보시기 바랍니다.
	
	app/
	---- shared/
	-------- sidebar/       // 재사용이 가능 컴포넌트나 사이트 전체를 구분짓는 요소
	------------ sidebarDirective.js
	------------ sidebarView.html
	-------- article/
	------------ articleDirective.js
	------------ articleView.html
	---- components/       // 작은 단위의 Angular 앱이 구성이되며 보통 페이지단위로 구성 할 수 있음.
	-------- home/
	------------ homeController.js
	------------ homeService.js
	------------ homeView.html
	-------- blog/
	------------ blogController.js
	------------ blogService.js
	------------ blogView.html
	---- app.module.js
	---- app.routes.js
	assets/
	---- img/       // 앱을 구성하는 이미지와 아이콘(별도의 서버로 구성되어 위와같은 폴더 트리를 구성한다면 더욱 좋음).
	---- css/       // 앱을 구성하는 css 파일(별도의 서버로 구성되어 위와같은 폴더 트리를 구성한다면 더욱 좋음)(별도의 서버로 구성되어 위와같은 폴더 트리를 구성한다면 더욱 좋음).
	---- js/        // AngularJS 이외에 별도로 앱을 이루는 파일
	---- libs/      // 기타 필요에 따라 사용되는 라이브러리
	index.html
	
이 구조를 읽고 이해하는데 있어 더욱 어렵게 느껴질 수 있지만 신입에게는 이전의 튜토리얼과 예시를 참조하게 된다면 도움이 될것입니다. 위 디렉토리 구조에서의 역할은 다음과 같습니다.

# index.html
프론트 엔드 구조의 root에 포함 되어 있으며, 주로 AngularJS와 라이브러리 로드를 담당합니다.

# Assets Folder
Assets 폴더 좋은 표준이며, AngularJS와 관련 없는 App 내에 필요한 내용들이 포함됩니다.

# App Folder
AngularJS 프로젝트내에 폴더와 자바스크립트로 이루어진 파일들이 포함됩니다. 
app.module.js 파일은 AngularJS 의존성, 앱 설치를 담당하며, app.route.js 파일은 모든 라우트 지정 및 설정을 담당합니다.
위 예제에는 components와 shared로 구성되어있지만 협업에 따라 알맞게 구성하면되며, 일단 두 폴더의 기능들을 살펴봅니다.

# Components Folder
Components 폴더는 Angular App에서의 실질적인 컨텐츠로 구성됩니다. 이들은 정 views, directives 와 services 로 이루어진 사이트내의 특정 부분을 이룹니다. 즉 각각의 페이지를 이루는 폴더와 그 안을 이루는 controller, services 와 HTML 파일이 존재합니다.
여기에서 각각의 컴포넌트는 view, controller, 필요에따라 services 파일을 구성하여 작은 단위의 MVC 앱과 유사한 모습을 보입니다.
만약 관련 컴포넌트 파일들이 존재하는 경우 views, controllers, services 이름을 구성하여 이들을 하위 폴더에 파일들을 포함하는것이 좋습니다.

이내용은 이전에 보여진 부분단위로 나뉜폴더 구조와 유사하다. 그래서 기본적으로 큰 AngularJS 앱 안에서의 여러 작은 단위의 AngularJS 앱으로 생각할 수 있다.

# Shared Folder
Shared 폴더는 각각의 개별 기능을 포함되며, 여러페이지에서 재사용하게되는 부분입니다.

특히 각각의 구성을 이루는 부분의 경우 AngularJS Directives로 작성되어야합니다. 위와 마찬가지로 각각의 컴포넌트들은 마찬가지로 이들을 포함하는 폴더가 있어야하며, 자바스크립트 파일과 html 템플릿이 이루게 됩니다.

## 모범사례
AngularJS로 이루어진 큰 앱을 개발하는데 있다면, 최대한 모듈화 할 것입니다. 다음은 내용은 이 작업을 수행하는데 있어 몇가지 추가 도움말입니다.

# Header와 Footer(App Container)의 모듈화
좋은 방법은 components 아래에 core 서브폴더를 생성하고, 그러면 서브폴더의 Header 와 Footer 그리고 컴포넌트가 추가 될때마다 여러페이지에걸처 공유될것입니다. 

# 압축
Grunt, Gulp등을 이용한 코드압축이 가능하며 따라서 코드 분할의 대한 걱정이 없습니다.

하지만 반드시 전체의 AngularJS App의 대해서 큰 .js 파일을 원하지 않고 논리적인 파일 연결을 원할 수 있습니다.

* app.js (앱 초기화, 설정과 라우팅)
* services.js (모든 서비스)

이부분의 대해서는 AngularJS app의 대한 초기 로딩 감소의 대해서 도움이 될것입니다.

만약 압축의 대해서 더 필요한 부분은이 있다면 [Declaring AngularJS Modules For Minification](https://scotch.io/tutorials/declaring-angularjs-modules-for-minification) 참고 하시기 바랍니다.

# 일관성 네이밍
이부분에 팁을 더한다면 추후 구성요소를 작성하거나, 추가적인 파일들이 추가가 될때 일관성있는 네이밍을 사용하여 두통에서 벗어날 수 있습니다.
ex : blogViewhtml, blogServices.js, blogContainer.js

## 모듈화 접근 방식 장점
상기 예제는 AngularJS 구축의 모듈화 접근 방법의 대한 예입니다. 이 접근 방법은 다음과 같은 장점이 있습니다.

# 코드 유지보수
논리적인 구분으로 인하여 위 내용에 따라 쉽게 찾고 수정이 가능합니다.

# 추가 확장
코드 확장작업에 있어 아주쉽게 가능해지며, 새로운 추가 개발자의 대해서 구조의 대해서 설명을 한다면 쉽게 접근 할 수 있습니다. 또한 쉽게 새로운 기능을 테스트하거나 제거 할 수 있습니다.

# 디버깅
모듈화된 접근 방식으로 인하여 코드 디버깅이 훨신 쉬워집니다.

# 테스팅
테스트 스크립트를 작성하고 테스팅을하는 부분이 훨씬 쉽습니다.