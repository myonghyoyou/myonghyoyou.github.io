---
title: CodingGames (6) Defibrillators 
tags: CodingGames
article_header:
  type: cover
  image: 
      src: 
---

다음 문제인 Defibrillators 이다.

출력되는 수많은 정보들로부터 경위도를 수집한 뒤, 기존의 경위도와 비교하여 가장 가까운 장소의 이름을 출력하는 문제이다.  
String의 split을 이용하여 위치 정보를 이름, 주소, 경도, 위도로 구분된 배열로 만든 뒤에 이름을 placeArray라는 배열에 넣는다.  
그 다음 거리를 계산하는 수식을 거친 뒤에 거리 정보를 distArray라는 배열에 넣은 다음 가장 적은 값을 가진 value의 인덱스를 구해서  
placeArray로부터 꺼내는 방식으로 해결했다.

``` javascript
const LON = readline();
const LAT = readline();
const N = parseInt(readline());

let distArray = [];
let placeArray = [];

for (let i = 0; i < N; i++) {
    const DEFIB = readline();

    let info = DEFIB.split(";");

    let lon = Number.parseFloat(LON.replace(",","."));

    let lat = Number.parseFloat(LAT.replace(",","."));

    let defibLon = Number.parseFloat(info[4].replace(",","."));

    let defibLat = Number.parseFloat(info[5].replace(",","."));

    let x = (defibLon - lon) * Math.cos((lon+defibLon)/2);

    let y = defibLat - lat;

    let dist = Math.sqrt(Math.pow(x,2) + Math.pow(y,2)) * 6371;

    placeArray.push(info[1]);
    distArray.push(dist.toString());

}

let minDist = Math.min.apply(null,distArray);

let idx = distArray.indexOf(minDist.toString());

console.log(placeArray[idx]);

```
