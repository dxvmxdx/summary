# Contents

1. [box model](#1-box-model)
2. [속성](#2-속성)  
   2-1. [position](#2-1-position)  
   2-2. [flexbox](#2-2-flexbox)  
   2-3. [grid](#2-3-grid)
3. [정렬](#3-정렬)

<br>
<br>
<br>
<br>
<br>

# 1. box model

### border와 outline 차이

- border  
  요소 테두리
- outline  
  요소 외곽선 (`border` 바깥에 그려지는 선)

<br>

### box-sizing

요소의 너비와 높이를 계산하는 방법을 지정

- `content-box`  
  기본값. 요소의 너비와 높이가 `content` 자체
- `border-box`  
  요소의 너비와 높이를 `border`까지 포함

<br>
<br>
<br>
<br>
<br>

# 2. 속성

## 2-1. position

- relative  
  요소를 일반적인 문서 흐름에 따라 배치.  
  자기 자신을 기준으로 이동.

- absolute  
  요소를 일반적인 문서 흐름에서 제거.  
  position 속성값이 `static`이 아닌 가장 가까운 부모를 기준으로 이동.

- fixed  
  요소를 일반적인 문서 흐름에서 제거.  
  브라우저를 기준으로 이동.

- sticky  
  요소를 일반적인 문서 흐름에 따라 배치.  
  스크롤 동작이 가능한 가장 가까운 부모에 달라붙으며, 요소가 사라질 때 어디에 붙을지 지정해줘야 한다.

<br>
<br>
<br>

## 2-2. flexbox

[참고 사이트](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

<br>

> ⚠️ flex item들의 display 속성값은 `block`이 된다.

<br>

### container에 지정하는 속성

- `display`  
  _flex, inline-flex_

- `flex-direction`  
  메인축을 지정.  
  _row, row-reverse, column, column-reverse_

- `flex-wrap`  
  item이 영역을 벗어나면 강제로 한 줄에 배치할 것인지 여러 행으로 배치할 것인지 지정.  
  _nowrap, wrap, wrap-reverse_

- `flex-flow`  
  `flex-direction` `flex-wrap` 단축속성

- `justify-content`  
  메인축에서 item을 어떻게 배치할 것인지 지정.

- `align-items`  
  교차축에서 item을 어떻게 배치할 것인지 지정.

- `align-content`  
  교차축에서 item을 어떻게 배치할 것인지 지정.  
  이 속성은 flex-wrap 속성값이 `wrap` 또는 `wrap-reverse` 일 경우, 즉 여러 줄일 때 적용된다.

- `gap`

<br>

### item에 지정하는 속성

- `order`  
  기본값 0

- `flex-grow`  
  기본값 0  
  container 내부에서 할당 가능한 공간의 정도를 지정.  
  item 모두 동일한 값을 갖는다면, container 내에서 동일한 공간을 할당받는다.

- `flex-shrink`  
  기본값 1  
  item의 크기가 container 크기보다 클 때, 설정된 값에 따라 축소된다.  
  값을 0으로 설정하면 item 요소 크기가 그대로 유지된다.

- `flex-basis`  
  기본값 auto  
  item의 초기 크기 지정.

- `flex`  
  `flex-grow` `flex-shrink` `flex-basis` 단축속성

- `align-self`  
  개별 item의 정렬 지정.  
  _auto, flex-start, flex-end, center, baseline, stretch_

<br>
<br>
<br>

## 2-3. grid

[참고 사이트](https://css-tricks.com/css-grid-layout-guide/)

<br>

### container에 지정하는 속성

- `display`  
  _grid, inline-grid_

- `grid-template-columns`  
  열의 크기를 정의

- `grid-template-rows`  
  행의 크기를 정의

  ```css
  .container {
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 100px);
  }
  ```

- `grid-template-areas`  
  지정된 그리드 영역 이름(`grid-area`)을 참조해 그리드 템플릿을 생성한다.

  ```css
  .container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 100px);
    grid-template-areas:
      'header header header'
      'main main aside'
      'footer footer footer';
  }
  header {
    grid-area: header;
  }
  main {
    grid-area: main;
  }
  aside {
    grid-area: aside;
  }
  footer {
    grid-area: footer;
  }
  ```

- `justify-content`  
  그리드 컨텐츠 수평 정렬.

- `align-content`  
  그리드 컨텐츠 수직 정렬.

- `justify-items`  
  그리드 아이템들 수평 정렬.  
  아이템의 가로 너비가 자신이 속한 그리드 열의 크기보다 작아야 적용된다.

- `align-items`  
  그리드 아이템들 수직 정렬.  
  아이템의 세로 너비가 자신이 속한 그리드 행의 크기보다 작아야 적용된다.

- `gap`

  ```css
  .container {
    gap: <row-gap> <column-gap>;
  }
  ```

<br>

### item에 지정하는 속성

- `grid-row-start`, `grid-row-end`, `grid-column-start`, `grid-column-end`  
  아이템의 시작위치와 끝위치를 지정할 수 있다.

- `grid-row`  
  `grid-row-start` / `grid-row-end` 단축속성

- `grid-column`  
  `grid-column-start` / `grid-column-end` 단축속성

- `grid-area`  
  `grid-row-start` / `grid-column-start` / `grid-row-end` / `grid-column-end` 단축속성  
  혹은, `grid-template-areas`가 참조할 영역 이름을 설정할 수도 있다.

- `justify-self`  
  아이템 수평 정렬.  
  아이템의 가로 너비가 자신이 속한 그리드 열의 크기보다 작아야 적용된다.

- `align-self`  
  아이템 수직 정렬.  
  아이템의 세로 너비가 자신이 속한 그리드 행의 크기보다 작아야 적용된다.

- `order`  
  기본값 0

<br>
<br>
<br>
<br>
<br>

# 3. 정렬

### block element

```css
.box {
  margin: auto;
}
```

<br>

### inline element

```css
/* 부모 요소(block) */
.parent {
  text-align: center;
}

/* 수직 정렬 */
.child {
  height: 100%;
  line-height: 100px; /* 부모 요소 height */
}
```

<br>

### 중앙 정렬

```css
/* flex */
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* transform */
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```
