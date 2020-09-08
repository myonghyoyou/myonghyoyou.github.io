---
title: Toy Project (1) Animated Navigation Toggle Menu
tags: ToyProject
article_header:
  type: cover
  image: 
      src: 
---

다음 문제인 Defibrillators 이다.

출력되는 수많은 정보들로부터 경위도를 수집한 뒤, 기존의 경위도와 비교하여 가장 가까운 장소의 이름을 출력하는 문제이다.  
String의 split을 이용하여 위치 정보를 이름, 주소, 경도, 위도로 구분된 배열로 만든 뒤에 이름을 placeArray라는 배열에 넣는다. 그 다음 거리를 계산하는 수식을 거친 뒤에 거리 정보를 distArray라는 배열에 넣은 다음 가장 적은 값을 가진 value의 인덱스를 구해서 placeArray로부터 꺼내는 방식으로 해결했다.

```
<div>{%- include extensions/codepen.html user='myonghyoyou' hash='MWyQjgY' default_tab='html,result' -%}</div>
```
{%- include extensions/codepen.html user='myonghyoyou' hash='MWyQjgY' default_tab='html,result' -%}
