# Contents

1. [DOM (document object model)](#1-dom)  
   1-1. [조작](#1-1-조작)  
   1-2. [Critical Rendering Path](#1-2-critical-rendering-path)
2. [Size](#2-size)

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
