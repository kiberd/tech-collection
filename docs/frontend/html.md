---
title: HTML
sidebar_label: HTML
sidebar_position: 4
toc_min_heading_level: 3
toc_max_heading_level: 3
---

import TOCInline from '@theme/TOCInline';

<TOCInline toc={toc} />

***

### DOCTYPE

Document Type의 약자로, **HTML이 어떤 버전으로 작성되었는지 미리 선언하여 웹브라우저가 내용을 올바로 표시할 수 있도록 해주는 것** 이다. `<!DOCTYPE>` 으로 선언하는데 이걸 해주지 않으면 **호환 모드(quirks mode)** 로 동작한다. 호환 모드의 경우 각 브라우저마다 문서를 나타내는 방식이 다르기 때문에 크로스 브라우징 이슈가 훨씬 심해지게 된다.

#### DTD(Document Type Definition)

DTD(Document Type Definition)란 문서 형식을 정의해놓은 것으로 DOCTYPE을 명시할 때 사용한다. 즉, HTML 문서가 어떤 문서 형식을 따르는지 DOCTYPE에서 DTD를 지정하는 것이다.

예시로 아래와 같은 것들이 있고 [W3C Recommended list of Doctype declarations](https://www.w3.org/QA/2002/04/valid-dtd-list.html) 에서 더욱 자세하게 확인 가능하다.

* XHTML 1.1
* XHTML 1.0
  * Strict DTD
  * Transitional DTD
  * Frameset DTD
* HTML 4.01
  * Strict DTD
  * Transitional DTD
  * Frameset DTD
* HTML 5

현 시점에선, HTML 5의 DTD로 DOCTYPE을 명시하는 것이 제일 바람직하다.

```html
<!DOCTYPE html>
```

<br/>

#### 참고

* [What is DOCTYPE?](https://stackoverflow.com/questions/414891/what-is-doctype)
* [비표준 모드 quirks mode, 표준 모드 standards mode 차이와 DOCTYPE](https://aboooks.tistory.com/169)
* [DOCTYPE(문서형 정의) 선언](https://webdir.tistory.com/40)
* [What is difference between XHTML and HTML?](https://stackoverflow.com/questions/4153403/what-is-difference-between-xhtml-and-html)


***


### data- 속성

**DOM에 데이터를 저장할 수 있는 사용자 정의 데이터 속성** 으로 `data-` 다음 오는 값이 데이터가 된다. 이 속성은 사용하고자 하는 용도에 적합한 속성이나 요소가 없을 때 사용하며 해당 웹페이지가 **독자적으로 사용하는 값** 이다. 즉, 웹페이지와 독립적인 소프트웨어가 이 속성을 사용해서는 안된다.

예를 들어, 음악 사이트에서 앨범 트랙의 음악을 리스트 형식으로 나타내는데 그걸 시간 순으로 정렬하기 위해서 `data-` 속성으로 음악 시간을 삽입한다고 하자.

```html
<ol>
  <li data-length="2m11s">빨간맛</li>
  ...
</ol>
```

만약 이 음악 사이트와 전혀 상관이 없는 외부에서 음악 시간을 알아내기 위해 사용한다면 목적에 부합하지 않는 것이다. 따라서, `data-` 속성은 해당 사이트만의 자체 스크립트를 위한 속성이라고 할 수 있다.

<br/>

#### 참고

* [W3, Custom Data Attribute](https://www.w3.org/TR/2011/WD-html5-20110525/elements.html#custom-data-attribute)
* [프론트엔드 인터뷰 핸드북, `data-` 속성은 무엇에 좋은가요?](https://github.com/yangshun/front-end-interview-handbook/blob/master/contents/kr/html-questions.md#data-속성은-무엇에-좋은가요)


***

### local storage vs session storage vs cookie

모두 클라이언트 상에서 key/value 쌍을 저장할 수 있는 메커니즘으로 **value는 반드시 문자열** 이어야 한다. 또한 모두 [동일 출처 정책(SOP)](https://github.com/baeharam/Must-Know-About-Frontend/blob/master/Notes/security/sop.md) 을 따르기 때문에 다른 도메인에서 접근할 수 없다.

|               | cookie           | local storage         | session storage         |
| ------------- | ---------------- | --------------------- | ----------------------- |
| 생성자        | 클라이언트/서버  | 클라이언트            | 클라이언트              |
| 지속시간      | 설정 여부에 따름 | 명시적으로 지울때까지 | 탭 / 윈도우 닫을 때까지 |
| 용량          | 5KB              | 5MB / 10MB            | 5MB                     |
| 서버와의 통신 | O                | X                     | X                       |
| 취약점        | XSS / CSRF 공격  | XSS 공격              | XSS 공격                |

<br/>

#### 참고

* [What is the difference between localStorage, sessionStorage, session and cookies?](https://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies)
* [프론트엔드 인터뷰 핸드북, `cookie`, `sessionStorage`, `localStorage` 사이의 차이점을 설명하세요](https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Korean/questions/html-questions.md#cookie-sessionstorage-localstorage-사이의-차이점을-설명하세요)
* [Local Storage vs Cookies](https://stackoverflow.com/questions/3220660/local-storage-vs-cookies)


***

### script, script async, script defer

* `<script>` : HTML 파싱이 중단되고 즉시 스크립트가 로드되며 로드된 스크립트가 실행되고 파싱이 재개된다.
* `<script async>` : HTML 파싱과 병렬적으로 로드가 되는데, 스크립트를 실행할 때는 파싱이 중단된다. 구글 애널리틱스와 같이 다른 스크립트가 의존하지 않는 독자적인 스크립트를 로드할 때 적합하다.
* `<script defer>` : HTML 파싱과 병렬적으로 로드가 되는데, 파싱이 끝나고 스크립트를 로드한다. 보통 `<body>` 태그 직전에 `<script>` 를 삽입하는 것과 동작은 같지만 브라우저 호환성에서 다를 수 있으므로 그냥 `<body>` 태그 직전에 삽입하는 것이 좋다.

**주의할 점은 async와 defer의 경우 `src` 속성이 없으면 적용되지 않는다.**

<br/>

#### 참고

* [Script Tag - async & defer](https://stackoverflow.com/questions/10808109/script-tag-async-defer)

* [프론트엔드 인터뷰 핸드북, `<script>`, `<script async>`, `<script defer>` 사이의 차이점을 설명하세요](https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Korean/questions/html-questions.md#script-script-async-script-defer-사이의-차이점을-설명하세요)


***


### 시맨틱 마크업

시맨틱(Semantic)이란 "의미론적인" 의 뜻을 가지며 마크업(Markup)이란 HTML 태그로 문서를 작성하는 것을 말한다. 따라서, 시맨틱 마크업이란 **의미를 잘 전달하도록 문서를 작성하는 것을 말한다.**

#### 작성방법

시맨틱 마크업을 하기 위해선 각 태그를 그 용도에 맞게 사용하여야 한다. 즉, 다음과 같은 것들을 말한다.

* 헤더/푸터에 `<header>` 와 `<footer>` 사용
* 메인 컨텐츠에 `<main>` 과 `<section>` 사용
* 독립적인 컨텐츠에 `<article>` 사용
* 최상위 제목으로 `<h1>` 사용
* 순서가 없는 목록으로 `<ul>` 과 `<li>` 사용
* 내비게이션에 `<nav>` 사용

이런 식으로 태그가 가지고 있는 의미에 맞게 사용하는 것인데, 이런 점 이외에도 CSS 스타일을 명시하는 태그를 사용하지 않는 것 또한 시맨틱 마크업의 한 종류이다. **즉, 태그가 가지는 의미 자체가 스타일이라면 이는 마크업 자체가 스타일을 갖는 것이기 때문에 시맨틱 마크업에 적합하지 않다.**

예를 들어, 동일한 효과를 부여하는 `<strong>` 과 `<b>` 태그가 있다. 둘은 동일하게 글자색을 진하게 하지만 `<b>` 태그의 경우는 그 자체가 "bold" 의 약어이기 때문에 태그 자체가 스타일을 가진다고 할 수 있다. 하지만 `<strong>` 의 경우는 "그 안의 내용이 다른 내용보다 더 강조되어야 한다" 라는 의미를 가지기 때문에 시맨틱 마크업에 더 적합하다.

<br/>

#### 특징

* 검색엔진이 시맨틱 태그를 중요한 키워드로 간주하기 때문에 **검색엔진 최적화(SEO)에 유리하다.**
* **웹 접근성** 측면에서, 시각장애가 있는 사용자로 하여금 그 의미를 훨씬 잘 파악할 수 있다.
* 단순한 `div` , `span` 으로 둘러싸인 요소들보다 코드를 볼 때 **가독성이 더 좋다.**

실무에서 시맨틱 마크업이 완벽하게 쓰이는 것은 이상적이긴 하지만, 이러한 특징들을 고려하고 웹사이트를 구성하는 것이 많은 측면에서 바람직하다.

<br/>

#### 참고

* [Stackoverflow, What are the benefits of using semantic HTML?](https://stackoverflow.com/questions/1729447/what-are-the-benefits-of-using-semantic-html)
* [Stackoverflow, What's the difference between  and ,  and ?](https://stackoverflow.com/questions/271743/whats-the-difference-between-b-and-strong-i-and-em)
* [MDN, Semantics](https://developer.mozilla.org/ko/docs/Glossary/Semantics)


***

