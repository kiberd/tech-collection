---
title: General
sidebar_label: General
sidebar_position: 2
toc_min_heading_level: 3
toc_max_heading_level: 3
---

import TOCInline from '@theme/TOCInline';

<TOCInline toc={toc} />

***

### https://xiubindev.tistory.com/119


***


### CSR(Client Side Rendering)과 SSR(Server Side Rendering)

#### SPA와 MPA

* **SPA (Single Page Application)**

하나의 HTML 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션이다.

* **MPA (Multiple Page Application)**

사용자가 페이지를 요청할 때마다, 웹 서버가 요청한 UI와 필요한 데이터를 HTML로 파싱해서 보여주는 방식의 웹 어플리케이션이다.

전통적인 방식을 이용한다면, SPA가 사용하는 렌더링 방식은 CSR이고, MPA가 사용하는 렌더링 방식은 SSR이다. 각 방식의 동작방식과 장단점을 알아보고, 전통적인 방식을 벗어나, SPA에서도 적절히 SSR을 구현했을 때의 장점과 그 이유를 알아보자.

<br/>

#### CSR

<!-- <img src="../../images/frontend/CSR.png"> -->

[이미지 출처](https://medium.com/@adamzerner/client-side-rendering-vs-server-side-rendering-a32d2cf3bfcc)

CSR에선 브라우저가 서버에 HTML과 JS 파일을 요청한 후 로드되면 사용자의 상호작용에 따라 JS를 이용해서 동적으로 렌더링을 시킨다.

##### :+1: 장점

* 첫 로딩만 기다리면, 동적으로 빠르게 렌더링이 되기 때문에 사용자 경험(UX)이 좋다.
* 서버에게 요청하는 횟수가 훨씬 적기 때문에 서버의 부담이 덜하다.

##### :-1: 단점

* 모든 스크립트 파일이 로드될 때까지 기다려야 한다. 
  * 리소스를 청크(Chunk) 단위로 묶어서 요청할 때만 다운받게 하는 방식으로 완화시킬 수 있지만 완벽히 해결할 수는 없다.
* 검색엔진의 검색 봇이 크롤링을 하는데 어려움을 겪기 때문에 검색엔진 최적화(Search Engine Optimization)의 문제가 있다.
  * 구글 봇의 경우는 JS를 지원하지만, 다른 검색엔진의 경우 그렇지 않기 때문에 문제가 된다.

<br/>

#### SSR

<!-- <img src="../../images/frontend/SSR.png"> -->

[이미지 출처](https://medium.com/@adamzerner/client-side-rendering-vs-server-side-rendering-a32d2cf3bfcc)

SSR에선 브라우저가 페이지를 요청할 때마다 해당 페이지에 관련된 HTML, CSS, JS 파일 및 데이터를 받아와서 렌더링을 시킨다.

##### :+1: 장점

* 초기 로딩 속도가 빠르기 때문에 사용자가 컨텐츠를 빨리 볼 수 있다.
* JS를 이용한 렌더링이 아니기 때문에 검색엔진 최적화가 가능하다.

##### :-1: 단점

* 매번 페이지를 요청할 때마다 새로고침 되기 때문에 사용자 경험이 SPA에 비해서 좋지 않다.
* 서버에 매번 요청을 하기 때문에 서버의 부하가 커진다.

<br/>

#### 참고

* [Client-Side Rendering versus Server-Side Rendering!](https://altalogy.com/blog/client-side-rendering-vs-server-side-rendering/)
* [What are Single Page Applications(SPA)?](https://dev.to/kendyl93/what-are-single-page-applications-spa-32bh)
* [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartlabs/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
* [싱글 페이지 어플리케이션에서의 검색 엔진 최적화 (SEO)](https://funnygangstar.tistory.com/entry/싱글-페이지-어플리케이션에서의-검색-엔진-최적화-SEO)
* [Google I/O 2019: Day 3 후기](https://hyunseob.github.io/2019/05/26/google-io-2019-day-3/)
* [[주니어탈출기] 서버사이드렌더링(SSR) & 클라이언트사이드렌더링(CSR)](https://velog.io/@zansol/확인하기-서버사이드렌더링SSR-클라이언트사이드렌더링CSR)
* [SPA 단점에 대한 단상](https://m.mkexdev.net/374)
* [CSR vs SSR](https://medium.com/@adamzerner/client-side-rendering-vs-server-side-rendering-a32d2cf3bfcc)


***

### 브라우저의 렌더링 원리

브라우저가 화면에 나타나는 요소를 렌더링 할 때, 웹킷(Webkit)이나 게코(Gecko) 등과 같은 [렌더링엔진](#gear-렌더링엔진) 을 사용합니다. 렌더링 엔진이 HTML, CSS, Javascript로 렌더링할 때 [CRP](#gear-crp)라는 프로세스를 사용하며 다음 단계들로 이루어집니다.

<br/>

1. **HTML를 [파싱](#gear-파싱) 후, [DOM](#gear-dom)트리를 구축합니다.**
2. **CSS를 파싱 후, [CSSOM](#gear-cssom)트리를 구축합니다.**
3. **Javascript를 실행합니다.**
   - 주의! HTML 중간에 스크립트가 있다면 HTML 파싱이 중단됩니다.
4. **DOM과 CSSOM을 조합하여 [렌더트리](#gear-렌더트리)를 구축합니다.**
   - 주의! `display: none` 속성과 같이 화면에서 보이지도 않고 공간을 차지하지 않는 것은 렌더트리로 구축되지 않습니다.
5. **뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기를 계산합니다. ([Layout](#gear-layout) 단계)**
6. **계산한 위치/크기를 기반으로 화면에 그립니다. ([Paint](#gear-paint) 단계)**

<br/>

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: 렌더링엔진

- 브라우저 마다 다르지만, 브라우저는 렌더링을 수행하는 렌더링 엔진을 가지고 있습니다. 크롬은 블링크(Blink), 사파리는 웹킷(Webkit) 그리고 파이어폭스는 게코(Gecko)라는 렌더링 엔진을 사용합니다.

#### :gear: CRP

- CRP (Critical Rendering Path, 중요 렌더링 경로)는 브라우저가 HTML, CSS, Javascipt를 화면에 픽셀로 변화하는 일련의 단계를 말합니다. CRP는 Document Object Model (DOM), CSS Object Model (CSSOM), 렌더 트리 그리고 레이아웃을 포함합니다.

#### :gear: 파싱

- 파싱은 하나의 프로그램을 런타임 환경(예를 들면, 브라우저 내 자바스크립트 엔진)이 실제로 실행할 수 있는 내부 포맷으로 분석하고 변환하는 것을 의미합니다. 즉, 파싱은 문서의 내용을 토큰(token)으로 분석하고, 문법적 의미와 구조를 반영한 파스 트리(parse tree)를 생성하는 과정입니다.

#### :gear: DOM

- **DOM(Document Object Model)이란?** 웹 페이지를 이루는 태그들을 자바스크립트가 이용할 수 있게끔 브라우저가 트리구조로 만든 객체 모델을 의미합니다. 영어 뜻풀이 그대로 하자면 문서 객체 모델을 의미합니다. 문서 객체란 html, head, body와 같은 태그들을 javascript가 이용할 수 있는 (메모리에 보관할 수 있는) 객체를 의미합니다. DOM은 HTML과 스크립팅 언어(JavaScript)를 서로 이어주는 역할을 합니다.

#### :gear: CSSOM

- **CSSOM(CSS Object Model)이란?** CSS 내용을 파싱하여 자료를 구조화 한 것을 CSSOM이라고 합니다. 즉 DOM처럼 CSS의 내용을 해석하고 노드를 만들어 트리 구조로 만든 것을 CSSOM이라 합니다.

#### :gear: 렌더트리

- **렌더트리(Render Tree)란?** 렌더 트리는 CSSOM과 DOM 트리의 결합으로 만들어집니다. 렌더 트리는 웹 페이지에 나타낼 각 요소들의 위치(Layout, 레이아웃)을 계산하는데 사용되고 픽셀을 화면에 렌더링하는 페인트(Paint) 즉 화면에 요소들을 표현하는 프로세스를 위해 존재합니다.

#### :gear: Layout

- **Layout(Reflow)이란?** 뷰포트 내에서 노드의 정확한 위치와 크기를 계산합니다. 이것이 바로 'Layout' 단계이며 경우에 따라 'Reflow' 라고도 합니다.

#### :gear: Paint

- **Paint란?** 노드와 해당 노드의 계산된 스타일 및 기하학적 형태에 대해 파악했으므로, 렌더링 트리의 각 노드를 화면의 실제 픽셀로 변환하는 마지막 단계에 이러한 정보를 전달합니다. 이 단계를 흔히 '페인팅' 또는 '래스터화'라고 합니다.

<br/>

#### 참고

- [Medium, 브라우저의 렌더링 과정](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/frontend/browser-rendering.md),
- [Github, 브라우저의 렌더링 원리](https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce)
- [MDN Web Docs, 중요 렌더링 경로](https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path)
- [Blog, 파싱이란? 파싱의 뜻은 무엇일까?(번역)](https://oneroomtable.tistory.com/entry/%ED%8C%8C%EC%8B%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-%EB%B2%88%EC%97%AD)
- [Blog, DOM이란? 가상 돔 (Virtual DOM )이 나오게 된 이유](https://dev-cini.tistory.com/10)
- [Blog, CSSOM(CSS Object Model) 이란?](https://itworldyo.tistory.com/151)
- [Blog, 렌더 트리 (Render Tree)](https://sgcomputer.tistory.com/172)
- [Blog, 레이아웃(리플로우) 및 페인트](https://velog.io/@seokkitdo/%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0-%EB%B0%8F-%ED%8E%98%EC%9D%B8%ED%8A%B8)


***

### Reflow와 Repaint가 실행되는 시점

<br/>

#### [Reflow](#gear-Reflow)

- DOM 엘리먼트 추가, 제거 또는 변경
- CSS 스타일 추가, 제거 또는 변경
- CSS 스타일을 직접 변경하거나, 클래스를 추가함으로써 레이아웃이 변경될 수 있습니다. 엘리먼트의 길이를 변경하면, DOM 트리에 있는 다른 노드에 영향을 줄 수 있습니다.
- CSS3 애니메이션과 트랜지션. 애니메이션의 모든 프레임에서 리플로우가 발생합니다.
- offsetWidth 와 offsetHeight 의 사용. offsetWidth 와 offsetHeight 속성을 읽으면, 초기 리플로우가 트리거되어 수치가 계산됩니다.
- 유저 행동. 유저 인터랙션으로 발생하는 hover 효과, 필드에 텍스트 입력, 창 크기 조정, 글꼴 크기 변경, 스타일시트 또는 글꼴 전환등을 활성화하여 리플로우를 트리거할 수 있습니다.

#### [Repaint](#gear-Repaint)

- 가시성이 변경되는 순간 (opacity, background-color, visibility, outline)
- Reflow 가 실행된 순간 뒤에 실행됩니다.

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: Reflow

- 생성된 DOM 노드의 레이아웃 수치(너비, 높이, 위치 등) 변경 시 영향 받은 모든 노드의(자신, 자식, 부모, 조상(결국 모든 노드) ) 수치를 다시 계산하여(Recalculate), 렌더 트리를 재생성하는 과정을 Reflow 라고 합니다.

#### :gear: Repaint

- Reflow 과정이 끝난 후 재 생성된 렌더 트리를 다시 그리게 되는데 이 과정을 Repaint 라고 합니다.

<br/>

#### 참고

- [github, Reflow & Repaint](https://k0102575.github.io/articles/2020-11/reflow-repaint)


***


### 주소창에 google.com을 입력하면 일어나는 일

1. **사용자가 웹 브라우저를 통해 google.com 을 입력하면 URL 주소 중 도메인 네임 부분을 [DNS](#gear-dns) 서버에서 검색합니다.**
2. **DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 [URL](#gear-url) 정보와 함께 전달합니다.**
3. **브라우저는 [HTTP](#gear-http) [프로토콜](#gear-프로토콜)을 사용하여 요청 메시지를 생성하고 HTTP 요청 메시지는 [TCP](#gear-tcp)/[IP](#gear-ip) 프로토콜을 사용하여 서버로 전송됩니다.**
4. **서버는 [response](#gear-response) 메시지를 생성하여 다시 브라우저에게 데이터를 전송합니다.**
5. **브라우저는 response를 받아 [파싱](#gear-파싱)하여 화면에 [렌더링](#gear-렌더링)합니다.**

<br/>

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: DNS

- 도메인 이름 시스템(DNS)은 사람이 읽을 수 있는 도메인 이름(예: www.amazon.com )을 머신이 읽을 수 있는 IP 주소(예: 192.0.2.44)로 변환합니다. 모든 통신에는 주소가 필요합니다. 출발지와 도착지의 주소를 알아야 통신을 할 수 있습니다. 우리는 이 주소를 IP라고 부릅니다. IP 주소로 변환하는 과정에 개입하는 것이 DNS 입니다.

#### :gear: URL

- URL(Uniform Resource Locator)은 통합 자원 지시자로 인터넷의 리소스를 가리키는 표준 명칭으로 서버의 자원을 요청할 때 사용됩니다. URL을 통해 인터넷 상의 모든 리소스를 요청할 수 있으며, HTTP, FTP 등의 자원 요청도 가능합니다.

#### :gear: HTTP

- HTTP(HyperText Transfer Protocol)은 TCP 기반의 클라이언트와 서버 사이에 이루어지는 요청/응답 프로토콜입니다. HTTP는 Text Protocol로 사람이 쉽게 읽고 쓸 수 있습니다. 프로토콜 설계상 클라이언트가 요청을 보내면 반드시 응답을 받아야 합니다. 응답을 받아야 다음 request를 보낼 수 있습니다.

#### :gear: 프로토콜

- 프로토콜은 통신하기 위한 약속들을 기술적으로 잘 정의해 둔 것입니다. 데이터를 송수신하는 순서와 내용을 결정합니다. HTTP, TCP/IP, UDP 모두 프로토콜입니다.

#### :gear: TCP

- TCP (전송 제어 프로토콜)은 두 개의 호스트를 연결하고 데이터 스트림을 교환하게 해주는 중요한 네트워크 프로토콜입니다. TCP는 데이터 전송을 제어하고 데이터를 어떻게 보낼 지, 어떻게 맞출 지 정합니다. 또한 데이터와 패킷이 보내진 순서대로 전달하는 것을 보장해줍니다.신뢰성과 연결성을 책임지기 위한 프로토콜이 TCP입니다. 호스트와 호스트간의 데이터 전송은 IP(인터넷 계층 프로토콜)에 의지하면서 동시에 신뢰성 있는 전송에 대해서는 TCP가 책임지는 구조입니다.

#### :gear: IP

- IP (Internet Protocol)은 비신뢰성, 비연결지향 데이터그램 프로토콜로 패킷을 받아서 주소를 해석하고 경로를 결정하여 다음 호스트로 전송하는 역할을 합니다.

#### :gear: response

- HTTP 메시지는 서버와 클라이언트 간에 데이터가 교환되는 방식입니다. 메시지 타입은 두 가지가 있습니다. 요청(request)은 클라이언트가 서버로 전달해서 서버의 액션이 일어나게끔 하는 메시지고, 응답(response)은 요청에 대한 서버의 답변입니다.

#### :gear: 파싱

- 파싱은 하나의 프로그램을 런타임 환경(예를 들면, 브라우저 내 자바스크립트 엔진)이 실제로 실행할 수 있는 내부 포맷으로 분석하고 변환하는 것을 의미합니다. 즉, 파싱은 문서의 내용을 토큰(token)으로 분석하고, 문법적 의미와 구조를 반영한 파스 트리(parse tree)를 생성하는 과정입니다.

<br/>

#### 참고

- [blog, [네트워크]google.com을 입력하면 일어나는 일](https://bohyeon-n.github.io/deploy/network/internet-2.html)
- [AWS, DNS란 무엇입니까?](https://aws.amazon.com/ko/route53/what-is-dns/)
- [blog, URL이란](https://velog.io/@mer1-97/URL%EC%9D%B4%EB%9E%80)
- [mdn web docs, HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)
- [mdn web docs, 프로토콜](https://developer.mozilla.org/ko/docs/Glossary/Protocol)
- [mdn web docs, TCP](https://developer.mozilla.org/ko/docs/Glossary/TCP)
- [Blog, 파싱이란? 파싱의 뜻은 무엇일까?(번역)](https://oneroomtable.tistory.com/entry/%ED%8C%8C%EC%8B%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-%EB%B2%88%EC%97%AD)

***

### 브라우저 저장소의 차이점

#### LocatStorage

- 로컬 스토리지는 저장한 데이터를 지우지 않는 이상 영구적으로 보관이 가능합니다.([도메인](#gear-도메인)마다 별도로 로컬 스토리지가 생성됩니다.)
- 최대 크기: 5MB
- 사용 예시: 자동 로그인

#### SessionStorage

- 세션 종료 시 클라이언트에 대한 정보가 삭제됩니다.
- 최대 크기: 5MB
- 사용 예시: 입력 폼 정보, 비로그인 장바구니

#### [쿠키](#gear-쿠키)(Cookie)

- 웹 사이트에서 쿠키를 설정하면, 모든 웹 요청에는 쿠키 정보가 포함됩니다. => 서버 부담 증가
- 최대 크기: 4KB
- 사용 예시: 팝업 창

#### 서버 인증과 브라우저 저장소

- 주요 정보를 요청 헤더에 넣는 방법

  > 보안에 매우 취약합니다.

- Session, Cookie 방식

  > 서버 부하가 증가하고, 세션 하이재킹 공격에 취약합니다.

- [JWT](#gear-jwt) 방식
  > 별도의 브라우저 저장소에 저장하지 않고 JWT를 발급하고 검증하면 되어 확장성이 뛰어납니다.<br/>
  > 그러나 Payload 정보가 제한적이고, JWT길이가 길다는 단점 존재합니다.

<br/>

---

<br/>

#### :hammer_and_wrench: 용어 공부

#### :gear: 도메인

- ip는 사람이 이해하고 기억하기 어렵기 때문에 이를 위해서 각 ip에 이름을 부여할 수 있게 했는데, 이것을 도메인이라고 합니다.

#### :gear: 쿠키

- 쿠키(Cookie)란 인터넷 사용자가 어떠한 웹 사이트를 방문할 경우 그 사이트가 사용하고 있는 서버를 통해 인터넷 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일입니다.

#### :gear: JWT

- JWT(Json Web Token)란 Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token입니다. JWT는 토큰 자체를 정보로 사용하는 Self-Contained 방식으로 정보를 안전하게 전달합니다.

<br/>

#### 참고

- [blog, 프론트엔드 면접 문제 은행](https://velog.io/@wkahd01/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%A9%B4%EC%A0%91-%EB%AC%B8%EC%A0%9C-%EC%9D%80%ED%96%89-HTML-%EC%A7%88%EB%AC%B8-%EB%8B%B5%EB%B3%80#%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- [생활코딩, 도메인이란?](https://opentutorials.org/course/228/1450)
- [blog, 쿠키(Cookie)란?](https://stupidsecurity.tistory.com/9)
- [blog, JWT란?](https://mangkyu.tistory.com/56)