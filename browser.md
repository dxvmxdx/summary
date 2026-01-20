# Contents

1. [DOM (document object model)](#1-dom)  
   1-1. [조작](#1-1-조작)

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
