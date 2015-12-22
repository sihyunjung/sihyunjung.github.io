---
layout: post
title:  "AngularJS 살펴보기..."
date:   2015-12-01
categories: javascript, AngularJS
---
## 들어가기에 앞서...<br>
 
> 출처<br>
http://d2.naver.com/helloworld/1172239 <br>
http://toddmotto.com/ultimate-guide-to-learning-angular-js-in-one-day

<br>
본내용은 개인적으로 제일 궁금하다싶은것들만 일단 추가함.
Backbonejs 위주로 다루(유지보수)었던 나로서 사실 AngularJS는(jQuery에서 쓰던 DOM 조작방식도좀 다르고.) 적응이좀 안되고 있다. 뭔가 진도도 안나가는거 같고... 구조도 어떻게 잡아야할거같고..;; 감이 좀 안오고있다. <br>
그래서 이제부터라도 방법을 바꿔 공부한 내용을 정리하고자 하는 차원에서 지킬의 힘을 빌려보고자한다.(쓸데없는 잡솔.-_-)<br>
사실 Backbonejs든 AngularJS든지간에 프레임워크라는건 개발시간을 단축시키기위한 도구일뿐 필수적인 요소 이상이라고는 생각하지는 않는다. <br>
단지 코어단 부터 개발을 붙이게된다면 개발 비용 및 유지보수까지의 시간이 너무나도 많이 든다는점이다.(물론 프레임워크를 제대로 사용하기위한 개발자의 비용과 시간이 또한 걸리게되고... 하나로 짜여진 일종에 통일화를 위해쓰는경우가 더 크다고 생각한다. 그래서 명명규칙이 나오는것이고...) <br>
결론은 이래나 저래나 개발자로서의 프레임워크의 대한 학습이 되엇든, 코어단으로부터의 개발이 되엇든지간에 가장 최종적인 목표는 사용하기 쉽고 유지보수가 간편하게 이우러질수있는 코드라면 잘짜여진 코드가 아닐까? 라는 생각(먼놈의 잡소리가 이리 길어.)<br>

 
## 기본개념<br>

#MVC 
프로그래밍 개발에 있어 소프트웨어의 구조 혹은 아키텍처를 구성할때 많이 사용되며 최근 프레임워크들은 이러한 패턴들을 많이 채용하고 있다.<br>

*** 모델(model)<br>**
데이터에 가까운 구조이다. 최근 서버와 데이터 통신하기위해 사용되는 key : value 형태로 이루어진 JSON을 많이 사용하고 있다.<br>
	
	{
		'collection' : [{
			'name' : '자바스크립트',
			'id' : 'javascript'
		},{
			'name' : '오브젝트C',
			'id' : 'objectC'
		},{
			'name' : '자바',
			'id' : 'java'
		}]
	}

*** 뷰(view)<br>**
뷰(view)는 HTML과 CSS의 렌더링이 마친 상태를 말함. 보통 모델(model)에서의 데이터를 이곳에서 display된다.<br>

*** 컨트롤러(controller)<br>**
모델(model)과 뷰(view) 중간사이에서 직접적인 연관이 있으며 사용자의 요구사항이나 요청에따라 그에 맞는 데이터를 모델(model)에 요구하여 결과를 뷰(view)에 반영하는 역활을 주목적으로한다.<br>
   
#AngularJS
위 내용에서 설명한 MVC 형태로 이루어진 SPA(Single Page Application) 애플리케이션을 개발 가능하도록 설계된 프레임워크중 하나이다.(추후 이문서가 정리되는대로 Backbonejs도 정리해보겠음..)<br>
특히나 컨트롤러(controller)와 디렉티브(Directive) 내에는 ng-app 아래에 각각의 트리구조형태의 scope가 존재함.<br>

**Getting Started with angular <br>**
HTML에서 ng-* 와 같은 형태로 정의되어 사용되며 스크립트에서 작성된 모듈과 컨트롤러등의 이름으로 매핑되어 사용됨.

	<div ng-app='myApp'>
		<div ng-controller='MyController'>
		<!-- myController logic-->
		</div>
	</div>

**Angular Module and controller**

	//모듈의 정의는 AngularJS 애플리케이션의 시작이며 기본단위이다.
	var myApp = angular.module('myApp', []);
	//의존성을 갖게되는 경우 
	//var myApp = angular.module('moduleName', ['dependencyName']); 배열형태로서의 의존성이 구성됨
	
	myApp.controller('MyController', ['$scope', function() {
		// MyController magic...
	}]);

Angular Module을 다음과 같은 chain형태로 사용가능하지만 유지보수와 디버깅의 어려움으로 인하여 좋아하지는 않는다.(개인적으로도..)

	angular.module('myApp', [])
	.controller('HeaderController', ['$scope', function($scope) {...}])
	.controller('LeftController', ['$scope', function($scope) {...}])
	.controller('SectionController', ['$scope', function($scope) {...}]);

위 내용을 다음과 같이 myApp이라는 네임스페이스를 선언하여 사용할수있다.

	var myApp = angular.module('myApp', []);
	myApp.controller('HeaderController', ['$scope', function() {...}]);
	myApp.controller('LeftController', ['$scope', function() {...}]);
	myApp.controller('SectionController', ['$scope', function() {...}]);

그리고 각각의 Service, Controller, Route, Directive, Factory 등 필요에 따라 파일을 생성하여 파일을 압축 및 하나의 스크립트로 생성하여 DOM에 사용함.

**Controller<br>**
컨트롤러는 모듈 내에 정의되며 ng-controller를 통하여 선언된 $scope를 통하여 DOM과 데이터 처리, 이벤트 핸들러 함수등이 이곳에서 정의됨.

	<div ng-app='myApp'>
		<div ng-controller='MyController'>
			//중괄호 연속해서 2개가 안되서 공백하나 넣음 
			{ {name}}
		</div>
	</div>
	
	var myApp = angular.module('myApp', []);
	myApp.controller('MyController',['$scope', function($scope) {
		$scope.name = '시현';
	}]);
	
위 예제에서 중요한 부분이 바로 $scope이다. $scope는 현재의 DOM을 의미하며 this와는 다른개념으로 동작됩니다. 이를 통하여 데이터를 set하거나 get을하며 function이 가능합니다.<br>
아래 예제는 위에제에서 이벤트 부분만 추가 구현한 내용이다.

	<div ng-app='myApp'>
		<div ng-controller='MyController'>
			<button ng-click='getName();'>{ {name}}</button>
		</div>
	</div>
	
	var myApp = angular.module('myApp',[]);
	myApp.controller('MyController',['$scope', function() {
		$scope.name = '시현';
		
		$scope.getName = function() {
			alert($scope.name);
		}
	}]);
	
아래는 예제는 static 형태의 객체를 받아서구현한 내용이다. 

	var myApp = angular.module('myApp', []);
	myApp.controller('MyController', ['$scope', function() {
	
		//$scope내에서의 user 네임스페이스를 생성하여 detilas객체로서 활용
		//DOM의 시점에서 좋을듯...
		
		$scope.user = {}
		$scope.user.details = {
		'userName' : '시현',
		'userId' : 'SiHyun'
		}
		
	}]);
	
DOM을 통하여 user 네임스페이스가 각각의 프로퍼티를 통해 display됨. 

	<div ng-app='myApp'>
		<div ng-controller='MyController'>
			<p class='userName'>안녕? { {user.details.userName}}</p>
			<p class='userId'> user ID : { {user.details.userId}}</p>
		</div>
	</div>

특별한 케이스가 아니라면 아래 예제처럼은 코딩하지 말것. 전역 네임스페이스가 더럽혀지거나 전역함수가 되어버림.

	var myApp = angular.module('myApp',[]);
	
	function MyController() {
		...
	}
	
**Directive<br>**
디렉티브는 AngularJS에서 제공되는 템플릿 HTML의 작은 조각같은 형태이다. 어렵지만 저는 일단 AngularJS의 HTML에서의 DOM 엘리먼트로 이해하는것으로 받아들임.-_-(ㅠㅠ). <br>

	<!-- 1. 속성을 이용한 선언-->
	<a custom-button>click me</a>
	
	<!-- 2. 커스텀 엘리먼트 -->
	<custom-button>click me</custom-button>
	
	<!-- 3. 클래스 (구형 브라우저에서의 지원을 위해..) -->
	<a class='custom-button'> click me </a>
	
	<!-- 4. 주석으로 정의 (추천X)-->
	<!--directive: custom-button-->
	
다음은 .directive() 메소드를 통하여 나의 directive를 정의하고 이름 'customButton'에 전달하면 AngularJS DOM에서는 위와 같이 custom-button으로 정의됨.

	myApp.directive('customButton', function() {
		return {
			restrict : 'A', //요소를 어떻게 사용할것인가의대한 정의 - E, A, C, M
			replace : true, //요소를 추가할지 교체할지 결정.
			transclude : true, //ng-transclude기준으로 template를 원본에 포함 유무 결정.
			template : '<a>!!!</a> ',
			link : function(scope, el, attrs) {
				//DOM, 이벤트 생성은 여기서하면됨.
			}
		}
	});
	
각 몇가지 속성을 다음과 같이 정리.

*** restrict : 디렉티브를 사용에 있어 다음과 같은 옵션을 이용하여 엘리먼트의 속성을 지정.<br>**
A : attribute

	<a custom-button>click me</a>
	
E : element

	<custom-button>click me</custom-button>
	
C : class

	<a class='custom-button'>click me</a>
	
M : comment 

	<!--directive : custom-button-->

*** replace : 디렉티브를 기준으로 template, templateUrl에 작성된 내용으로 교체.<br>**
true

	<div ng-app="myApp" class="ng-scope">
	    <button custom-button="" href="">replace test</button>
	</div>
	
false

	<div ng-app="myApp" class="ng-scope">
	    <a custom-button="" href=""><button>replace test</button></a>
	</div>
	
*** transclude : 디렉티브의 ng-transclude를 기준으로 template를 원본에 포함 유무를 결정.<br>**
true

	<div ng-app="myApp" class="ng-scope">
	    <a custom-button="" href="">
	        <button>replace test</button>
	        <span ng-transclude="">
	            sihyun
	            <span class="ng-scope">Click me</span>
            </span>
        </a>
	</div>
	
false

	<div ng-app="myApp" class="ng-scope">
	    <a custom-button="" href="">
	        <button>replace test</button>
            <span ng-transclude="">
                sihyun
            </span>
        </a>
	</div>

*** template : 디렉티브 내에 html을 사용(개인적으로 좋아하지 않음)<br>**

	myApp.directive('customButton', function() {
		return {
			template : '<button>click!!</button> '
		}
	});
	
*** templateUrl : 별도의 html파일 경로를 정의함.<br>**

	myApp.directive('customButton', function() {
		return {
			templateUrl : 'templates/customButton.html'
		}
	});
	

템플릿 파일 내용

	<!-- include customButton.html -->
	<a href='' class='myawesomebutton' ng-transclude>
		<i class='icon-ok-sign'></i>
	</a>
	
템플릿을 하게되면 캐시를 하게되는데 이를 원지 않는다면 <script> tag를 이용하면됨.

	<script type='text/ng-template' id='customButton.html'>
		<a href='' class='myawesomebutton' ng-transclude>
			<i class='icon-ok-sign'></i>
		</a>
	</script>



**Services<br>**
서비스는 AngularJS에서 사용되는 Class를 정의하며 첫번째 인자의 키값을 두번째 인자로서는 AngularJS에서 첫번째 인자의 키값을 이용하여 만들 싱글톤 객체입니다.

	myApp.service('Math', function() {
		//인스턴스화될 생성자 함수이므로 this사용가능
		this.multiply = function(x,y){
			return x*y;
		}
	});
	
다음처럼 사용하시면 됩니다.

	//Math를 넣어 서비스를 사용하도록 한다.
	myApp.controller('myController', ['$scope', 'Math', function($scope, Math) {
		var a = 12,
			b = 24;
			
			//result 288
			var result = Math.multiply(a,b);
	}]);



**Factories<br>**
팩토리는 서비스와 반대로 싱글톤이 아닌 객체 자체를 이용하여 dependency하는 방식입니다.

	myApp.factory('Server', ['$http', function($http) {
		return {
			get : function(url) {
				return $http.get(url);
			},
			post : function(url) {
				return $http.post(url);
			}
		}
	}]);
	
다음처럼 사용하시면 됩니다.

	myApp.controller('myController', ['$scope', 'Server', function($scope, Server) {
		var jsonGet = 'http://myserver/getURL',
			jsonPost = 'http://myservrver/postURL';
			Server.get(jsonGet);
			Server.post(jsonPOST);
	}]);



**Routing<br>**
AngularJS는 Backbonejs처럼 Routing을 지원한다. 즉 브라우저 URL에 따라 template, controller 대응이 가능하다.
	
	//route를 사용하기위해 $routeProvider를 이용한다.
	myApp.config(['$routeProvider', function($routeProvider) {
		$routeProvider
		.when('/',{
			templateUrl : 'views/main.html'
		})
		.when('/emails',{
			templateUrl : 'views/emails.html'
		})
		.otherwise([
			redirectTo : '/'
		]);
	}]);


