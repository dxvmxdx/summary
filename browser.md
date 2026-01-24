# Contents

1. [DOM (document object model)](#1-dom)  
   1-1. [조작](#1-1-조작)  
   1-2. [Critical Rendering Path](#1-2-critical-rendering-path)
2. [Size](#2-size)
3. [Event](#3-event)  
   3-1. [종류](#3-1-종류)  
   3-2. [이벤트 객체](#3-2-이벤트-객체)  
   3-3. [이벤트 버블링](#3-3-이벤트-버블링)

<br>
<br>
<br>
<br>
<br>

# 1. DOM

브라우저가 HTML 문서를 로드한 후 파싱하여 생성하는 모델

<br>

## 1-1. 조작

```html
<section>
  <h1>Title</h1>
  <h3>This is content.</h3>
</section>
```

```js
const section = document.querySelector('section');
const h3 = document.querySelector('h3');

const h2 = document.createElement('h2');
section.insertBefore(h2, h3); // section 내 h3 전에 추가

const category = document.createElement('div');
category.classList.add('category');
category.addEventListener('click', (e) => {
  if (e.target.matches('.active')) {
    // ...
  }
});
section.appendChild(category);
```

<br>
<br>
<br>

## 1-2. Critical rendering path

브라우저 렌더링 과정.  
이를 최적화하면 렌더링 성능을 향상시킬 수 있다.

"DOM, CSSOM -> Render Tree -> Layout -> Paint"

<br>

### Understading CRP

클라이언트(브라우저)가 HTTP 요청을 보내면 서버는 HTML을 포함한 응답을 전송한다.  
브라우저는 응답받은 HTML을 파싱해 DOM 트리로 변환한다.

> 💡 불필요한 노드 생성 NO!

브라우저는 스타일시트, 스크립트 또는 포함된 이미지 참조 같은 외부 리소스 링크를 찾을 때마다 요청을 보냅니다.
불러온 리소스를 처리할 때까지 나머지 HTML 파싱이 멈춘다.  
브라우저는 CSS 객체 모델을 구축할 때까지 HTML을 파싱한다.

DOM과 CSSOM이 완료되면 브라우저는 렌더 트리를 생성합니다. 그리고 보여지는 모든 콘텐츠의 스타일을 계산합니다.

렌더 트리가 완성된 후, 레이아웃이 시작되고 모든 렌더 트리 요소에 대한 위치와 크기가 정의됩니다.

> 렌더 트리가 수정되면(노드 추가, 콘텐츠 변경, 스타일 업데이트, ...) Layout 발생!  
> 💡 일괄적으로 업데이트하고 박스모델 속성을 animating하지 않는다. [css 속성 확인 참고 사이트](https://www.lmame-geek.com/css-triggers/)

레이아웃이 완료되면 페이지는 화면에 'Paint'됩니다. 처음 로드시, 전체 화면을 그린다. 그 후에는 브라우저가 필요한 최소 영역만을 다시 그리도록 최적화되어 있기 때문에 영향을 받는 영역만 화면에 다시 그린다.

<br>
<br>
<br>
<br>
<br>

# 2. Size

```js
// 사용자 모니터 크기
window.screen.width;
window.screen.height;

// 브라우저 전체 크기
window.outerWidth;
window.outerHeight;

// 브라우저 document 크기 (스크롤 포함)
window.innerWidth;
window.innerHeight;

// 브라우저 document 크기 (스크롤 제외)
document.documentElement.clientWidth;
document.documentElement.clientHeight;
```

<br>

### Element.getBoundingClientRect()

요소의 크기 및 위치 정보를 제공하는 객체 반환

<img src="https://developer.mozilla.org/ko/docs/Web/API/Element/getBoundingClientRect/element-box-diagram.png" width=500 />

<br>
<br>
<br>
<br>
<br>

# 3. Event

## 3-1. 종류

- keyup vs keydown  
  한국어 같은 언어일 경우 keyup을 쓰는 것이 좋다

- DOMContentLoaded  
  html 문서가 로드되면 이벤트 발생

- load  
  리소스(스타일시트, 이미지, 폰트, ...)가 모두 로드되면 이벤트 발생

- beforeunload  
  페이지를 나갈 때 이벤트 발생  
  (사용자에게 실제로 페이지를 떠날 것인지 묻는 확인 대화 상자를 표시할 수 있다)

<br>
<br>
<br>

## 3-2. 이벤트 객체

### target vs currentTarget

`event.target`: 이벤트가 발생한 요소  
`event.currentTarget`: 이벤트가 바인딩된 요소

<br>
<br>
<br>

## 3-3. 이벤트 버블링

이벤트가 발생하면, 발생한 요소에 바인딩된 핸들러가 동작하고 이어서 부모 요소의 핸들러가 동작한다.(**같은 이벤트 종류에 한하여**)  
가장 최상단의 조상 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작한다.

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

```js
const container = document.querySelector('#container');
const button = document.querySelector('button');
const output = document.querySelector('#output');

function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

document.body.addEventListener('click', handleClick);
container.addEventListener('click', handleClick);
button.addEventListener('click', handleClick);

// 버튼 클릭 시
// Expected output:
// You clicked on a BUTTON element
// You clicked on a DIV element
// You clicked on a BODY element
```

<br>

한 요소에 이벤트 핸들러를 여러 개 추가하면 등록한 순서대로 동작한다.

```js
button.addEventListener('click', handleClick1); // 1
button.addEventListener('click', handleClick2); // 2
```
