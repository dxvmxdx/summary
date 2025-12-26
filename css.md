# Contents

1. [box model](#1-box-model)
2. [속성](#2-속성)  
   2-1. [position](#2-1-position)

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
