---
title: CSS
sidebar_label: CSS
sidebar_position: 5
---



import TOCInline from '@theme/TOCInline';

<TOCInline toc={toc} />

### Box Model

<p align="center">
	<img src="/img/box model.png"/>  
</p>

문서상의 요소들을 시각적인 목적을 위해서, 모든 요소를 하나의 "직사각형 박스"로 여기는 모델이다. 모든 박스는 아래 영역들을 갖는다.

* **컨텐츠 영역(Content Area)** : 글이나 이미지, 비디오 등 요소의 실제 내용을 포함한다.
* **안쪽여백 영역(Padding Area)** : 안쪽여백 경계(Padding Edge)가 감싼 영역으로 컨텐츠 영역을 요소의 안쪽 여백까지 포함하는 크기로 확장한다.
* **테두리 영역(Border Area)** : 테두리 경계(Border Edge)가 감싼 영역으로, 안쪽여백 영역을 요소의 테두리까지 포함하는 크기로 확장한다.
* **바깥여백 영역(Margin Area)** : 바깥여백 경계(Margin Edge)가 감싼 영역으로, 테두리 영역을 확장해 요소와 인근 요소사이의 빈 공간까지 포함하도록 한다.

<br/>

#### box-sizing

box-sizing 속성을 사용하면, `width` 와 `height` 이 컨텐츠 영역 기준인지, 테두리 영역 기준인지 정할 수 있다.

* `box-sizing: content-box` : 기본값이며 컨텐츠 영역 기준이다. 즉, 안쪽여백 영역부터 포함하지 않는다.
* `box-sizing: border-box` : 테두리 영역 기준이며 바깥여백 영역부터 포함하지 않는다.

<br/>

#### 참고

* [MDN, CSS 기본 박스 모델 입문](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)
* [W3, CSS Box Model Module Level 3](https://www.w3.org/TR/css-box-3/)
* [BrainJar, CSS Positioning](http://www.brainjar.com/css/positioning/)

***

### 마진겹침 현상

마진겹침(Margin-Collpasing)이란 블록 레벨 엘리먼트(Block-level element)에 한해서 발생하는 현상으로, 좌우 방향으로는 적용되지 않고 **오로지 수직방향** 으로 적용된다. 2개의 마진이 겹칠 때 더 큰 마진으로 덮어 씌우는 방식이며 하나의 마진이 음수일 경우 더하는 방식을 취한다.

<br/>

#### 3가지 경우

마진겹침은 블록 레벨 엘리먼트라는 가정하에 3가지 경우에 한해서 발생한다.

1. **인접한 엘리먼트**

```html
<div class="element1"></div>
<div class="element2"></div>
```

```css
div {
  width: 100px;
  height: 100px;
  background-color: red;
}
.element1 { margin-bottom: 20px; }
.element2 { margin-top: 40px; }
```

<p align="center">
<img src="/img/margin-collapsing1.png"/>
</p>

두번째 엘리먼트의 위쪽 마진이 더 크기 때문에 둘 사이의 간격은 40px이 된다.

2. **부모와 처음/마지막 자식 사이에서**

```html
<div class="parent">
  <div class="child">
  </div>
</div>
```

```css
div {
  width: 100px;
  height: 100px;
}
.parent {
  background-color: red;
  margin-top: 20px; 
}
.child { 
  background-color: blue;
  margin-top: 20px; 
}
```

<p align="center">
<img src="/img/margin-collapsing2.png"/>
</p>

둘의 마진이 같기 때문에 20px이 된다. 이를 해결하기 위해선 **부모에 inline 컨텐츠, border, padding** 을 줘서 경계를 구분시키면 된다.

```css
.parent {
  background-color: red;
  border: 1px solid red;
  margin-top: 20px;
}
```

<p align="center">
<img src="/img/margin-collapsing3.png"/>
</p>

3. **빈 엘리먼트**

```html
<div class="empty"></div>
<div class="element"></div>
```

```css
.empty { margin-top: 50px; }
.element {
  width: 100px;
  height: 100px;
  background-color: red;
  margin-top: 100px;
}
```
<p align="center">
<img src="/img/margin-collapsing4.png"/>
</p>



높이가 없는 빈 엘리먼트가 인접해있을 때도 마진겹침이 발생하여 위쪽 마진은 100px이 된다. 이를 해결하기 위해선 **빈 엘리먼트에 height, min-height, padding, border나 inline 컨텐츠** 를 줘서 경계를 구분시키면 된다.

```css
.empty {
  border: 1px solid red;
  margin-top: 50px;
}
```
<p align="center">
<img src="/img/margin-collapsing5.png"/>
</p>


<br/>

#### 예외 대상

`position: absolute` , `float: left` , `display: flex` 등 다양한 상황에 마진겹침이 발생하지 않지만, 그냥 **새로운 BFC(Block Formatting Context)를 생성하는 조건이 마진겹침을 발생시키지 않는다** 고 알아두면 된다.

<br/>

#### 참고

* [MDN, 마진 상쇄 정복](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)
* [Velog, CSS 마진 상쇄(Margin-collapsing) 원리 완벽 이해](https://velog.io/@raram2/CSS-%EB%A7%88%EC%A7%84-%EC%83%81%EC%87%84Margin-collapsing-%EC%9B%90%EB%A6%AC-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4#%EB%A7%88%EC%A7%84-%EC%83%81%EC%87%84-%EA%B7%9C%EC%B9%99-%EC%98%88%EC%99%B8)
* [Youtube, [ㅁ] 마진병합 margin collapsing | 코딩가나다 | 빔캠프](https://www.youtube.com/watch?v=c19Mjg-ivxc)
* [생활코딩, 마진겹침 현상](https://opentutorials.org/course/2418/13464)


***

# Block Formatting Context

MDN의 정의를 보면,

> *BFC는 웹페이지의 블록 레벨 요소를 렌더링하는데 사용되는 CSS의 비주얼 서식 모델 중 하나이다.*

즉, 블록 레벨 요소가 포함되는 **서식모델** 이다.

<br/>

#### 생성 조건

BFC가 생성되기 위해선 다음과 같은 조건 중 하나를 만족해야 한다.

* 루트 혹은 이를 포함하는 요소

* float가 none이 아니면서 clear 되지 않은 경우
* position이 absolute / fixed
* display가 inline-block / tabel-cell / tabel-caption
* overflow가 visible이 아님
* display가 flex / inline-flex

위 조건에 따른 예제를 보면, 아래 요소들은 모두 BFC를 생성한다.

```html
<div class="float"></div>
<div class="position"></div>
<div class="display"></div>
<div class="overflow"></div>
<div class="flex"></div>
```

```css
.float { float: left; }
.position { position: absolute; }
.display { display: inline-block; }
.overflow { overflow: hidden; }
.flex { display: flex; }
```

<br/>

#### 활용

1. **마진겹침 제거하기**

```html
<div class="bfc">
  <p>element</p>
  <p>element</p>
  <div class="new-bfc">
    <p>element</p>
  </div>
</div>
```

```css
div {
  background-color: blue;
}
p {
  margin: 10px 0;
  background-color: green;
}
.bfc {
  overflow: hidden;
}
.new-bfc {
  overflow: hidden;
}
```

[Codepen](https://codepen.io/BaeHaram/pen/YzXGZBd)

`p` 태그 사이에 10px의 마진이 마진겹침으로 인해 20px이 아닌 10px이 되었다. 이 때 새로운 BFC를 생성해서 `p` 를 집어넣으면 마진겹침을 해결할 수 있다.

2. **float된 요소들 포함하기**

```html
<div class="container">
  <div class="float"></div>
</div>
```

```css
.container {
  border: 3px solid red;
  overflow: hidden;
}

.float {
  float: left;
  width: 500px;
  height: 300px;
  background-color: yellow;
}
```

보통 float된 요소를 포함하기 위해선 `.clearfix` 핵을 사용하지만 새로운 BFC를 생성함으로도 해결할 수 있다.

3. **float된 요소를 감싸는 텍스트를 분리하기**

```html
 <div class="container">
  <div class="box"></div>
  <p>많은 양의 텍스트....</p>
</div>
```

```css
.container {
  border: 3px solid red;
  overflow: hidden;
}
.box {
  float: left;
  width: 100px;
  height: 100px;
  background-color: green;
}

p {
  overflow: hidden;
}
```

`p` 태그로 새로운 BFC를 생성하여 텍스트가 float 된 요소를 감싸지 않도록 하였다.

<br/>

#### 참고

* [Understanding Block Formatting Contexts in CSS](https://www.sitepoint.com/understanding-block-formatting-contexts-in-css/)
* [MDN, Block formatting context](https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context)


***


### z-index의 동작방식

#### z-index와 쌓임 맥락

z-index를 이해하기 위해선 먼저, 쌓임 맥락(Stacking Context)의 개념을 이해해야 한다. **쌓임 맥락이란, HTML 요소들에 사용자를 바라보는 기준으로 가상의 z축을 생성하여 3차원 개념으로 보는 것** 이다. 따라서, 쌓임 맥락을 형성한다는 것은 자신만의 3차원 공간을 형성하는 것이며 그 요소들의 우선순위를 z-index가 정하게 되는 원리이다.

<img src="/img/stacking context.png"/>

[그림 출처](https://tympanus.net/codrops/css_reference/z-index/)

위 그림을 보면, 각 요소들이 사용자를 바라보는 z축 상에서 z-index에 따라 **"쌓이는"** 것을 볼 수 있다. 바로 이것이 쌓임 맥락의 개념이며 쌓임 맥락을 형성하는 조건은 [꽤 많다.](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context) 모든 걸 기억하면 좋겠지만 중요한 것들을 기억해두록 하자.

* **position이 relative/absolute이면서 z-index가 auto가 아닌 요소**
* **position이 fixed/sticky인 요소**
* **opacity가 1보다 작은 요소**
* **transform이 none이 아닌 요소**

가장 많이 쓰는 속성들을 기준으로 조금만 나열해보면 위와 같은데 이 정도는 암기하도록 하자.

쌓임 맥락은 다음 특징들을 갖는다.

* 쌓임 맥락은 **다른 쌓임 맥락을 포함할 수 있다.**
* 쌓임 맥락에서 쌓임을 고려하는 것은 **오직 자식 요소들에 대해서** 만이다.

즉, 2개의 쌓임 맥락을 형성하는 요소가 있다고 했을 때, 각각은 독립적인 쌓인 맥락을 갖으며 그 안의 자식 요소들은 부모 안에서의 쌓임만 고려된다는 것이다. 

<br/>

#### 우선순위

쌓임 맥락을 형성하는 요소가 아무것도 없다고 하면 다음 우선순위로 쌓이게 된다.

<img src="/img/default stacking order.png"/>


[그림 출처](https://tympanus.net/codrops/css_reference/z-index/)

만약, 동일한 성격의 요소라면 마크업 순서로 쌓임이 결정된다. 즉, 아래와 같은 경우,

```html
<div>A</div>
<div>B</div>
<div>C</div>
```

```css
div {
  position: absolute;
}
```

여기서 쌓임 맥락을 형성하는 것으로 착각할 수도 있는데 `position: absolute` 라고 해도 `z-index: auto` 이면 쌓임 맥락을 형성하지 않는다. 어쨌든 여기선 동일한 성격의 요소들이기 때문에 마크업 순서인 A,B,C 순으로 쌓임이 형성된다. ([Codepen](https://codepen.io/BaeHaram/pen/ExjmvRY) 에서 확인할 수 있다.)

쌓임 맥락을 형성한다면, `z-index` 값을 설정할 수 있는 `position: static` 이 아닌 요소들의 경우는 동일한 마크업 레벨에서, `z-index` 값으로 우선순위를 결정한다. `z-index` 값을 설정할 수 없는 요소라면 마크업 순서로 결정한다. 여기선 `position: static` 이 아니고 `z-index: auto` 가 아닌 요소를 보자.

```html
<div>1</div>
<div>2</div>
<div>3
  <div>4</div>
  <div>5</div>
  <div>6</div>
</div>
```

마크업이 위와 같이 되어 있을 때, `z-index` 에 따라 다음과 같이 쌓인다.

<img src="/img/z-index stacking order.png"/>

[그림출처](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

* div1과 div2, div3는 같은 레벨에 있으므로 z-index에 따라 쌓이기 때문에 div2 > div3 > div1 순으로 쌓인다.
* div4와 div5, div6은 div3안에 있으므로 **그 안에서** z-index에 따라 쌓인다.
* 즉, div3 안의 요소들의 z-index가 div1,div2 보다 커도 영향을 주지 않는다.
* 결론적으로, div5 > div6 > div4 순으로 쌓인다.

<br/>

#### 참고

* [CSSReference, z-index](https://tympanus.net/codrops/css_reference/z-index/)
* [MDN, Stacking Context](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)
* [MDN, z-index](https://developer.mozilla.org/ko/docs/Web/CSS/z-index)


***

### block vs inline vs inline-block

| 특징                         | block              | inline             | inline-block       |
| ---------------------------- | ------------------ | ------------------ | ------------------ |
| 상하 마진/패딩               | :white_check_mark: | :x:                | :white_check_mark: |
| 좌우 마진/패딩               | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| 요소 사이 줄바꿈             | :white_check_mark: | :x:                | :x:                |
| 기본너비가 부모너비          | :white_check_mark: | :x:                | :x:                |
| 요소 사이 공백               | :x:                | :white_check_mark: | :white_check_mark: |
| 너비와 높이 명시했을 때 적용 | :white_check_mark: | :x:                | :white_check_mark: |

[Codepen](https://codepen.io/BaeHaram/pen/poJaRyK) 을 통해서 확인해보면 위의 특징들을 좀 더 명확하게 알 수 있으니 해보도록 하자.

<br/>

#### 참고

* [Gist, css-display.md](https://gist.github.com/Asheq/1ef5ec77b8e89c2c9da89d2b7a1cf8cb)
* [Stackoverflow, CSS display: inline vs inline-block](https://stackoverflow.com/questions/9189810/css-display-inline-vs-inline-block)


***

# Reset.css vs Normalize.css

크롬, 사파리, IE 등 각 브라우저마다 HTML 요소의 기본 스타일을 가지고 있다. 따라서, CSS로 스타일링을 적용할 때 이러한 특징이 동일한 스타일 적용을 방해하기 때문에 이를 해결하기 위해서 나온 스타일 초기화 기법들이다.

<br/>

#### Reset.css

**모든 브라우저 내장 스타일을 없애는 기법** 으로, 그 어떤 스타일도 없는 상태에서 스타일링을 시작한다. `h1` ~ `h6` , `p` , `em` 등 각 태그에 적용되는 스타일을 모두 없앤다. 가장 유명한 스타일은 Eric Mayer의 Reset CSS이며 이를 [깃헙](https://github.com/shannonmoeller/reset-css) 에서 유지하고 있다. 보통, 초기화를 한 후 각자의 방식에 맞게 변형해서 사용한다.

<br/>

#### Normalize.css

**모든 브라우저의 스타일을 동일하게 하는 기법** 으로, 스타일을 없애는 Reset.css와는 다르게 기존 스타일을 유지하되 브라우저들의 다른 스타일을 고치는 방식이다. 가장 유명한 스타일은 necolas의 normalize.css이며 이를 [깃헙](https://github.com/necolas/normalize.css/) 에서 유지하고 있다. 실제 코드의 주석을 보면 각 요소를 스타일링 하는 이유에 대해 설명하고 있다.

<br/>

#### 차이점과 선택

* Reset.css의 경우, 모든 것을 초기화하기 때문에 스타일을 처음부터 적용해 나가는 것이 힘들 수 있고 브라우저의 버그를 고치는 것이 아니기 때문에 각 브라우저마다 다른 버그를 발생시킬 수 있다. 하지만, 아예 초기화를 하는 것이기 때문에 업데이트가 필요없다.
* Normalize.css의 경우, 브라우저가 업데이트 되어서 새로운 내장 스타일이 적용될 때마다 각 브라우저의 다른 점을 파악하여 스타일을 업데이트해야 하기 때문에 끊임없는 버전 업데이트가 있어야 최신 스타일을 유지할 수 있다. 하지만, 브라우저의 버그를 고치기 때문에 버그가 발생할 걱정을 덜고 기본 스타일을 어느정도 유지하기 때문에 스타일링에 힘을 덜 들일 수 있다.

둘 다 장단점이 있기 때문에 어떤 것이 더 낫다라고는 할 수 없고, 상황에 맞게 적용해서 사용하면 된다고 생각한다.

<br/>

#### 참고

* [Stackoverflow, CSS reset - What exactly does it do?](https://stackoverflow.com/questions/11578819/css-reset-what-exactly-does-it-do)
* [Stackoverflow, What is the difference between Normalize.css and Reset CSS?](https://stackoverflow.com/questions/6887336/what-is-the-difference-between-normalize-css-and-reset-css)
* [reset.css 와 Normalize.css](https://sapjil.net/resetcss-normalizecss/)
* [About normalize.css](http://nicolasgallagher.com/about-normalize-css/)


***

### 그리드 시스템

그리드 레이아웃을 구현하기 위해 설계한 시스템으로 너비 960px 혹은 1200px 기준으로 정해놓은 시스템들이 있다. 열(Column)의 개수에 따라 12단/16단/24단 그리드라고 부르기도 한다. 그리드 레이아웃을 구현하는 방법은 여러가지가 있으며 여기선 12단 그리드로 진행한다.

> float, flexbox, grid의 기본개념과 문법들에 대해 알고있어야 한다.

<br/>

#### Float grid system

flexbox나 grid가 나오기 이전에 사용하던 방식으로 float 속성을 사용하여 구현한다. float된 높이를 잡기 위해 clearfix 핵을 사용해야 하며 각 너비를 열/행의 간격에 맞게 계산해주어야 한다.

**[Codepen](https://codepen.io/BaeHaram/pen/XWbYpwx) 에서 꼭 직접 해보자!**

<img src="/img/float-grid.png"/>

```html
<h1>Float grid system</h1>
<div class="container">
  <div class="row">
    <div class="col col-3-12"></div>
    <div class="col col-3-12"></div>
    <div class="col col-3-12"></div>
    <div class="col col-3-12"></div>
  </div>
  <div class="row">
    <div class="col col-6-12"></div>
    <div class="col col-6-12"></div>
  </div>
  <div class="row">
    <div class="col col-4-12"></div>
    <div class="col col-4-12"></div>
    <div class="col col-4-12"></div>
  </div>
</div>
```

```css
:root {
  --gutter: 10px;
}
.container { 
  width: 500px;
  border: 3px solid red;
  padding: 1rem;
}
.row::after {
  content: '';
  display: block;
  clear: both;
}

.row + .row {
  margin-top: var(--gutter);
}

.col {
  height: 100px;
  background-color: orange;
  float: left;
  margin-right: var(--gutter);
}

.col:last-child { margin-right: 0; }

.col-1-12 { width: calc(100%/(12/1) - var(--gutter)*11/12); }
.col-2-12 { width: calc(100%/(12/2) - var(--gutter)*10/12); }
.col-3-12 { width: calc(100%/(12/3) - var(--gutter)*9/12); }
.col-4-12 { width: calc(100%/(12/4) - var(--gutter)*8/12); }
.col-5-12 { width: calc(100%/(12/5) - var(--gutter)*7/12); }
.col-6-12 { width: calc(100%/(12/6) - var(--gutter)*6/12); }
.col-7-12 { width: calc(100%/(12/7) - var(--gutter)*5/12); }
.col-8-12 { width: calc(100%/(12/8) - var(--gutter)*4/12); }
.col-9-12 { width: calc(100%/(12/9) - var(--gutter)*3/12); }
.col-10-12 { width: calc(100%/(12/10) - var(--gutter)*2/12); }
.col-11-12 { width: calc(100%/(12/11) - var(--gutter)*1/12); }
.col-12-12 { width: calc(100%/(12/12) - var(--gutter)*0/12); }


.col:nth-child(even) {
  background-color: blue;
}
```

`row::after` 로 clearfix 핵을 적용하였고 행의 `margin` 으로 수직 간격을, 열의 `margin` 으로 수평 간격을 지정하였다. 또한 12단 그리드이기 때문에 `.col-x-12` 가 뜻하는 것은 12단 중에 x개를 차지하는 너비를 말한다. 간격에 유동적으로 적용하기 위해 간격변수인 `--gutter` 기준으로 계산하였다.

<br/>

#### Flexbox grid system

**[Codepen](https://codepen.io/BaeHaram/pen/oNXyZvv) 에서 꼭 직접 해보자!**

<img src="/img/flexbox-grid.png"/>

대부분 float를 사용한 방식과 동일하되, clearfix 핵을 없애고 flex를 적용한 것이다.

```css
/*.row::after {
  content: '';
  display: block;
  clear: both;
}*/
.row {
  display: flex;
}
```

<br/>

#### Grid layout grid system

grid를 사용한 경우로, float/flexbox와는 완전히 다르다. 너비를 계산하지도 않고 `row` 나 `column` 을 따로 지정하지 않는다. 단순하게 grid 관련된 속성을 사용해서 열/행의 너비 및 높이와 각 항목의 비율을 지정한다.

**[Codepen](https://codepen.io/BaeHaram/pen/JjdZWoQ) 에서 꼭 직접 해보자!**

<img src="/img/grid-grid.png"/>

```html
<h1>Grid layout grid system</h1>
<div class="container">
  <div class="grid">
    <div class="item item-1">1</div>
    <div class="item item-2">2</div>
    <div class="item item-3">3</div>
    <div class="item item-4">4</div>
    <div class="item item-5">5</div>
    <div class="item item-6">6</div>
    <div class="item item-7">7</div>
    <div class="item item-8">8</div>
  </div>
</div>
```

```css
.container { 
  width: 500px;
  border: 3px solid red;
  padding: 1rem;
}

.grid {
  display: grid;
  height: 500px;
  grid-template-rows: repeat(4,1fr);
  grid-template-columns: repeat(3,1fr);
  grid-row-gap: 10px;
  grid-column-gap: 10px;
}

.item {
  background-color: crimson;
  text-align: center;
  font-size: 2rem;
  font-weight: bold;
}

.item:nth-child(even) {
  background-color: yellow;
}

.item-3 {
  grid-row: 1/3;
  grid-column-start: 3;
}

.item-6 {
  grid-column: 1/4;
  grid-row: 3/4;
}
```

<br/>

#### 참고

* [Sass로 12단 그리드 시스템 만드는 법](https://medium.com/fluosoup/sass로-12단-그리드-시스템-만드는-법-d2c7cf54c36)
* [CSS Grid 완벽 가이드](https://heropy.blog/2019/08/17/css-grid/)

*** 


### CSS Position


#### https://mingeesuh.tistory.com/3
