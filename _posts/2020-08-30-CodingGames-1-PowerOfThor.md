---
title: CodingGames (1) Power Of Thor &#58; Episode1
tags: CodingGames
article_header:
  type: cover
  image: 
  
---
코스모스 스터디를 통해 CodingGames의 알고리즘 문제 풀이를 진행 중이다.
오늘은 금주에 풀어야 하는 문제 외에도 이제껏 풀지 못했던 문제들까지 풀어보려고 한다.

CodingGames의 문제들을 계속 풀어보며 느끼는 것은 지문과 주어지는 변수에 대한 이해가 진짜 중요하다는 것이다.
Power Of Thor 에서도 initialTx 와 initialTy에 대한 이해가 제대로 되지 않다보니 if문의 조건을 잘못 걸었다.
거기에 더해 증감식에 대한 이해도 부족하다 보니 계속 똑같은 오류가 발생해도 이유를 발견하지 못하고 있었다.

아주 기본적인 내용일지라도 허투루 넘기지 않고 제대로 이해하는 게 중요하다는 것을 알려준 문제.

``` js
var inputs = readline().split(' ');
const lightX = parseInt(inputs[0]); // the X position of the light of power
const lightY = parseInt(inputs[1]); // the Y position of the light of power
const initialTx = parseInt(inputs[2]); // Thor's starting X position
const initialTy = parseInt(inputs[3]); // Thor's starting Y position

let xDist = lightX - initialTx;
let yDist = lightY - initialTy;

// game loop
while (true) {
    const remainingTurns = parseInt(readline()); // The remaining amount of turns Thor can move. Do not remove this line.
    
    console.error(xDist);
    console.error(yDist);

    if(xDist >0 && yDist > 0){
        xDist = --xDist;
        yDist = --yDist;
        console.log("SE");
    } else if (xDist > 0 && yDist === 0 ){
        xDist = --xDist;
        console.log("E");
    } else if (xDist > 0 && yDist < 0 ){
        xDist = --xDist;
        yDist = ++yDist;
        console.log("NE");
    } else if (xDist === 0 && yDist > 0){
        yDist = --yDist;
        console.log("S");
    } else if (xDist === 0 && yDist < 0 ){
        yDist = ++yDist;
        console.log("N");
    } else if (xDist < 0 && yDist < 0 ){
        xDist = ++xDist;
        yDist = ++yDist;
        console.log("NW");
    } else if (xDist < 0 && yDist > 0 ){
        xDist = ++xDist;
        yDist = --yDist;
        console.log("SW");
    } else if (xDist < 0 && yDist === 0){
        xDist = ++xDist;
        console.log("W");
    }

}
```
