# Contents

1. [CDN 연결](#1-cdn-연결)
2. [method](#2-method)  
   2-1. [탐색 메서드](#2-1-탐색-메서드)  
   2-2. [.css()](#2-2-css)  
   2-3. [클래스](#2-3-클래스)  
   2-4. [애니메이션 effect](#2-4-애니메이션-effect)  
   2-5. [.text(), html(), val()](#2-5-text-html-val)  
   2-6. [.attr()](#2-6-attr)  
   2-7. [요소 삽입과 제거](#2-7-요소-삽입과-제거)

<br>
<br>
<br>
<br>
<br>

# 1. CDN 연결

```html
<!doctype html>
<html lang="ko">
  <head>
    <script
      defer
      src="https://code.jquery.com/jquery-4.0.0.min.js"
      integrity="sha256-OaVG6prZf4v69dPg6PhVattBXkcOWQB62pdZ3ORyrao="
      crossorigin="anonymous"
    ></script>
    <script defer src="script.js"></script>
  </head>
</html>
```

<br>
<br>
<br>
<br>
<br>

# 2. method

## 2-1. 탐색 메서드

### .parents('선택자'), .parent(), .closest('선택자')

```js
$(target).parents('li'); // target 요소의 모든 조상 중 li 요소
$(target).parent(); // target 요소의 부모 요소
```

<br>

### .children('선택자'), .find('선택자')

```js
$(target).children(); // target 요소의 모든 자식 요소
$(target).find('a'); // target 요소의 모든 자손 중 a 요소
```

<br>

### .siblings('선택자')

```js
$(target).siblings(); // target 요소의 모든 형제 요소
```

<br>

### .prev(), .prevAll(), .next(), .nextAll()

```js
$(target).prev(); // target 요소의 이전 형제 요소
$(target).prevAll(); // target 요소의 모든 이전 형제 요소
$(target).next(); // target 요소의 다음 형제 요소
$(target).nextAll(); // target 요소의 모든 다음 형제 요소
```

<br>

### .eq(index), .not('선택자')

```js
$('li').eq(0); // li 요소 중 첫번째 요소
$(target).not($(this)); // target 요소 중 $(this) 요소 제외한 모든 요소
```

<br>
<br>
<br>

## 2-2. .css()

인라인 스타일로 추가

```js
$('body').css({
  'background-image': '',
});
```

<br>
<br>
<br>

## 2-3. 클래스

```js
$(target).addClass('className');
$(target).removeClass('className');
$(target).toggleClass('className');
```

<br>
<br>
<br>

## 2-4. 애니메이션 effect

```js
$(target).show();
$(target).hide();
$(target).toggle();

$(target).fadeIn();
$(target).fadeOut();
$(target).fadeToggle();

$(target).slideDown();
$(target).slideUp();
$(target).slideToggle();
```

> ✅
> `$(target).stop()`: target 요소의 실행 중인 애니메이션을 즉시 중단시킨다.  
> ex. `$(this).stop().slideToggle();`

<br>
<br>
<br>

## 2-5. .text(), .html(), .val()

```js
$(target).text();
$(target).html();
$(target).val(); // target 요소의 value 속성값 반환
```

<br>
<br>
<br>

## 2-6. .attr()

```js
$(target).attr('data-link'); // target 요소의 `data-link` 속성값을 반환
```

<br>
<br>
<br>

## 2-7. 요소 삽입과 제거

```js
$('.container').append(html); // .container 요소 내부의 맨 뒤에 html 추가
$('.container').prepend(html); //  .container 요소 내부의 맨 앞에 html 추가
$(target).after(html); // target 요소 바로 뒤에 html 추가
$(target).before(html); // target 요소 바로 앞에 html 추가

$(target).remove(); // target 요소 삭제
```
