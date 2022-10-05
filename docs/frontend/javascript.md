---
title: JavaScript
sidebar_label: JavaScript
sidebar_position: 3
toc_min_heading_level: 3
toc_max_heading_level: 3
---

import TOCInline from '@theme/TOCInline';

<TOCInline toc={toc} />

***

### AJAX

#### AJAX란 무엇인가?

Asynchronous Javascript And XML의 약자로, 비동기적으로 JS를 사용해서 데이터를 받아와 동적으로 DOM을 갱신 및 조작하는 웹 개발 기법을 의미한다. 여기서 XML이 있는 이유는 예전엔 데이터 포맷으로 XML을 많이 사용했기 때문이다.

<br/>

#### 어떻게 동작하는가?

사용자가 AJAX가 적용된 UI와 상호작용하면, 서버에 AJAX 요청을 보내게 된다. 서버는 DB에서 데이터를 가져와서 JS 파일에 정의되어 있는 대로 HTML/CSS와 데이터를 융합하여 만든 DOM 객체를 UI에 업데이트 시킨다. 비동기로 이루어지며, 기존의 페이지를 전부 로딩하는 방식이 아닌 **일부만 업데이트 하는 방식이다.**

<br/>

#### 어떻게 사용하는가?

##### XMLHttpRequest

일반적으로 `XMLHttpRequest` 객체를 사용하여 인스턴스를 만들어 인스턴스의 `open()` , `send()` 등의 메소드를 이용한다. 

```javascript
var ourRequest = new XMLHttpRequest();
ourRequest.open(
  "GET",
  "https://learnwebcode.github.io/json-example/animals-1.json"
);
ourRequest.onload = () => {
  var ourData = JSON.parse(ourRequest.responseText);
  console.log(ourData[0]);
};
ourRequest.send();
```

`open()` 으로 요청할 메소드와 URL을 설정한 뒤, 모두 로드되었을 경우의 콜백함수를 초기화한다.

##### Fetch API

새로나온 `fetch` 를 사용해서 요청을 할 수도 있는데 IE를 지원하지 않는다는 점을 제외하고는 `XMLHttpReqeust` 보다 훨씬 직관적이다. ES6(ES2015) 에서 표준이 되었고, Promise를 리턴한다.

```javascript
fetch("https://learnwebcode.github.io/json-example/animals-1.json")
	.then(res => res.json())
	.then(resJson => console.log(resJson));
```

응답객체는 `json()` , `blob()` 과 같은 내장 메서드로 body를 추출해내고 이는 다시 Promise를 리턴한다.

CodeSandBox를 통해서 둘을 비교해보자.

[![Edit XMLHttpRequest & Fetch API](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/fetch-api-dzqoj?fontsize=14&hidenavigation=1&theme=dark)

<br/>

#### 장단점

##### 장점

* 페이지를 전환하지 않고 빠르게 화면 일부분 업데이트 할 수 있다.
* 수신하는 데이터 양을 줄일 수 있고 클라이언트에게 처리를 맡길 수 있다.
* 서버 처리를 기다리지 않고 비동기 요청이 가능하다.

##### 단점

* 지원하지 않는 브라우저가 있다.
* 페이지 전환없이 서버와 통신을 하기 때문에 보안상에 문제가 있을 수 있다.
* 무분별하게 사용하면 역으로 서버의 부하가 늘어날 수 있다.
* [동일 출처 정책](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/security/sop.md) 문제가 발생할 수 있다.

<br/>

#### 참고

* [JSON and AJAX Tutorial: With Real Examples(유튜브)](https://www.youtube.com/watch?v=rJesac0_Ftw)
* [How is Ajax independent from a server?](https://www.quora.com/How-is-Ajax-independent-from-a-server)
* [MDN, Ajax 시작하기](https://developer.mozilla.org/ko/docs/Web/Guide/AJAX/Getting_Started)
* [위키백과, Ajax](https://ko.wikipedia.org/wiki/Ajax)
* [Stackoverflow, Difference between fetch, ajax, and xhr](https://stackoverflow.com/questions/52261136/difference-between-fetch-ajax-and-xhr)
* [AJAX, XMLHttpRequest와 Fetch 살펴보기](https://junhobaik.github.io/ajax-xhr-fetch/)

***

### 이벤트 위임

#### 이벤트 버블링과 캡쳐링

이벤트 위임을 알기 위해선 선수지식으로 이벤트 버블링과 캡쳐링의 동작방식을 알아야 한다.

**이벤트 버블링** 이란, 하위 엘리먼트에 이벤트가 발생할 때 그 엘리먼트부터 시작해서 상위요소까지 이벤트가 전달되는 방식을 말한다.

**이벤트 캡쳐링** 이란, 하위 엘리먼트에 이벤트 핸들러가 있을 때 상위 엘리먼트부터 이벤트가 발생하기 시작해서 하위 엘리먼트까지 이벤트가 전달되는 방식을 말한다.

아래 예시를 보면 확실히 알 수 있다.

```html
<div>
  <ul>
    <li>예시</li>
  </ul>
</div>
```

```javascript
document.querySelector('li').addEventListener('click', () => console.log('li 클릭'));
document.querySelector('ul').addEventListener('click', () => console.log('ul 클릭'));
document.querySelector('div').addEventListener('click', () => console.log('div 클릭'));
```

이렇게 각 엘리먼트들에 이벤트 핸들러를 달고 `li` 태그를 클릭해보면 콘솔에는 아래와 같이 찍히게 된다.

```
li 클릭
ul 클릭
div 클릭
```

즉, 기본적으로는 동작방식이 "이벤트 버블링" 인 것이다. 이 방식을 "이벤트 캡쳐링" 으로 바꾸기 위해선 `addEventListener()` 의 마지막 인자로 `{ capture: true }` 를 전달해주면 된다. 이렇게 하면, 클릭한 엘리먼트의 상위요소 중 이벤트 캡쳐링이 적용된 엘리먼트부터 콘솔에 찍히고 그 다음부터는 다시 "이벤트 버블링" 방식을 동작한다. `ul` 의 이벤트 핸들러에 캡쳐링을 적용해보자.

```javascript
document.querySelector('ul').addEventListener('click', () => console.log('ul 클릭'), { capture: true });
```

```
ul 클릭
li 클릭
div 클릭
```

`li` 부터는 다시 "이벤트 버블링" 이 동작해서 `div` 에 이벤트가 발생하는 것이다.

<br/>

#### 이벤트 위임

**이벤트 위임** 이란 하위 엘리먼트들이 여러개 있을 때, 하위 엘리먼트들에 각각 이벤트 핸들러를 달지 않고 상위 엘리먼트에 이벤트 핸들러를 달아 하위 엘리먼트들을 제어하는 방식이다. 이 패턴으로 얻는 이점들은 다음과 같다.

* 동적으로 엘리먼트를 추가할 때마다 핸들러를 고려할 필요가 없다.
* 상위 엘리먼트에 하나의 이벤트 핸들러만 추가하면 되기 때문에 코드가 훨씬 깔끔해진다.
* 메모리에 있게되는 이벤트 핸들러가 적어지기 때문에 퍼포먼스 측면에서 이점이 있다.

예시를 살펴보자.

```html
<ul onclick="alert(event.type + '!')">
    <li>첫번째</li>
    <li>두번째</li>
    <li>세번째</li>
</ul>
```

`li` 에 각각 핸들러를 달지 않고 상위 엘리먼트인 `ul` 에만 달았다는 걸 확인할 수 있다. 무조건 이벤트 위임이 좋은 것은 아니기 때문에 상황에 맞게 잘 사용하도록 하자.

<br/>

#### 참고

* [이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
* [JavaScript Event Delegation is Easier than You Think](https://www.sitepoint.com/javascript-event-delegation-is-easier-than-you-think/)
* [What is DOM Event delegation?](https://stackoverflow.com/questions/1687296/what-is-dom-event-delegation)


***

### 실행 컨텍스트 (Execution Context)

####  정의

**코드의 실행환경에 대한 여러가지 정보를 담고 있는 개념** 으로, 간단히 말하자면 자바스크립트 엔진에 의해 만들어지고 사용되는 코드 정보를 담은 객체의 집합이라고 할 수 있다.

<br/>

#### 종류

자바스크립트의 코드는 3가지 종류로 이루어지는데, 글로벌 스코프에서 실행하는 글로벌 코드, 함수 스코프에서 실행하는 함수 코드 그리고 여기서 다루진 않지만 `eval()` 로 실행되는 코드가 있다. 이 각각의 코드는 자신만의 실행 컨텍스트를 생성한다.

엔진이 스크립트 파일을 실행하기 전에 **글로벌 실행 컨텍스트(Global Execution Context, GEC)** 가 생성되고, 함수를 호출할 때마다 **함수 실행 컨텍스트(Function Execution Context, FEC)** 가 생성된다. 주의할 점은, 글로벌의 경우 실행 이전에 생성되지만 함수의 경우 호출할 때 생성된다는 점이다.

<br/>

#### 실행 컨텍스트 스택 (Execution Context Stack)

실행 컨텍스트가 생성되면 흔히 콜 스택(Call Stack)이라고도 불리는 실행 컨텍스트 스택에 쌓이게 된다. GEC는 코드를 실행하기 전에 쌓이고 모든 코드를 실행하면 제거된다. FEC는 호출할 때 쌓이고 호출이 끝나면 제거된다. 예시 코드를 통해 살펴보자.

```javascript
function func() {
  console.log('함수 실행 컨텍스트');
}
console.log('글로벌 실행 컨텍스트');
func();
```

제일 처음, 코드를 실행하기 전에 GEC가 쌓이고 코드를 실행하면서 콘솔에 "글로벌 실행 컨텍스트" 가 출력된다. 그 다음 `func()` 을 호출하고 그에 대한 FEC가 만들어져 쌓이고 FEC를 실행하며 콘솔에 "함수 실행 컨텍스트" 가 출력된다. 이후 `func()` 이 종료되고 FEC가 스택에서 제거된 후, 모든 코드의 실행이 끝나면서 GEC가 스택에서 제거된다. [GIF](https://miro.medium.com/max/1100/1*dUl6qPEaDJJTXWythQsEtQ.gif) 를 통해서 더 쉽게 이해할 수 있으니 꼭 보자.

<br/>

#### 구성요소

실행 컨텍스트는 다음과 같은 구성요소를 갖는다.

* Lexical Environment
* Variable Environment
* this 바인딩

##### Lexical Environment

Lexical Environment는 **변수 및 함수 등의 식별자(Identifier) 및 외부 참조에 관한 정보를 가지고 있는 컴포넌트** 이다. 이 컴포넌트는 2개의 구성요소를 갖는다.

* **Environment Record**
* **outer 참조**

Environment Record가 식별자에 관한 정보를 가지고 있으며 outer 참조는 외부 Lexical Environment를 참조하는 포인터이다.

```javascript
var x = 10;
 
function foo() {
  var y = 20;
  console.log(x);
}
```

위와 같은 코드가 있을 때는 아래와 같이 Lexical Environment가 형성된다.

```
globalEnvironment = {
	environmentRecord = { x: 10 },
	outer: null
}
fooEnvironment = {
	environmentRecord = { y: 20 },
	outer: globalEnvironment
}
```

따라서, `foo()` 에서 `x` 를 참조할 때는 현대 Environment Record를 찾아보고 없기 때문에 outer 참조를 사용하여 외부의 Lexical Environment에 속해 있는 Environment Record를 찾아보는 방식이다.

##### Variable Environment

Variable Environment는 Lexical Environment와 동일한 성격을 띠지만 **`var` 로 선언된 변수만 저장한다는 점에서 다르다.** 즉, Lexical Environment는 `var` 로 선언된 변수를 제외하고 나머지(`let` 으로 선언되었거나 함수 선언문)를 저장한다. ( 코드로 확인해 볼려면 [여기](https://stackoverflow.com/a/45788048/11789111) 를 보자 )

##### this 바인딩

this의 바인딩은 실행 컨텍스트가 생성될 때마다 this 객체에 어떻게 바인딩이 되는지를 나타낸 것이다.  (ES6부터 this의 바인딩이 LexicalEnvironment 안에 있는 EnvironmentRecord 안에서 일어난다는 사실을 기억해두도록 하자. 그렇게 중요하진 않으니 알고만 있자.)

* **GEC의 경우**
  * strict mode라면 `undefined` 로 바인딩된다.
  * 아니라면 글로벌 객체로 바인딩된다. (브라우저에선 window, 노드에선 global)
* **FEC의 경우**
  * 해당 함수가 어떻게 호출되었는지에 따라 바인딩된다.

<br/>

#### 과정

EC는 2가지 과정을 거친다.

1. **Creation Phase (생성단계)**
2. **Execution Phase (실행단계)**

##### 생성단계

생성단계는 다시 3가지 단계로 이루어진다.

1. Lexical Environment 생성
2. Variable Environment 생성
3. this 바인딩

여기서 **주의할 점은 값이 변수에 매핑되지 않는다는 것** 이다. `var` 의 경우는 `undefined` 로 초기화되고 `let` 이나 `const` 는 아무 값도 가지지 않는다.

##### 실행단계

이제 코드를 실행하면서 **변수에 값을 매핑시킨다.** 예시를 통해 보자.

```javascript
var a = 3;
let b = 4;

function func(num) {
  var t = 9;
  console.log(a + b + num + t);
}

var r = func(4);
```

* **GEC의 생성 단계**

여기서 생성이 될 때 실행 컨텍스트 스택에 쌓인다.

```
GEC {
	ThisBinding: window,
	LexicalEnvironment: {
		EnvironmentRecord: {
			b: <uninitialized>,
			func: func(){...}
		},
		outer 참조: null
	},
	VariableEnvironment: {	
		EnvironmentRecord: {
			a: undefined,
			r: undefined
		},
		outer 참조: null
	}
}
```

* **GEC의 실행 단계**

```
GEC {
	ThisBinding: window,
	LexicalEnvironment: {
		EnvironmentRecord: {
			b: 4,
			func: func(){...}
		},
		outer 참조: null
	},
	VariableEnvironment: {
		EnvironmentRecord: {
			a: 3,
			r: undefined
		},
		outer 참조: null
	}
}
```

이제 `func()` 을 호출하고 FEC를 생성한다.

* **FEC의 생성 단계**

```
FEC {
	ThisBinding: window,
	LexicalEnvironment: {
		EnvironmentRecord: {
			arguments: { num: 4, length: 1 },
		},
		outer: GEC의 LexicalEnvironment
	},
	VariableEnvironment: {
		EnvironmentRecord: {
			t: undefined
		},
		outer: GEC의 LexicalEnvironment
	}
}
```

* **FEC의 실행 단계**

```
FEC {
	ThisBinding: window,
	LexicalEnvironment: {
		EnvironmentRecord: {
			arguments: { num: 4, length: 1 },
		},
		outer: GEC의 LexicalEnvironment
	},
	VariableEnvironment: {
		EnvironmentRecord: {
			t: 9
		},
		outer: GEC의 LexicalEnvironment
	}
}
```

FEC가 스택에서 제거 되고 GEC의 `r` 이 20으로 초기화된다.

```
GEC {
	ThisBinding: window,
	LexicalEnvironment: {
		EnvironmentRecord: {
			b: 4,
			func: func(){...}
		},
		outer 참조: null
	},
	VariableEnvironment: {
		EnvironmentRecord: {
			a: 3,
			r: 20
		},
		outer 참조: null
	}
}
```

모든 코드를 실행하고 GEC가 스택에서 제거된 뒤 프로그램이 종료된다. [GIF](https://miro.medium.com/max/1100/1*SBP65hdVDW5j0LuVryTiXw.gif) 를 보면 더 확실히 이해할 수 있으니 꼭 보자.

<br/>

#### 참고

* [What is the 'Execution Context' in JavaScript exactly?](https://stackoverflow.com/questions/9384758/what-is-the-execution-context-in-javascript-exactly)
* [[JS] 자바스크립트의 The Execution Context (실행 컨텍스트) 와 Hoisting (호이스팅)](https://velog.io/@imacoolgirlyo/JS-자바스크립트의-Hoisting-The-Execution-Context-호이스팅-실행-컨텍스트-6bjsmmlmgy)
* [자바스크립트 함수 (2) - 함수 호출](https://meetup.toast.com/posts/123)
* [The ECMAScript “Executable Code and Execution Contexts” chapter explained](https://medium.com/@g.smellyshovel/the-ecmascript-executable-code-and-execution-contexts-chapter-explained-fa6e098e230f#f88f)
* [ECMA-262-5 in detail. Chapter 3.2. Lexical environments: ECMAScript implementation.](http://dmitrysoshnikov.com/ecmascript/es5-chapter-3-2-lexical-environments-ecmascript-implementation/#this-binding)
* [ECMAScript 5 spec: LexicalEnvironment versus VariableEnvironment](https://2ality.com/2011/04/ecmascript-5-spec-lexicalenvironment.html)
* [Variable Environment vs lexical environment](https://stackoverflow.com/questions/23948198/variable-environment-vs-lexical-environment)
* [ECMAScript 2020 Language Specification](https://tc39.es/ecma262/)
* [JavaScript Internals: Execution Context](https://medium.com/better-programming/javascript-internals-execution-context-bdeee6986b3b)

***

### 스코프

> [실행 컨텍스트(Execution Context)](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/javascript/execution-context.md) 를 모른다면 보고 오도록 하자.

#### 정의

스코프란 **자바스크립트 엔진이 참조의 대상이 되는 식별자(Identifier)를 검색할 때 사용하는 규칙의 집합** 이다. 즉, 어떤 변수를 사용하거나 함수를 호출하려고 할 때 해당하는 식별자로 사용하는데, 그 식별자를 검색하는 메커니즘이라고 이해하면 된다.

<br/>

#### 렉시컬 스코프

**프로그래머가 코드를 짤 때, 변수 및 함수/블록 스코프를 어디에 작성하였는가에 따라 정해지는 스코프** 를 렉시컬 스코프라고 한다. "렉시컬(Lexical)" 이라는 명칭이 붙은 이유는 자바스크립트 컴파일러가 소스코드를 토큰(Token)으로 쪼개서 의미를 부여하는 렉싱(Lexing) 단계에 해당 스코프가 확정되기 때문이다. 다시 쉽게 말하면, 변수 혹은 함수/블록이 어디에 써있는가를 보고 그 스코프를 판단하면 된다.

<br/>

#### 스코프 체인

**현재 스코프에서 식별자를 검색할 때 상위 스코프를 연쇄적으로 찾아나가는 방식** 을 말한다. 실행 컨텍스트를 배웠다면 생성될 때마다 LexicalEnvironment가 만들어지고 그 안에 outer 참조 값이 있다는 것을 알 것이다. 바로 이 outer 참조 값이 상위 스코프의 LexicalEnvironment를 가리키기 때문에 이를 통해 체인처럼 연결되는 것이다.

즉, 다음과 같은 과정으로 스코프 체인을 검색한다.

1. 현재 실행 컨텍스트의 LexicalEnvironment의 EnvironmentRecord에서 식별자를 검색한다.
2. 없으면 outer 참조 값으로 스코프 체인을 타고 올라가 상위 스코프의 EnvironmentRecord에서 식별자를 검색한다.
3. 이를 outer 참조 값이 `null` 일 때까지 계속하고 찾지 못한다면 에러를 발생시킨다.

<br/>

#### 참조

* [You don't know JS, Scope and Closures](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/ch1.md)
* [PoiemaWeb, 스코프](https://poiemaweb.com/js-scope)


***


### 호이스팅(Hoisting)

> [스코프](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/Javascript/scope.md) 의 개념을 이해하지 못했다면 이해하고 오자.

호이스팅이란 "끌어올린다" 라는 뜻으로 **변수 및 함수 선언문이 스코프 내의 최상단으로 끌어올려지는 현상** 을 말한다. 여기서 주의할 점은 **"선언문"** 이라는 것이며 "대입문"은 끌어올려지지 않는다. 아래 코드를 보자.

```javascript
console.log(a);
var a = 2;
```

컴파일러는 자바스크립트 엔진이 인터프리팅(Interpreting)을 하기 전에 컴파일을 하는데 이 때, `var a = 2;` 를 2개의 구문으로 본다.

* `var a`
* `a = 2`

`var a` 는 변수 선언문으로 컴파일을 할 때 처리하고, `a = 2` 는 실행할 때까지 내버려둔다. 따라서, 변수 a는 호이스팅 되고 콘솔에는 다음과 같은 결과가 나온다.

```javascript
undefined
```

함수 선언문의 경우도 호이스팅 된다.

```javascript
func();
function func() { console.log('함수 호이스팅'); }
```

```
함수 호이스팅
```

<br/>

#### 함수 호이스팅에서 주의할 점

함수 호이스팅에서 주의할 점이 더 있다.

* **함수 표현식(Function Expression)은 호이스팅 되지 않는다.**

```javascript
func();
var func = function() {}
```

여기서는 변수 func의 호이스팅이 발생해서 참조할 수는 있기 때문에 ReferenceError가 발생하지 않지만 그 값이 `undefined` 이기 때문에 TypeError가 발생한다.

* **함수와 변수 선언문 중에는 함수 선언문이 먼저다.**

```javascript
func();
var func = function(){ console.log('변수 호이스팅') }
function func() {
  console.log('함수 호이스팅');
}
```

동일한 이름으로 함수 선언문과 변수 선언문(= 함수표현식)이 있지만 함수 선언문의 호이스팅이 먼저이기 때문에 결과는 다음과 같다.

```
함수 호이스팅
```

<br/>

#### 참고

* [You don't know JS, Scope and Closures : Chapter 5: The (not so) Secret Lifecycle of Variables](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/ch5.md)


***

### 클로저(Closure)

클로저란 **함수가 속한 렉시컬 스코프를 기억하여 함수가 렉시컬 스코프 밖에서 실행될 때도 그 스코프에 접근할 수 있게 하는 기능** 을 말한다.

```javascript
function outer() {
  var a = 2;
  function inner() {
    console.log(a);
  }
  return inner;
}

var func = outer();
func(); // 2
```

여기서 GC(Garbage Collector)가 `outer()` 의 참조를 없앨 것 같지만 내부함수인 `inner()` 가 해당 스코프의 변수인 a를 참조하고 있기 때문에 없애지 않는다. 따라서 스코프 외부에서 `inner()` 가 실행되도 해당 스코프를 기억하기 때문에 2를 출력하게 된다. 즉, 여기서 클로저는 `inner()` 가 되며 `func` 에 담겨 밖에서도 실행되고 렉시컬 스코프를 기억한다.

<br/>

#### 예제

클로저를 사용하는 대표적인 예제는 역시 "반복문 클로저"이다.

```javascript
function func() {
  for (var i=1; i<5; i++) {
    setTimeout(function() { console.log(i); }, i*500);
  }
}
func(); // 5 5 5 5
```

코드의 의도한 바는 1부터 4까지 간격을 두고 출력하는 것이었지만 5가 4번 출력된다. 왜 이렇게 되는 것일까?  

`setTimeout()` 을 반복문 안에서 돌리면 콜백함수가 계속해서 task queue에 쌓이게 되고 반복문이 끝나고 나서 call stack으로 돌아와서 실행된다. 콜백함수는 클로저이기 때문에 상위 스코프에게 `i` 의 값을 물어보고 상위 스코프인 `func` 의 스코프에선 `i` 가 5까지 증가했기 때문에 5가 4번 출력된다.

<br/>

#### 해결방법

위 문제를 해결하기 위해선 2가지 방법이 있다.

* **새로운 함수 스코프로 해결하기**

```javascript
function func() {
  for (var i=1; i<5; i++) {
    (function (j) { 
      setTimeout(function() { console.log(j); }, j*500);
    })(i);
  }
}
func(); // 1 2 3 4
```

`setTimeout()` 을 IIFE(Immediately Invoked Function Expression, 즉시실행함수 표현식)로 감싸게 되면, 새로운 스코프를 형성하고 나중에 콜백함수가 `j` 를 참조할 때 그 시점의 `i` 값을 갖기 때문에 원하는 결과를 얻을 수 있게 된다.

* **블록 스코프로 해결하기**

```javascript
function func() {
  for (let i=1; i<5; i++) {
    setTimeout(function() { console.log(i); }, i*500);
  }
}
func(); // 1 2 3 4
```

함수 스코프가 아닌 블록 스코프를 갖는 `let` 을 사용하면 `for` 문 내의 새로운 스코프를 갖기 때문에 매 반복마다 새로운 `i` 가 선언되고 반복이 끝난 이후의 값으로 초기화가 된다. 따라서, `setTimeout()` 의 클로저인 콜백함수가 `i` 를 참조하기 위해 상위 스코프를 검색할 때 블록 스코프에서 매 반복마다 선언 및 초기화 된 `i` 를 참조하는 것이다.

실제 클로저를 사용하는 예를 알고 싶다면 [Stackoverflow의 답변](https://stackoverflow.com/a/39045098/11789111) 을 참고하자.

<br/>

#### 참고

* [TOAST, 자바스크립트의 스코프와 클로저](https://meetup.toast.com/posts/86)
* 카일 심슨, 『You don't know JS - 타입과 문법, 스코프와 클로저』, 최병현, 한빛미디어(2017)

***

### 네이티브 객체 vs 호스트 객체

#### 네이티브 객체 (Native Object)

ECMAScript 명세에서 의미론적인 부분을 완전히 정의해놓은 객체들로, 다음과 같은 것들이 있다.

* `Object`
* `Function`
* `Date`
* `Math`
* `parseInt`
* `eval` ...

<br/>

#### 호스트 객체 (Host Object)

자바스크립트를 실행하는 환경에 종속된 객체로 그 환경에서만 찾아볼 수 있는 객체이다. 만약 브라우저 환경이라면 다음과 같은 것들이 있다.

* `window`
* `document`
* `location`
* `XMLHttpRequest`
* `querySelectorAll` ...

<br/>

#### 참고

* [Stackoverflow, What is the difference between native objects and host objects?](https://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects)
* [MDN, Standard built-in objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
* [Poiemaweb, 빌트인 객체](https://poiemaweb.com/js-built-in-object)


***


### this의 바인딩

EC(Execution Context)가 생성될 때마다 this의 바인딩이 일어나며 우선순위 순으로 나열해보면 다음과 같다.

1. `new` 를 사용했을 때 해당 객체로 바인딩된다.

```javascript
var name = "global";
function Func() {
  this.name = "Func";
  this.print = function f() { console.log(this.name); };
}
var a = new Func();
a.print(); // Func
```

2. `call`, `apply`, `bind` 와 같은 명시적 바인딩을 사용했을 때 인자로 전달된 객체에 바인딩된다.

```javascript
function func() {
  console.log(this.name);
}

var obj = { name: 'obj name' };
func.call(obj); // obj name
func.apply(obj); // obj name
(func.bind(obj))(); // obj name
```

3. 객체의 메소드로 호출할 경우 해당 객체에 바인딩된다.

```javascript
var obj = {
  name: 'obj name',
  print: function p() { console.log(this.name); }
};
obj.print(); // obj name
```

4. 그 외의 경우

* strict mode: `undefined` 로 초기화된다.
* 일반: 브라우저라면 `window` 객체에 바인딩 된다.

<br/>

#### 참고

* [김정환 블로그, 자바스크립트 this 바인딩 우선순위](http://jeonghwan-kim.github.io/2017/10/22/js-context-binding.html#암시적-바인딩과-new-바인딩의-우선순위)
* [How does the “this” keyword work?](https://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work)


***

### var vs let vs const

모두 변수를 선언하는 키워드라는 것은 동일하다. 하지만, let과 const는 ES2015(ES6)에서 등장했고 여러가지 다른 특성을 갖는다.

#### 스코프 규칙

> 스코프의 개념을 모른다면 [스코프](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/Javascript/scope.md) 를 보고 오자.

* var는 함수 스코프를 갖는다.
* let과 const는 블록 스코프를 갖는다.

```javascript
function run() {
  var foo = "Foo";
  let bar = "Bar";

  console.log(foo, bar);

  {
    let baz = "Bazz";
    console.log(baz);
  }

  console.log(baz); // ReferenceError
}

run();
```

따라서, 위 코드를 실행했을 때 블록 안에 있는 `baz` 를 출력하게 되면 ReferenceError가 발생하는 것이다.

<br/>

#### 호이스팅

> 호이스팅의 개념을 모른다면 [호이스팅](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/Javascript/Hoisting.md) 을 보고 오자.

* var는 **함수 스코프의 최상단으로 호이스팅** 되고 선언과 동시에 `undefined` 로 초기화 된다.
* let과 const는 **블록 스코프의 최상단으로 호이스팅** 되고 선언만 되고 값이 할당되기 전까지 어떤 값으로도 초기화되지 않는다.

```javascript
function run() {
  console.log(foo); // undefined
  var foo = "Foo";
  console.log(foo); // Foo
}

run();
```

따라서, var의 경우 위와 같이 선언 전에 출력하면 `undefined` 가 출력된다.

```javascript
function checkHoisting() {
  console.log(foo); // ReferenceError
  let foo = "Foo";
  console.log(foo); // Foo
}

checkHoisting();
```

반면에, let의 경우는 선언 전에 호이스팅 되긴 하지만 어떤 값도 가지지 않기 때문에 ReferenceError가 발생한다. 이런 현상을 **TDZ(Temporal Dead Zone)** 라고 한다. 즉, 선언은 되었지만 참조는 할 수 없는 사각지대를 갖는 것이다.

<br/>

#### 글로벌 객체로의 바인딩

**strict mode가 아니라는 가정 하에,**

* var는 글로벌 스코프에서 선언되었을 경우 글로벌 객체에 바인딩된다.
* let과 const는 글로벌 스코프에서 선언되었을 경우 글로벌 객체에 바인딩되지 않는다.

```javascript
var foo = "Foo";  // globally scoped
let bar = "Bar"; // globally scoped

console.log(window.foo); // Foo
console.log(window.bar); // undefined
```

브라우저 환경에서 글로벌 객체는 `window` 인데, var의 경우 바인딩이 되었고 let의 경우는 되지 않았다는 걸 볼 수 있다.

<br/>

#### 재선언 (Redeclaration)

* var는 재선언이 가능하다.
* let과 const는 재선언이 불가능하다.

```javascript
var foo = "foo1";
var foo = "foo2"; // 문제없음

let bar = "bar1";
let bar = "bar2"; // SyntaxError: Identifier 'bar' has already been declared
```

<br/>

#### let vs const

* var와 let은 재할당이 가능하다.
* const는 선언과 초기화가 반드시 동시에 일어나야 하며 재할당이 불가능하다. 즉, 상수와 같은 고정값을 선언할 때 사용하는 키워드이다.

<br/>

#### 참고

* [Stackoverflow, What's the difference between using “let” and “var”?](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var)
* [Stackoverflow, Are variables declared with let or const not hoisted in ES6?](https://stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-not-hoisted-in-es6)
* [PoiemaWeb, let, const](https://poiemaweb.com/es6-block-scope)

***

### IIFE (Immediately-Invoked Function Expression)

IIFE는 직역하면 즉시-실행 함수 표현식이다. 즉, 2가지 조건을 지닌다.

* **즉시 실행하여야 한다.**
* **함수 표현식이어야 한다. ( = 함수 선언문이 아니어야 한다. )**

결국, 정의하자면 함수 표현식을 즉시 실행하는 것을 말하며 해당 함수는 **익명함수와 기명함수 모두 가능** 하다. 다음과 같이 만들 수 있다.

```javascript
(function(){ 
  console.log('IIFE'); 
})();
```

실행하면 바로 "IIFE"가 출력된다. 여기서 함수는 익명함수를 사용했고 괄호로 감쌌는데, 이것은 함수 표현식으로 만들겠다는 것으로 괄호로 감싸지 않으면 함수 선언문이 되기 때문이다.

<br/>

#### 2가지 형태와 화살표 함수

IIFE는 2가지 형태로 사용할 수 있다.

```javascript
(function(){ console.log('IIFE'); })();
(function(){ console.log('IIFE'); }());
```

더글라스 크락포드는 아래의 방식이 에러를 덜 발생시키는 표현이라고 선호하는데, 반대하는 개발자들도 있어서 **단순히 스타일의 차이** 로 인식하면 된다.

##### 화살표 함수(Arrow Function)으로 사용

ES2015(ES6)에 등장한 화살표 함수를 사용해서도 IIFE를 만들 수 있다.

```javascript
(() => console.log('IIFE'))();
```

단, 주의할 점은 이 방식의 경우 크락포드가 선호하는 방식을 사용할 수 없다.

```javascript
(() => console.log('IIFE')());
```

위 코드는 동작하지 않는데, 그 이유는 함수를 호출하기 위해선 호출대상이 명세에서 말하는 *MemberExpression*이어야 한다. 그러나 화살표 함수의 경우 *AssignmentExpression* 이기 때문에 불가능한 것이다. 이게 무슨 말인지 모르겠다면 [스택오버플로우의 답변](https://stackoverflow.com/a/34589765/11789111) 을 참고하자.

<br/>

#### 사용 이유

IIFE를 사용하는 이유는, 보통 전역 스코프(Global Scope)를 오염시키지 않기 위해서 사용한다. 즉, 변수를 전역 스코프에 선언하는 것을 피하기 위함이다. 이런 기법이 다음과 같은 상황에 이용된다.

##### 클로저와 private 데이터

IIFE안에서 클로저를 생성하면 private 데이터를 만들 수 있고 외부에서 접근할 수 없다.

```javascript
const getCount = (function(){
  let count = 1;
  return function() {
    ++count;
    return count;
  }
})();

console.log(getCount()); // 2
console.log(getCount()); // 3
```

IIFE안의 익명함수는 클로저가 되고 변수 `count` 는 private 데이터가 되므로 밖에 보여지지 않는다. 흔히 말하는 모듈 패턴이 바로 이 방식에 의존한다.

##### 변수의 별칭(alias)

예를 들어, 동일한 전역 변수를 갖는 2개의 라이브러리를 사용한다고 했을 때 충돌을 해결하기 위해 사용할 수도 있다. jQuery의 경우 `$` 라는 전역 변수를 갖는데 이를 갖는 또 다른 라이브러리가 있다고 하면 다음과 같이 해결할 수 있다.

```javascript
window.$ = funciton(){};
(function($){...})(jQuery);
```

##### 최소화(Minification)를 활용한 최적화(Optimization)

[UglifyJS](https://github.com/mishoo/UglifyJS2) 와 같은 라이브러리는 최소화를 통해서 최적화를 하는데, 여기에는 변수명을 짧게 만드는 것도 포함한다.

```javascript
(function(window, document, undefined) {...})(window, document);
```

위와 같은 IIFE가 있다고 하면, 다음과 같이 줄일 수 있다.

```javascript
(function(w, d, u) {...})(window, document);
```

이를 통해 파일의 크기를 줄일 수 있게 된다.

<br/>

#### 참고

* [Explain the encapsulated anonymous function syntax](https://stackoverflow.com/questions/1634268/explain-the-encapsulated-anonymous-function-syntax)
* [What is the practical use of an IIFE with a name?](https://stackoverflow.com/questions/18365801/what-is-the-practical-use-of-an-iife-with-a-name)
* [Use Cases for JavaScript's IIFEs](https://mariusschulz.com/blog/use-cases-for-javascripts-iifes)
* [What is the (function() { } )() construct in JavaScript?](https://stackoverflow.com/questions/8228281/what-is-the-function-construct-in-javascript)


***


### 모듈 시스템: CommonJS, AMD, UMD, ES6

#### 모듈(module)이란?

모듈이란 **여러 기능들에 관한 코드가 모여있는 하나의 파일** 로 다음과 같은 것들을 위해 사용한다.

* **유지보수성** : 기능들이 모듈화가 잘 되어있다면, 의존성을 그만큼 줄일 수 있기 때문에 어떤 기능을 개선한다거나 수정할 때 훨씬 편하게 할 수 있다.
* **네임스페이스화** : 자바스크립트에서 전역변수는 전역공간을 가지기 때문에 코드의 양이 많아질수록 겹치는 네임스페이스가 많아질 수 있다. 그러나 모듈로 분리하면 모듈만의 네임스페이스를 갖기 때문에 그 문제가 해결된다.
* **재사용성** : 똑같은 코드를 반복하지 않고 모듈로 분리시켜서 필요할 때마다 사용할 수 있다.

이런 장점들을 살리기 위해서 모듈 개념이 필요했고 자바스크립트에선 모듈을 개발하기 위한 여러가지 시도들이 있었다. CommonJS, AMD, UMD 및 ES6 등 각각의 특징과 사용법을 알아보자.

<br/>

#### CommonJS

자바스크립트의 공식 스펙이 브라우저만 지원했기 때문에 이를 서버사이드 및 데스크탑 어플리케이션에서 지원하기 위한 노력이 있었다. 그걸 위해 만든 그룹이 CommonJS이며 여기선 자바스크립트가 범용적인 언어로 쓰이기 위한 스펙을 정의하고 있다. 그룹을 만들었을 때, 범용적인 언어로 만들기 위해서는 모듈화의 개념이 필요했고 이 그룹만의 모듈 방식을 정의하게 되었는데 그것이 바로 CommonJS 방식의 모듈화다.

다른 모듈을 사용할 때는 **require** 를, 모듈을 해당 스코프 밖으로 보낼 때에는 **module.exports** 를 사용하는 방식으로, Node.js에선 현재 이 방식을 사용하고 있다. hello world를 출력하는 함수를 가진 파일을 `a.js` 라고 하고 그 함수를 가져와서 사용하는 파일을 `b.js` 라고 하면 다음과 같이 사용할 수 있다.

* `a.js`

```javascript
const printHelloWorld = () => {
  console.log('Hello Wolrd');
};

module.exports = {
  printHelloWorld
};
```

* `b.js`

```javascript
const func = require('./a.js'); // 같은 디렉토리에 있다고 가정
func.printHelloWorld();
```

여기서 `module.exports` 의 `module` 은 현재 모듈에 대한 정보를 갖고 있는 객체이다. 이는 예약어이며 그 안에 `id` , `path` , `parent` 등의 속성이 있고 `exports` 객체를 가지고 있다.

##### exports vs module.exports

`module.exports` 외에도 `exports` 를 사용하기도 하는데 이 관계에 대해서 명확히 이해하고 있어야 한다. 정리하자면 아래와 같다.

* `module.exports` 는 빈 객체를 참조한다.
* `exports` 는 `module.exports` 를 참조한다.
* `require` 는 항상 `module.exports` 를 리턴받는다.

즉, 함수를 모듈 밖으로 내보내고자 할 때는 위에 예시에서 2가지 모두 사용할 수 있다.

```javascript
exports.printHelloWorld = printHelloWorld;
module.exports = { printHelloWorld };
```

그렇다면 왜 2가지를 설정해놓았을까? 그 이유는 `exports` 는 항상 `module.exports` 를 참조하기 때문에 `exports` 를 사용하면 직접 `module.exports` 를 수정하지 않고 객체의 멤버를 만들거나 수정하는 방식으로 사용한다. 따라서, `exports` 에 어떤 값을 할당하거나 새로운 객체를 할당했다고 하더라도 결국 `require` 는 `module.exports` 를 리턴받기 때문에 잠재적인 버그를 피할 수가 있다.

<br/>

#### AMD(Asynchronous Module Definition)

CommonJS 그룹에서 의견이 맞지 않아 나온 사람들이 만든 그룹으로 비동기 모듈에 대한 표준안을 다루는 그룹이다. CommonJS가 서버쪽에서 장점이 많은 반면에 AMD는 브라우저 쪽에서 더 큰 효과를 발휘한다. 브라우저에서는 모든 모듈이 다 로딩될 때까지 기다릴 수 없기 때문에 비동기 모듈 로딩방식으로 구현을 해놓았다. 이 방식에서 사용하는 함수는 `define()` 과 `require()` 이며 AMD 스펙을 가장 잘 구현한 모듈로더는 RequireJS 이다. 한번 간단한 예시로 사용해보자.

* `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Document</title>
</head>
<body>
  <script data-main="index.js" src="require.js"></script>
</body>
</html>
```

`require.js` 파일을 받아서 `<script>` 태그에 넣어주고 `data-main` 속성으로 `require.js` 가 로드된 후에 실행할 자바스크립트 파일 경로를 넣어준다. 즉, `require.js` 가 로드되자마자 `index.js` 가 실행되는 구조이다.

* `index.js`

```javascript
require.config({
  baseUrl: '/',
  paths: {
    a: 'a',
    b: 'b',
  }
});

require(['a'], (a) => {
  a.printA();
});
```

`require.config` 는 설정부분으로 기본 경로와 각 모듈에 해당하는 경로를 설정해준다. 그 다음 `require` 를 통해서 첫번째 인자에 해당하는 모듈이 로드되었을 경우에 그걸 `a` 로 받아서 `printA()` 함수를 호출하는 콜백함수를 실행한다. 의존성 모듈을 지정해주는 것이다.

* `a.js`

```javascript
define(() => {
  return {
    printA: () => console.log('a')
  }
});
```

모듈 a는 위와 같이 만들어져 있고 `define()` 을 통해서 정의된다. 여기서도 `require()` 에서 의존성 모듈을 설정해준 것처럼 콜백함수가 실행되기 전에 로드되어야 할 모듈들을 지정해줄 수 있다.

<br/>

#### UMD(Universal Module Definition)

위에서 살펴본 바로, 모듈 구현방식이 CommonJS 와 AMD로 나뉘기 때문에 그걸 통합하기 위한 하나의 패턴이라고 할 수 있다. [공식 umd 소스코드](https://github.com/umdjs/umd/blob/master/templates/returnExports.js) 를 살펴보면 아래와 같다.

```javascript
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define([], factory);
    } else if (typeof module === 'object' && module.exports) {
        // Node. Does not work with strict CommonJS, but
        // only CommonJS-like environments that support module.exports,
        // like Node.
        module.exports = factory();
    } else {
        // Browser globals (root is window)
        root.returnExports = factory();
  }
}(typeof self !== 'undefined' ? self : this, function () {

    // Just return a value to define the module export.
    // This example returns an object, but the module
    // can return a function as the exported value.
    return {};
}));
```

AMD, CommonJS, Browser 방식의 모듈을 지원하기 위한 것으로 확인하는 방식은 코드를 살펴보면 다음과 같다.

* **AMD**: `define()` 이 함수이고 `define.amd` 속성의 객체를 가지고 있다.
* **CommonJS**: `module` 이 객체이고 `module.exports` 속성의 객체를 가지고 있다.
* **Browser**: 따로 특이사항이 없다.

통합하는 방식은 2개의 인자를 전달받는 함수를 실행하는 것으로, 첫번째 인자는 Browser 쪽을 구현할 `root` 에 넘길 값으로 `undefined` 이면 `this` 로 아니라면 `self` , 즉 `window` 로 설정한다. 그리고 2번째 인자로 빈 객체 리터럴을 리턴하는 함수를 보낸다. 이렇게 되면 각각의 환경에서 모두 모듈개념을 사용할 수 있게 된다.

<br/>

#### ES6(ES2015) 방식

`import` 와 `export` 구문을 사용하는 방식으로 나에게는 이 방식이 가장 익숙하다. 하지만 모든 브라우저가 지원하는 것이 아니기 때문에 Babel의 `@babel/plugin-transform-modules-commonjs` 를 통해 변환시켜서 사용한다. 모듈 A,B가 있고 각각을 `export` 로 내보내는 방식과 그에 따라 어떻게 `import` 로 불러오는지 살펴보자.

* `moduleA.js`

```javascript
const A = () => {};
export default A;
```

* `moduleB.js`

```javascript
export const B = () => {};
```

* `index.js`

```javascript
import A from 'moduleA';
import { B } from 'moduleB';
```

여기서 눈여겨봐야될 것은 `default` 의 유무인데 `export` 를 사용할 때는 **named export** 와 **default export** 를 사용할 수 있다. 단, default export는 모듈 내에서 한번만 사용할 수 있고 named export는 여러번 사용할 수 있다는 것이다. 그렇게 default export로 내보내면 `import` 에선 내보낸 이름 그대로 바로 사용할 수 있지만, named export로 내보내면 `{}` 로 묶어서 불러와야 한다. 이것이 기본적인 사용법이고 별칭(alias)을 `as` 로 주어서 다른 이름으로 사용할 수도 있고 `*` 와일드카드를 사용하여 한번에 불러오거나 내보낼 수도 있다. 이런 여러가지 변형기법의 사용은 [여기](https://velog.io/@doondoony/JavaScript-Module-System#-es6-modulesesm) 를 참고하자.

<br/>

#### 참조

* [JavaScript Modules: A Beginner’s Guide](https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/)
* [AMD, CommonJS, UMD 모듈](https://www.zerocho.com/category/JavaScript/post/5b67e7847bbbd3001b43fd73)
* [[NodeJS] module.exports와 exports의 차이점](https://programmingsummaries.tistory.com/340)
* [module.exports vs exports in Node.js](https://stackoverflow.com/questions/7137397/module-exports-vs-exports-in-node-js)
* [JavaScript 표준을 위한 움직임: CommonJS와 AMD](https://d2.naver.com/helloworld/12864)
* [RequireJS - AMD의 이해와 개발](https://d2.naver.com/helloworld/591319)
* [requireJS를 통한 모듈화 및 의존성 관리 - (1)](https://wckhg89.github.io/archivers/requirejs1#requirejs를-이용한-모듈화-및-의존성-관리)


***


### 콜 스택(Call stack)과 힙(Heap)

자바스크립트 엔진이 자바스크립트를 실행할 때 원시 타입 및 참조 타입을 저장하는 메모리 구조로 콜 스택과 힙을 가진다.

* **콜 스택** : **원시타입 값** 과 함수 호출의 **실행 컨텍스트(Execution Context)** 를 저장하는 곳이다.
* **힙** : 객체, 배열, 함수와 같이 크기가 **동적으로 변할 수 있는 참조타입 값** 을 저장하는 곳이다.

<br/>

#### 예시를 통한 동작 원리 보기

```javascript
let a = 10;
let b = 35;
let arr = [];
function func() {
  const c = a + b;
  const obj = { d: c };
  return obj;
}
let o = func();
```

위 코드로 콜 스택과 힙의 동작을 보면 다음과 같다.

제일 처음, GEC(Global Execution Context)가 생성되고 원시 값은 콜 스택에, 참조 값은 힙에 저장된다.

<img src="../../images/javascript/memory1.png"/>

그 다음 함수 `func()` 을 실행하게 되고 새로운 FEC(Function Execution Context)가 생성되며 동일하게 원시 값은 스택에, 참조 값은 힙에 저장된다.

<img src="../../images/javascript/memory2.png"/>

이후, 함수 `func()` 이 객체 `obj` 를 리턴해서 `o` 에 저장된다. 리턴하기 때문에 FEC는 콜 스택에서 제거된다.

<img src="../../images/javascript/memory3.png"/>

전체 코드가 실행이 끝나고 GEC가 콜 스택에서 제거된다. GEC가 제거됨에 따라서, 힙의 객체를 참조하는 스택의 값이 없기 때문에 가비지 컬렉터(Garbage Collector, GC)에 의해 제거된다.

<br/>

#### 참고

* [JavaScript V8 Engine Explained](https://hackernoon.com/javascript-v8-engine-explained-3f940148d4ef)
* [<번역>자바스크립트의 메모리 모델](https://junwoo45.github.io/2019-11-04-memory_model/)
* [V8 Memory usage(Stack & Heap)](https://speakerdeck.com/deepu105/v8-memory-usage-stack-and-heap?slide=9)
* [V8 엔진(자바스크립트, NodeJS, Deno, WebAssembly) 내부의 메모리 관리 시각화하기](https://ui.toast.com/weekly-pick/ko_20200228/)


***


### 이벤트 루프 (Event loop)

> [콜 스택(Call stack)과 힙(Heap)](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/javascript/stack-heap.md) 을 보고 오자.

자바스크립트는 **단일 스레드(Single-threaded) 기반 언어** 로, 자바스크립트 엔진이 단일 콜 스택을 갖는다. 이 말은 요청이 동기적으로 처리된다는 것을 의미한다. 그렇다면 비동기 요청은 어떻게 처리될 수 있을까? 그것은 바로 자바스크립트를 실행하는 환경인 브라우저나 Node.js가 담당한다. **여기서 자바스크립트 엔진과 그 실행 환경을 상호 연동시켜주는 장치가 바로 이벤트 루프이다.** 따라서, 이벤트 루프는 자바스크립트 엔진에 있지 않고 그 환경에 속한다.

<br/>

#### 태스크 큐(Task queue)와 마이크로태스크 큐(Microtask queue)

자바스크립트의 실행 환경은 2가지 큐를 가지고 있으며 **각각 스크립트 실행, 이벤트 핸들러, 콜백함수 등의 태스크(Task) 담기는 공간이다.** 태스크가 콜백함수라면 그 종류에 따라 다른 큐에 담기며 대표적인 예로는 다음과 같은 것들이 있다.

* **태스크 큐**
  * `setTimeout()` , `setInterval()` , UI 렌더링, `requestAnimationFrame()`
* **마이크로태스크 큐**
  * Promise, MutationObserver

이벤트 루프는 2개의 큐를 감시하고 있다가 콜 스택이 비게 되면, 콜백함수를 꺼내와서 실행한다. 이 때 **마이크로태스크 큐의 콜백함수가 우선순위를 가지기 때문에** 마이크로태스크 큐의 콜백함수를 전부 실행하고 나서 태스크 큐의 콜백함수들을 실행한다. (단, UI 렌더링이 태스크 큐에 속하기 때문에 마이크로태스크 큐의 태스크가 많으면 렌더링이 지연될 수 있다.)

<br/>

#### 예시를 통한 동작방식의 이해

```javascript
console.log('콜 스택!');
setTimeout(() => console.log('태스크 큐!'), 0);
Promise.resolve().then(() => console.log('마이크로태스크 큐!'));
```

결과는 다음과 같다.

```
콜 스택!
마이크로태스크 큐!
태스크 큐!
```

처음 스크립트가 로드될 때 **"스크립트 실행"** 이라는 태스크가 먼저 태스크 큐에 들어간다. 그리고 나서 이벤트 루프가 태스크 큐에서 해당 태스크를 가져와 콜 스택을 실행하는 것이다. 즉, 콜 스택에는 이미 GEC(Global Execution Context)가 생성되어 있는 상태에서 "스크립트 실행" 이라는 태스크를 실행하게 되면 그제서야 GEC에 속한 코드가 실행되는 방식이다.

그럼 하나하나 어떻게 동작하는지 그림으로 살펴보자.

<img src="../../images/javascript/task0.png" width="600px"/>

제일 먼저, "스크립트 실행" 태스크가 태스크 큐에 들어가게 된다.

<img src="../../images/javascript/task1.png" width="600px"/>

이후, 이벤트 루프가 그 태스크를 가져와서 로드된 스크립트를 실행시킨다. 따라서 맨 처음에 `console.log` 가 실행된다.

<img src="../../images/javascript/task2.png" width="600px"/>

그 다음, `setTimeout()` 이 콜 스택으로 가고 브라우저가 이를 받아서 타이머를 동작시킨다.

<img src="../../images/javascript/task3.png" width="600px"/>

타이머가 끝나면 `setTimeout()` 의 콜백함수를 태스크 큐에 넣는다.

<img src="../../images/javascript/task4.png" width="600px"/>

`Promise` 가 콜 스택으로 가고 콜백함수를 마이크로태스크 큐에 넣는다.

<img src="../../images/javascript/task5.png" width="600px"/>

이벤트 루프는 마이크로태스크 큐에서 제일 오래된 태스크인 `Promise` 의 콜백함수를 가져와 콜 스택에 넣는다.

<img src="../../images/javascript/task6.png" width="600px"/>

`Promise` 의 콜백함수가 끝나고 태스크 큐에서 제일 오래된 태스크인 `setTimeout()` 의 콜백함수를 가져와 콜 스택에 넣는다.

<img src="../../images/javascript/task7.png" width="600px"/>

`setTimeout()` 의 콜백함수가 끝나면 콜 스택이 비게 되고 프로그램이 종료된다.

<br/>

#### 참고

* [자바스크립트와 이벤트 루프](https://meetup.toast.com/posts/89)
* [what the heck is the event loop anyway](https://www.youtube.com/watch?v=8aGhZQkoFbQ) → 굉장히 좋은 영상, 꼭 1번은 보자.
* [Event loop: microtasks and macrotasks](https://javascript.info/event-loop)


***


### 프로토타입 (Prototype)

자바스크립트에서 정말 헷갈리는 개념으로, 자바스크립트를 다루는데 있어서 굉장히 중요한 역할을 하기 때문에 반드시 이해하여야 한다.

#### 정의

자바스크립트의 모든 객체는 자신의 **"원형(Prototype)"** 이 되는 객체를 가지며 이를 프로토타입이라고 한다. 보이지 않는 속성인 `[[Prototype]]` 이 자신의 프로토타입 객체를 참조한다. 이를 `__proto__` 라는 속성으로 참조할 수 있으나 이는 비표준이고 모든 브라우저에서 동작하는 것은 아니기 때문에 실제로 사용하는 것은 피해야 한다.

<br/>

#### .prototype과 [[Prototype]]

프로토타입이 헷갈리는 이유는 그 명명법과 연결방식에 있는데, 모든 객체는 은닉 속성인 `[[Prototype]]` 을 갖는데 특별히 **함수 객체** 는 접근할 수 있는 속성인 `prototype` 을 갖는다. 이름만 보면 같은 것으로 보이기 때문에 관계를 명확히 파악하여야 한다.

* `[[Prototype]]` : 자신의 프로토타입 객체를 참조하는 속성이다.
* `.prototype` : `new` 연산자로 자신을 생성자 함수로 사용한 경우, 그걸로 만들어진 새로운 객체의 `[[Prototype]]` 이 참조하는 값이다.

다음 코드를 보자.

```javascript
function Func() {}
var a = new Func(); 
```

`Func` 을 생성자로 호출하여 새로운 객체 `a` 를 생성한다. 이는 아래와 같은 구조로 프로토타입이 연결된다.

<img src="../../images/javascript/prototype1.png"/>

그림을 보면 위에서 언급하지 않은 2가지가 있는데. `constructor` 는 모든 `.prototype` 객체의 속성에 있는 것으로 실제 객체를 참조한다. 따라서 위와 같이 `Func` 을 가리키는 것이다. 그 다음, `Func.prototype` 의 `[[Prototype]]` 이 `Object.prototype` 으로 연결되는데 이는 모든 객체의 프로토타입 객체로 마지막으로 연결되는 프로토타입 객체이다. 즉, 정리하자면 다음과 같다.

* `new` 연산자로 새로운 객체 `a` 를 생성하면, `a` 의 프로토타입 객체는 생성자 함수로 사용한 `Func` 의 속성인 `Func.prototype` 이 된다.
* `Func.prototype` 은 `constructor` 속성을 가지며 이는 실제 객체 `Func` 을 가리킨다.
* `Func.prototype` 또한 객체이므로 `[[Prototype]]` 을 가지고 이는 모든 객체의 원형이 되는 객체인 `Object.prototype` 을 가리킨다.

<br/>

#### 프로토타입 체인

어떤 객체의 프로퍼티를 참조하거나 값을 할당할 때 **해당 객체에 프로퍼티가 없을 경우, 그 객체의 프로토타입 객체를 연쇄적으로 보면서 프로퍼티를 찾는 방식** 을 프로토타입 체인이라고 한다. 단, 참조할 때와 값을 할당할 때의 메커니즘이 다르니 기억해두어야 한다.

* **프로퍼티를 참조할 때**
  * 찾고자 하는 프로퍼티가 객체에 존재하면 사용한다.
  * 없으면 `[[Prototype]]` 링크를 타고 끝까지 올라가면서 해당 프로퍼티를 찾는다.
  * 찾으면 그 값을 사용하고 없으면 `undefined` 를 반환한다.
* **프로퍼티에 값을 할당할 때**
  * 찾고자 하는 프로퍼티가 객체에 존재하면 값을 바꾼다.
  * 프로퍼티가 없고 `[[Prototype]]` 링크를 타고 올라가서 해당 프로퍼티를 찾았을 경우
    * 그 프로퍼티가 변경가능한 값, 즉 `writable: true` 라면 새로운 직속 프로퍼티를 할당해서 상위 프로퍼티가 가려지는 현상이 발생한다.
    * 그 프로퍼티가 변경불가능한 값, 즉 `writable: false` 라면 비엄격 모드에선 무시되고 엄격 모드에선 에러가 발생한다.
    * 해당 프로퍼티가 세터(setter) 일 경우, 이 세터가 호출되고 가려짐이 발생하지 않는다.

여기서 말하는 **"가려짐"** 이란, 상위 프로토타입 객체에 동일한 이름의 프로퍼티가 있는 경우, 하위 객체의 프로퍼티에 의해 가려지는 현상을 말한다.

```javascript
function Func() {}
Func.prototype.num = 2;
var a = new Func();
a.num = 1;
console.log(a.num); // 1
```

위 코드의 경우 객체 `a` 의 프로토타입 객체인 `Func.prototype` 에 `num` 이 있지만 `a.num = 1` 로 인해 가려짐 현상이 발생해서 1을 출력한다.

```javascript
function Func() {}
Object.defineProperty(Func.prototype, "num", {
  value: 2,
  writable: false
})
var a = new Func();
a.num = 1; // 무시됨
console.log(a.num); // 2
```

그러나 `defineProperty()` 를 사용해서 변경불가능한 프로퍼티로 만들면, 비엄격모드에서 무시되서 2를 출력한다.

<br/>

#### 관련 함수 및 연산자

##### Object.create()

**프로토타입 객체를 받아 연결시켜서 새 객체를 만드는 함수** 로, 프로토타입 객체를 바꾸기 때문에 원래의 `constructor` 를 잃어버린다.

```javascript
function Func1() {}
function Func2() {}

Func2.prototype = Object.create(Func1.prototype);
console.log(Func2.prototype.constructor); // Func1
```

기대하는 결과는 `Func2` 이겠지만 프로토타입 객체가 `Func1.prototype` 으로 바뀌었고 이에 따라 `constructor` 값을 잃어버렸다. 따라서, 프토토타입 체인을 통해서 `Func1.prototype` 의 `constructor` 인 `Func1` 을 출력하게 된다.

### instanceof vs isPrototypeOf

instanceof는 2개의 피연산자를 받는 연산자이고 isPrototypeOf는 `Object.prototype` 에 있는 함수이다. **둘 다 특정 객체의 프로토타입 체인에 찾고자 하는 객체가 있는지 검사할 때 사용** 된다.

```javascript
function A() {}
function B() {}
B.prototype = new A();
B.prototype.constructor = B;
function C() {}
C.prototype = new B();
C.prototype.constructor = C;
var c = new C();
console.log(c instanceof A, A.prototype.isPrototypeOf(c)); // true
console.log(c instanceof B, B.prototype.isPrototypeOf(c)); // true
console.log(c instanceof C, C.prototype.isPrototypeOf(c)); // true
```

##### getPrototypeOf, setPrototypeOf

각각 특정 객체의 프로토타입 객체를 가져오고 할당하는 함수이다.

```javascript
function A() {}
function B() {}
var a = new A();
console.log(Object.getPrototypeOf(a)); // A.prototype
Object.setPrototypeOf(a, B.prototype);
console.log(Object.getPrototypeOf(a)); // B.prototype
```

<br/>

#### 참고

* You don't know JS, 프로토타입 (책)
* [Stackoverflow, What's the difference between isPrototypeOf and instanceof in Javascript?](https://stackoverflow.com/questions/2464426/whats-the-difference-between-isprototypeof-and-instanceof-in-javascript)
* [Poiemaweb, 프로토타입](https://poiemaweb.com/js-prototype)
* MDN 문서의 프로토타입 관련 함수들


***


### == vs ===

둘 다 동일한 비교를 하지만 엄격한 동등 비교 연산자(===)의 경우, **타입변환(Type conversion)이 일어나지 않으며 타입이 일치해야한다.**

```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```

위 경우를 보면, 동등 비교 연산자(==) 를 사용해서 여러가지 원시타입을 비교해보았는데, 타입변환이 발생함을 볼 수 있다.

단, 객체/배열의 경우는 참조타입이기 때문에 두 연산자 모두 동일하게 동작한다.

```javascript
var a = {}
var b = {}

a == b // false
a === b // false

var c = [];
var d = [];

c == d // false
c === d // false
```

문자열의 경우는 좀 특별한데, 자바스크립트에서 문자열은 원시타입이지만 객체로도 만들 수 있기 때문에 그 동등 비교가 다르다.

```javascript
var a = "string"
var b = new String("string")
a == b // true
a === b // false
```

퍼포먼스 측면에서는 아주 미묘한 차이가 있기 때문에 신경쓸 바가 못되고, **안전한 타입 체크와 더 좋은 코드를 위해 엄격한 동등 비교 연산자(===)를 사용하는 것이 바람직하다.** 물론, 각 타입에 따라 변환이 어떻게 일어나는지에 대한 원리는 따로 공부해서 이해하도록 하자.

<br/>

#### 참고

* [Stackoverflow, Which equals operator (== vs ===) should be used in JavaScript comparisons?](https://stackoverflow.com/questions/359494/which-equals-operator-vs-should-be-used-in-javascript-comparisons)
* [JS, 강제변환](https://baeharam.netlify.com/posts/javascript/JS-강제변환)


***


### 엄격 모드 (Strict mode)

ECMAScript5 부터 도입된 기능으로 **기존에 무시되던 에러들로 하여금 에러를 발생시키게 한다.** 파일 전체에 적용시킬 수도 있고 함수 스코프에 적용시킬 수 있지만 블록 스코프는 불가능하다.

```javascript
"use strict"; // 파일 전체에 적용

function f() {
  "use strict"; // 함수 스코프에 적용
}
```

이를 통해서 실수를 잡아낼 수 있고 안전한지 않은 것들을 예방할 수 있다. 다음 특징들을 갖는다.

* `var` 가 생략된 변수를 전역객체에 바인딩 하지 않는다.
* `NaN = 5` 같은 할당 구문은 불가능하다.
* 제거할 수 없는 프로퍼티를 제거할 수 없다.  (`delete Object.prototype`)
* 함수의 매개변수 이름은 중복될 수 없다.  (`function sum(x,x){}`)
* `with` 키워드 사용할 수 없다.
* 일반 변수를 삭제할 수 없다.  (`delete x`)
* `arguments.callee` 를 사용할 수 없다.
* `arguments` 객체는 항상 원본 인자를 저장한다, 즉 매개변수를 바꿔도 `arguments` 의 값은 바뀌지 않는다.
* 8진수를 사용할 수 없다. (`var a = 013`)
* `eval` 은 새로운 변수를 스코프에 추가하지 않는다.

JSLint나 ESLint와 같은 린터(Linter)를 사용할 수 있으면 사용하되 사용할 수 없으면 `"use strict"` 를 사용하는 것이 좋다.

<br/>

#### 참고

* [MDN, Strict mode](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode)
* [Stackoverflow, What does “use strict” do in JavaScript, and what is the reasoning behind it?](https://stackoverflow.com/questions/1335851/what-does-use-strict-do-in-javascript-and-what-is-the-reasoning-behind-it)


***


### new의 동작방식

자바스크립트에선 `new` 연산자를 통해 함수를 생성자로 호출할 수 있고 그에따라 새로운 객체를 생성할 수 있다. 다음과 같은 과정으로 이루어진다.

* 빈 객체를 생성한다.
* `[[Prototype]]` 속성을 생성자 호출할 함수의 `prototype` 속성으로 지정한다.
  * 만약 함수의 `prototype` 속성이 원시값이라면 `Object.prototype` 으로 지정된다.
* 객체를 생성하고 이 객체를 `this` 로 지정한다.
* 함수를 호출하고 해당 함수의 `this` 로 위에서 지정한 객체를 사용한다.
* 함수의 리턴값이 원시값이라면 새로 만들어진 객체가 리턴되고 리턴값이 객체라면 해당 객체가 리턴된다.

이를 코드로 보면 다음과 같다.

```javascript
function Func() {}
const f = new Func();
```

* 빈 객체 생성 `{}`
* 해당 객체의 `[[Prototype]]` 을 `Func.prototype` 으로 지정
* 이 객체를 `this` 로 지정
* `Func()` 을 호출하고 이 함수에서 `this` 를 위 객체로 지정
* 함수의 리턴값이 `undefined` 원시값이므로 생성한 객체를 리턴

<br/>

#### 참고

* [Stackoverflow, What is the 'new' keyword in JavaScript?](https://stackoverflow.com/questions/1646698/what-is-the-new-keyword-in-javascript)
* [Stackoverflow, How does the new operator work in JavaScript?](https://stackoverflow.com/questions/6750880/how-does-the-new-operator-work-in-javascript)


***

### ES6 (ES2015) 의 특징들

모든 특징들을 나열하진 않고 중요하다고 생각하는 특징들만 나열한다.

<br/>

##### 화살표 함수(Arrow Function)

`=>` 로 사용할 수 있으며 함수와 달리 `this` 가 함수 스코프에 바인딩 되지 않고 렉시컬 스코프를 가진다. 즉, 자신을 감싸는 코드와 동일한 `this` 를 공유한다. 또한 표현식과 문에서도 사용할 수 있다.

```javascript
// 표현식(expression)
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// 문(Statement)
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// 렉시컬 this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

##### 클래스(Classes)

프로토타입 기반의 객체지향 패턴을 쉽게 만든 장치로, 상속과 생성자 및 인스턴스와 정적 메서드 등을 지원한다.

```javascript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

##### 향상된 객체 리터럴

객체 리터럴로 객체를 만들 때 프로퍼티 지정을 좀 더 유연하게 할 수 있도록 기능이 확장되었다.

```javascript
var obj = {
    // 프로토타입 객체 지정
    __proto__: theProtoObj,
    // 프로퍼티이름과 값이 동일할 경우 줄일 수 있음
    handler,
    // 메서드
    toString() {
     // super 호출
     return "d " + super.toString();
    },
    // 계산된 프로퍼티
    [ 'prop_' + (() => 42)() ]: 42
};
```

##### 템플릿 문자열(Template String)

복잡한 문자열을 쉽게 만들어주는 장치로 문자열 안에 문자열 및 변수를 넣을 수 있고 여러 줄의 문자열이 가능하다.

```javascript
// 문자열 안에 문자열 사용하기
`In JavaScript '\n' is a line-feed.`

// 여러 줄 문자열
`In JavaScript this is
 not legal.`

// 문자열 보간(interpolation)
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

##### 비구조화(Destructuring)

배열과 객체의 패턴 매칭을 통해서 바인딩을 하는 기법이다.

```javascript
// 배열 매칭
var [a, , b] = [1,2,3];

// 객체 매칭
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// 객체 매칭을 단축해서 사용
// op, lhs, rhs가 스코프 내에서 바인딩됨
var {op, lhs, rhs} = getASTNode()

// 매개변수 위치에도 사용 가능
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft 비구조화 (Fail-soft는 고장이 나도 작동하도록 짠 프로그램을 말한다)
var [a] = [];
a === undefined;

// 기본 값이 있는 Fail-soft 비구조화
var [a = 1] = [];
a === 1;
```

##### 기본 값 + Rest + Spread

기본 값은 주어지는 값이 없을 때 초기화시키는 값이고 rest 문법은 명시한 변수 외에 나머지를 배열로 가져오는 것이다. Spread 문법은 배열을 반대로 펼치는 역할이다.

```javascript
function f(x, y=12) {
  // y가 없거나 undefined이면 12이다.
  return x + y;
}
f(3) == 15
```

```javascript
function f(x, ...y) {
  // y는 배열이다.
  return x * y.length;
}
f(3, "hello", true) == 6
```

```javascript
function f(x, y, z) {
  return x + y + z;
}
// 각 배열의 원소를 인자로 넘긴다.
f(...[1,2,3]) == 6
```

##### Let + Const

블록 스코프를 갖고 재선언이 불가능하며 선언 이전에 사용할 수 없다.

```javascript
function f() {
  {
    let x;
    {
      // 블록 스코프를 갖기 때문에 허용!
      const x = "sneaky";
      // const는 재할당 불가, 에러!
      x = "foo";
    }
    // 해당 블록에 이미 선언됨, 재선언 불가이므로 에러!
    let x = "inner";
  }
}
```

##### 반복자(Iterator) + For...Of

반복자는 자신만의 반복을 정의하는 규약이고 이는 `for...of` 를 통해 순회할 수 있다. `[Symbol.iterator]` 라는 이름의 메서드를 정의해야하며 그 메서드는 반드시 `next()` 메서드를 가진 객체를 반환해야 한다.

```javascript
let fibonacci = {
  [Symbol.iterator]() {
    let pre = 0, cur = 1;
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        return { done: false, value: cur }
      }
    }
  }
}

for (var n of fibonacci) {
  if (n > 1000)
    break;
  console.log(n);
}
```

##### 제너레이터(Generator)

반복자를 쉽게 생성해주는 것으로 `function*` 과 `yield` 를 사용한다. 반복자의 하위 타입으로, `next` 와 `throw` 를 포함한다. 또한 ES7의 `await` 과 같이 사용할 수 있다.

```javascript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  if (n > 1000)
    break;
  console.log(n);
}
```

##### 모듈(Module)

* **import 로 모듈 불러오기**

```javascript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

```javascript
// app.js
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```

```javascript
// otherApp.js
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

* **export 로 모듈 내보내기**

```javascript
// lib/mathplusplus.js
export * from "lib/math";
export var e = 2.71828182846;
export default function(x) {
    return Math.log(x);
}
```

##### Map + Set + WeakMap + WeakSet

자주 쓰이는 자료구조로, Weak이 붙은 것은 가비지 컬렉션을 허용하며 `size` 프로퍼티를 가지지 않는다.

```javascript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
// WeakSet에 들어간 객체가 어떠한 참조도 가지지 않으므로 가비지 컬렉션이 된다.
```

##### 심볼(Symbol)

새로 추가된 **원시타입** 으로 유일한 값을 가지며 객체의 접근제어를 가능하게 한다. `description` 매개변수를 이용해 디버깅이 가능하며 `Object.getOwnPropertySymbols` 를 통해 객체의 심볼 프로퍼티들을 볼 수 있다.

```javascript
var MyClass = (function() {

  // IIFE 안의 심볼, 모듈화 된 것.
  // 즉, 심볼로 private 데이터 만듦
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

  return MyClass;
})();

var c = new MyClass("hello");
// "key"와 Symbol("key")는 다르다.
c["key"] === undefined
```

### Number + String + Array + Object 에 추가된 것들

```javascript
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // 유사배열객체를 배열로 변환
Array.of(1, 2, 3) // [1,2,3]
[0, 0, 0].fill(7, 1) // [0,7,7]
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
[1, 2, 3, 4, 5].copyWithin(3, 0) // [1, 2, 3, 1, 2]
["a", "b", "c"].entries() // 반복자 [0, "a"], [1,"b"], [2,"c"]
["a", "b", "c"].keys() // 반복자 0, 1, 2
["a", "b", "c"].values() // 반복자 "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) }) // 얕은 복사
```

##### 프라미스(Promise)

비동기 작업이 맞이할 미래의 완료/실패와 결과 값을 나타내는 객체이다.

```javascript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

<br/>

#### 참고

* [lukehoban, es6features github repo](https://github.com/lukehoban/es6features)
* 각 특징들의 MDN 관련 문서들


***

### ES9 (ES2018) ~ ES10 (ES2019) 의 특징들

#### ES9 (ES2018)

##### async 반복자

기존 반복자(Iterator)의 경우 `next()` 메서드는 `value` 와 `done` 을 가진 객체를 리턴했지만 각각이 Promise로 감싸진 Promise 객체를 리턴할 수도 있게 되었다.

```javascript
const asyncIterator = function() {
  let num = 3;
  return {
    next() {
      if (num) {
        return Promise.resolve({
          value: num--,
          done: false,
        });
      } else {
        return Promise.resolve({ 
          done: true 
        });
      }
    }
  }
}

const iterator = asyncIterator();

(async function() {
  await iterator.next().then(console.log); // { value: 3, done: false }
  await iterator.next().then(console.log); // { value: 2, done: false }
  await iterator.next().then(console.log); // { value: 1, done: false }
  await iterator.next().then(console.log); // { done: true }
})();
```

##### Object rest/spread 문법

배열에서 사용되던 rest/spread 문법이 객체에서도 사용가능하게 되었다.

```javascript
const obj = { one: 1, two: 2, three: 3 };
const { one, ...rest } = obj;
const newObj = { ...rest, four: 4 };
console.log(rest, newObj); // { two: 2, three: 3 } { two: 2, three: 3, four: 4 }
```

##### `Promise.prototype.finally`

Promise 객체를 `then` 이나 `catch` 로 처리한 후에 try-catch 문처럼 `finally` 를 쓸 수 있게 되었다.

```javascript
const timeout = () => {
  return new Promise((res) => {
    setTimeout(() => { res("Promise 끝"); }, 1000);
  });
};

timeout()
.then(console.log)
.finally(() => console.log("무조건"));
```

<br/>

#### ES10 (ES2019)

##### `Array.prototype.flat()/flatMap()`

배열안에 배열 혹은 비어있는 원소가 있을 때 하나의 배열로 합쳐주는 메서드가 `flat` 이고 `map` 을 한 다음에 `flat` 하는 메서드가 `flatMap` 이다.

```javascript
const arr = [[1,2,3], 4, , 5];

console.log(arr.flat()); // [ 1, 2, 3, 4, 5 ]

console.log(arr.flatMap(a => {
  if (a instanceof Array) {
    return a.length;
  } else if (Number.isInteger(a)) {
    return a*2;
  } else {
    return undefined;
  }
})); // [ 3, 8, 10 ]
```

##### `Object.fromEntries()`

`Object.entries` 와는 반대로 key/value 쌍의 배열로부터 객체를 만드는 메서드이다.

```javascript
const arr = [['one', 1], ['two', 2], ['three', 3]];
console.log(Object.fromEntries(arr)); // { one: 1, two: 2, three: 3 }
```

##### `String.prototype.trimStart()/trimEnd()`

앞/뒤의 공백을 지우는 메서드로, 기존에 `String.prototype.trimLeft/trimRight` 이 있지만 `String.prototype.padStart/padEnd` 와 일치시키기 위해 나왔다.

```javascript
const target = "    target    ";
console.log(target.trimStart());
console.log(target.trimEnd());
```

```
target
    target
```

##### `Symbol.prototype.description`

심볼을 정의할 때 디버깅 목적으로 문자열을 인자로 전달하여 만들기도 하는데 이를 보기 위한 프로퍼티이다.

```javascript
const symbol = Symbol('description');
console.log(symbol.description);
```

##### catch의 에러인자 생략 가능

try-catch 문에서 catch의 인자로 에러인자가 오는데 이를 사용하지 않는다면 생략가능하다.

```javascript
try { console.log('try'); } catch {} // try
```

<br/>

#### 참고

* [ECMAScript-new-feature-list, ES2018](https://github.com/daumann/ECMAScript-new-features-list/blob/master/ES2018.MD)
* [ECMAScript-new-feature-list, ES2019](https://github.com/daumann/ECMAScript-new-features-list/blob/master/ES2019.MD)
* [ZeroCho님, ES2019(ES10)의 변화](https://www.zerocho.com/category/ECMAScript/post/5c909bfe5a8005001ffb3f14)


***


### ES11 (ES2020) 의 특징들

##### `String.prototype.matchAll`

기존의 `String.prototype.match` 의 기능은 일치하는 결과값 외에는 아무런 정보도 주지 않는다. 따라서 이 기능에서 다양한 정보들을 주도록 확장된 함수이다.

* `String.prototype.match`

```javascript
const text = "From 2019.01.29 to 2019.01.30";
const regexp = /(?<year>\d{4}).(?<month>\d{2}).(?<day>\d{2})/gu;
const results = text.match(regexp);

console.log(results);
// [ '2019.01.29', '2019.01.30' ]
```

* `String.prototype.matchAll`

```javascript
const text = "From 2019.01.29 to 2019.01.30";
const regexp = /(?<year>\d{4}).(?<month>\d{2}).(?<day>\d{2})/gu;
const results = Array.from(text.matchAll(regexp));

console.log(results);
// [
//   [
//     '2019.01.29',
//     '2019',
//     '01',
//     '29',
//     index: 5,
//     input: 'From 2019.01.29 to 2019.01.30',
//     groups: [Object: null prototype] { year: '2019', month: '01', day: '29' }
//   ],
//   [
//     '2019.01.30',
//     '2019',
//     '01',
//     '30',
//     index: 19,
//     input: 'From 2019.01.29 to 2019.01.30',
//     groups: [Object: null prototype] { year: '2019', month: '01', day: '30' }
//   ]
// ]
```

##### Dynamic import

동적으로 모듈을 불러오는 함수로 프라미스를 반환하며 완료(fulfilled)되면 요청한 모듈의 네임스페이스에 해당하는 객체를 반환한다.

```javascript
(async () => {
    const module = await import('./test-module.js');
    module.func();
});
```

##### BigInt

7번째 원시타입으로, `MAX_SAFE_INTEGER` 를 넘어가는 값을 사용할 수 있으며 `n` 이 접미사로 붙는다.

```javascript
const bigint = BigInt(Number.MAX_SAFE_INTEGER + 1);
console.log(bigint, typeof bigint);
// 9007199254740992n bigint
```

##### `Promise.allSettled`

기존의 `Promise.all` 은 거부(rejected)되는 프라미스가 하나라도 있을 경우 해당 결과값만 리턴했지만 이 함수를 사용하면 거부/완료 모두에 대한 결과값을 배열로 반환한다.

```javascript
const p1 = Promise.resolve("완료1");
const p2 = Promise.reject("거부");
const p3 = Promise.resolve("완료2");

Promise.all([p1, p2, p3]).then(console.log).catch(console.log);
Promise.allSettled([p1, p2, p3]).then(console.log);

/*
[
  { status: 'fulfilled', value: '완료1' },
  { status: 'rejected', reason: '거부' },
  { status: 'fulfilled', value: '완료2' }
]
거부
*/
```

##### `globalThis`

호스트 환경마다 `this` 객체의 값이 달랐는데 이 부분을 알아서 통합시켜주었다. Node.js에선 `global` 객체로, 브라우저에선 `window` 객체로 나온다.

##### Optional chaining

객체의 깊이가 깊어짐에 따라서 프로퍼티의 존재여부를 먼저 판단해야 하는데 그 때마다 `&&` 연산자를 사용해서 가독성에 좋지 못했다. 이를 `?.` 로 해결하였다.

```javascript
const book = {
    entities: {
        hashtags: ['test']
    }
};

const hashtags = book.entities && book.entities.hashtags;

// Optional chaining
const hashtags = book.entities?.hashtags;
```

##### Nullish coalescing operator (null 병합 연산자)

보통 기본 값을 할당하기 위해서 `||` 를 사용하여 좌변이 `null` 이나 `undefined` 이면 다른 값을 사용하는 방식을 많이 사용하였다. 이를 `??` 로 해결하였다.

```javascript
const values = {
    nullValue: null
};

const value = values.nullValue ?? "default value";
```

이외에도 for-in 메커니즘 확정, `import.meta` 등이 있으니 필요하다면 공부하도록 하자.

<br/>

#### 참고

* [TOAST UI, ECMAScript 2020의 새로운 점](https://ui.toast.com/weekly-pick/ko_20200409/)
* [DEV, ES2020 Features in simple examples](https://dev.to/carlillo/es2020-features-in-simple-examples-1513)


***

### 이벤트 전파

생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM트리를 통해 전파됩니다.

<br/>

> #### [이벤트 버블링](#gear-이벤트버블링)
>
> 이벤트가 하위 요소에서 상위 요소 방향으로 전파

<br/>

> #### [이벤트 캡처링](#gear-이벤트캡처링)
>
> 이벤트가 상위 요소에서 하위 요소 방향으로 전파

<br/>

+) 이벤트 버블링과 캡처링를 막기 위해서 [e.stopPropagation()](#gear-stoppropagation)을 사용합니다.
해당 웹 API를 통해 이벤트가 전파되는 것을 막을 수 있습니다.

<br/>

> #### 타깃 단계
>
> 이벤트가 이벤트 타깃에 도달

<br/>

#### [이벤트 위임](#gear-이벤트위임): 이벤트 버블링 활용하기

이벤트 위임을 사용하지 않고, 동일한 이벤트를 일일히 수동으로 달아주기에는 코드 낭비가 너무 심합니다.

따라서 부모 요소에 이벤트를 부여해 버블링을 통해 하위 요소를 동작시킬때도 해당 이벤트가 발생하도록 만드는 것이 바람직합니다.

<br/>

아래와 같은 상황에서

```html
<div class="itemList">
  <li>
    <input type="checkbox" id="item1" />
    <label for="item1">1</label>
  </li>
  <li>
    <input type="checkbox" id="item2" />
    <label for="item2">2</label>
  </li>
</div>
```

- case1: 각각 이벤트들을 부여해주기
  inputs의 내부 input에 각각 이벤트를 달아주었습니다.

```js
let inputs = document.querySelectorAll('input');
inputs.forEach((input) => {
  input.addEventListener('click', () => {
    alert('clicked');
  });
});
```

- case2: 부모 요소에 이벤트 부여하기
  부모 요소인 itemList를 클릭했을 때, 이벤트 버블링을 통해 checkbox type을 클릭했을 경우, 이벤트가 똑같이 동작하도록 만들었습니다.

```js
let itemList = document.querySelector('.itemList');
itemList.addEventListener('click', (e) => {
  console.log(e);
  if (e.target.type === 'checkbox') {
    alert('click');
  }
});
```

이처럼 이벤트 버블링을 통한 이벤트 위임은 하위 요소에 각각의 이벤트를 붙이지 않고도 상위 요소에서 하위 요소의 이벤트들을 제어할 수 있습니다.

<br/>

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: 이벤트버블링

- 이벤트 버블링(Event Bubbling)은 특정 화면 요소에서 이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미합니다. 아래와 같은 그림처럼요.

<!-- <img src="../../Images/important-4/event-bubbling.png" width="600px"> -->

> 하위의 클릭 이벤트가 상위로 전달되어 가는 그림

#### :gear: 이벤트캡처링

- 이벤트 캡처링(Event Capturing)은 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식입니다.

<!-- <img src="../../Images/important-4/event-capturing.png" width="600px"> -->

> 클릭 이벤트가 발생한 지점을 찾아내려 가는 그림

<br/>

```html
<div class="outside">
  녹색 영역
  <div class="middle">
    하늘색 영역
    <div class="inner">
      핑크색 영역
      <div class="float">회색</div>
    </div>
  </div>
</div>
```

<br/>

```js
const outside = document.querySelector('.outside');
const middle = document.querySelector('.middle');
const inner = document.querySelector('.inner');
const float = document.querySelector('.float');

function callback() {
  alert(this.className + ' is Capturing!');
}

outside.addEventListener('click', callback, true);
middle.addEventListener('click', callback, true);
inner.addEventListener('click', callback, true);
float.addEventListener('click', callback, true);
```

<br/>

위 코드는 이벤트 캡처링의 예시입니다. `float`을 클릭하면 `가장 상위 부모요소`인 `outside`의 이벤트부터 발생합니다. 이때 `addEventListener`함수의 `두번째 인자`로 전달된 `true`는 `이벤트를 캡처링해야하는지 여부`를 지정합니다.

<br/>

```js
target.addEventListener(type, listener[, useCapture]);
```

<br/>

- `type` : 이벤트의 이름을 지정하는 문자열. 대소문자 구별. (click, keypress 등..)
- `listener` : 이벤트가 발생할때 호출할 이벤트 리스너 함수.
- `useCapture` : 캠쳐링을 사용할지 여부를 지정하는 Boolean. default는 false 입니다. (선택사항)

#### :gear: 이벤트위임

- 캡처링과 버블링을 활용하면 강력한 이벤트 핸들링 패턴인 이벤트 위임(event delegation) 을 구현할 수 있습니다.
  이벤트 위임은 비슷한 방식으로 여러 요소를 다뤄야 할 때 사용됩니다. 이벤트 위임을 사용하면 요소마다 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있습니다.
  공통 조상에 할당한 핸들러에서 `event.target`을 이용하면 실제 어디서 이벤트가 발생했는지 알 수 있습니다. 이를 이용해 이벤트를 핸들링합니다.

#### :gear: stopPropagation

- “난 이렇게 복잡한 이벤트 전달 방식 알고 싶지 않고, 그냥 원하는 화면 요소의 이벤트만 신경 쓰고 싶어요.”라고 생각하시는 분들이 충분히 있을 수 있습니다. 실제로 마감 기한에 쫓기는 상황에서 이런 동작 방식을 정확히 이해하는 시간보다는 구현에 더 많은 시간을 쏟아야 하기 때문입니다. 그럴 때는 아래처럼 stopPropagation() 웹 API를 사용합니다.

```js
function logEvent(event) {
  event.stopPropagation();
}
```

위 API는 해당 이벤트가 전파되는 것을 막습니다. 따라서, 이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 발생시키고 상위 요소로 이벤트를 전달하는 것을 방해합니다. 그리고 이벤트 캡쳐의 경우에는 클릭한 요소의 최상위 요소의 이벤트만 동작시키고 하위 요소들로 이벤트를 전달하지 않습니다.

<br/>

#### 참고

- [blog, 프론트엔드 면접 문제 은행](https://velog.io/@wkahd01/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A9%B4%EC%A0%91-%EB%AC%B8%EC%A0%9C-%EC%9D%80%ED%96%89-HTML-%EC%A7%88%EB%AC%B8-%EB%8B%B5%EB%B3%80#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%A0%84%ED%8C%8C)
- [blog, 이벤트 버블링,캡처,위임](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
- [blog, 이벤트 버블링(Bubbling) & 캡쳐링(Capturing)](https://thisblogfor.me/web/bubblingcapturing/)


***


### ES6에서 Arrow 함수를 언제, 왜 쓸까?

<br/>

Arrow 함수(arrow function)를 언제, 왜 사용하는지 그 이유들을 알아봅시다.

<br/>

#### 1. 함수 본연의 기능을 아주 잘 표현하는 문법입니다.

보통 프로그래밍할 때 function 문법은 아래와 같은 이유로 많이 사용합니다.

- 여러가지 기능을 하는 코드를 한 단어로 묶고 싶을 때 (그리고 나중에 재사용할 때)
- [입출력기능](#gear-입출력기능)을 만들 때

<br/>

그리고 `arrow function`을 사용하면 함수 본연의 입출력기능을 아주 직관적으로 잘 표현해줍니다.

<br/>

```js
let 두배만들기 = (x) => {
  return x * 2;
};

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br/>

`arrow function`을 쓰면 입출력기능이 쉽고 예쁘게 표현되는 것이 `arrow function`를 쓰는 이유 중 하나입니다.

<br/>

<br/>

#### 2. 소괄호 생략이 가능합니다.

파라미터가 하나라면 소괄호를 생략 가능합니다.

<br/>

```js
let 두배만들기 = (x) => {
  return x * 2;
};

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br/>

이렇게도 가능합니다.

<br/>

<br/>

#### 3. 중괄호 생략이 가능합니다.

중괄호 안에 return 한줄 뿐이라면 중괄호와 return도 생략가능합니다.

<br/>

```js
let 두배만들기 = (x) => x * 2;

console.log(두배만들기(4));
console.log(두배만들기(8));
```

<br/>

생략하니 이제 x가 어떻게 변하는 함수인지 입출력기능이 바로 한눈에 들어오는 걸 볼 수 있습니다.

<br/>

<br/>

#### 4. `arrow function`을 쓰면 내부에서 `this`값을 쓸 때 밖에 있던 `this`값을 그대로 사용합니다.

`arrow function`은 어디서 쓰던 내부의 `this` 값을 변화시키지 않습니다. <br/>

또한 바깥에 있던 `this`의 의미를 그대로 내부에서도 사용합니다. 이게 `arrow function`을 쓰는 핵심 이유입니다. <br/>

예시를 보겠습니다.

<br/>

```js
let 오브젝트1 = {
  함수: function () {
    console.log(this);
  },
};

오브젝트1.함수();
```

<br/>

위의 코드는 실행하면 무슨 결과가 나올까요? <br/>

> 결과: 함수()를 가지고 있는 오브젝트인 오브젝트1이 콘솔창에 출력됩니다.

<br/>

다른 예시를 봅시다.

<br/>

```js
let 오브젝트1 = {
  함수: () => {
    console.log(this);
  },
};

오브젝트1.함수();
```

<br/>

위의 코드는 출력하면 어떤 결과가 나올까요? <br/>

> 결과: `window`라는게 출력됩니다. 여기선 `this`가 `window`입니다. <br/>
> 함수의 주인인 오브젝트1이 출력되지 않는 이유는 `this`값은 함수를 만나면 항상 변하는데, <br/> > `arrow function` 안에서는 변하지 않고 밖에 있던 `this`를 그대로 씁니다. <br/>
> (오브젝트 밖에 있던 `this`는 `window`입니다.)/

<br/>

왜냐면 `arrow function`은 외부에 있던 `this`를 그대로 내부로 가져와서 사용하는 함수기 때문입니다. <br/>

내가 예측하던 `this`값과 달라질 수도 있으니 이는 장점이 아니라 단점이 될 수도 있습니다.

`this`의 뜻이 달라지는 것 처럼 일반 `function`과 용도가 완전 같지 않기 때문에

일반 `function`을 항상 대체할 수 있는 문법이 아닙니다. 그것만 주의합시다.

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: 입출력기능

<br/>

2를 집어넣으면 x + 2를 출력해주는 함수를 어떻게 만들어쓸까요?

<br/>

```js
function 더해주세요(x) {
  return x + 2;
}
```

<br/>

위와 같은 문법을 이용해서 만들어 사용합니다. 함수의 소괄호안에는 `input` 역할을 하는 `파라미터`가 있고 <br/>

함수내에 `return` 이라는 것은 `output` 역할을 하는 키워드입니다. 그럼 이제 더해주세요(2); 이렇게 사용하면? <br/>

4가 이 자리에 남게 됩니다. 소괄호에 뭔가 집어넣으면 return을 이용해 뭔가 뱉어내는 것. 이게 바로 함수의 입출력 기능입니다.

<br/>

#### 참고

- [코딩애플, Arrow function은 function을 대체하는 신문법이 아님](https://codingapple.com/unit/es6-3-arrow-function-why/)