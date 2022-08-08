---
title: 자바스크립트 핵심 개념 모음
sidebar_label: JavaScript
sidebar_position: 2
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