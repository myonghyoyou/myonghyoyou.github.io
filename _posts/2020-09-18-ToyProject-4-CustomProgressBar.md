---
title: Toy Project (4) Custom Progress Bar
tags: ToyProject
article_header:
  type: cover
  image: 
      src: 
---

데이터 로딩 시 흔하게 볼 수 있는 ProgressBar를 손수 만들어 보았다.

ProgressBar는 이미 BootStrap 에서도 제공하고 있고 일전에 BootStrap의 라이브러리 내부를 참고하여 사내 라이브러리에 맞게 수정하여 포함시킨 적이 있다. \
하지만 어떤 자료도 참고하지 않고 스스로 만들고 싶다는 생각이 들어서 시작하게 된 프로젝트다. \
한 번 만드는 걸로 끝나지 않고, 지웠다가 다시 만들고 하기를 4-5번 반복했다. 그렇게 하니 내부적으로 쓰인 기능들이 손과 머리에 익는 느낌이다.

#### 학습 내용

  1. flex를 이용한 div 요소의 중앙 정렬 방법.
  2. display에 flex를 적용시킨 div 내의 요소 정렬은 flex-flow를 통해 쉽게 할 수 있다. (수직 정렬, 수평 정렬)
  3. display에 flex를 적용시킨 요소는 기본적으로 block으로 지정된다. inline으로 지정하고 싶다면 inline-flex를 적용시키면 된다.
  4. getAttribute 메소드를 통해 Dom 요소에 직접적으로 타이핑된 속성값을 얻어올 수 있다.
  5. setInterval과 clearInterval의 활용 -> 재차 사용해봄으로써 익숙해질 필요가 있음.

<div>{%- include extensions/codepen.html user='myonghyoyou' hash='LYNJaVO' default_tab='html,result' -%}</div>
