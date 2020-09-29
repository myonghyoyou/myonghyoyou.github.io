---
title: CodingGames (8) Ghost Legs 
tags: CodingGames
article_header:
  type: cover
  image: 
      src: 
---

이번에 풀어본 문제는 Ghost Legs 이다.

사다리 타기를 진행했을 때, 위에 있는 값이 아래의 어떤 값으로 가게 되는지에 대한 알고리즘을 짜야하는 문제이다. \
ASCII ART를 이용한 문제다보니 생소하기도 하고 까다로운 부분들이 많았다.

``` javascript
var inputs = readline().split(' ');
const W = parseInt(inputs[0]);
const H = parseInt(inputs[1]);

let topValue = [];
let bottomValue = [];
let legs = [];
let result = [];

for (let i = 0; i < H; i++) {
    const line = readline();
    
    if(i == 0){
        topValue = line.split("  "); // 사다리 위쪽의 값들을 배열로 변환
    } else if (i == H-1){
        bottomValue = line.split("  "); // 사다리 아래쪽의 값들을 배열로 변환
    } else {
        let tempLine = line;
        
        tempLine = tempLine.replace(/\|--\|/gi, "|--  |"); // 사다리의 줄이 있는 부분을 구분짓는다. 다중으로 있을 경우를 생각하여 정규식 사용. (replace는 하나만 교체하므로)
        tempLine = tempLine.split("  ");

        legs.push(tempLine); // 사다리 배열들을 legs 배열에 넣는다.
    }
}

for(let i = 0; i < topValue.length; i++){
    let pair = "";
    pair += topValue[i];
    let end = i;

    for(let j = 0; j<legs.length; j++){
        if(legs[j][end].length == 3){ // 사다리에 줄이 있으면 end 값을 오른쪽으로 한 칸 이동
            end = end +1;
        } else if (end != 0){
            if(legs[j][end-1].length == 3){ // 사다리에 줄이 있으면 end 값을 왼쪽으로 한 칸 이동
                end = end-1 
            }
        }
    }
        pair += bottomValue[end];

        console.log(pair);
}
```


