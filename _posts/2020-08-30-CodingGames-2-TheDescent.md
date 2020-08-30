---
title: CodingGames (2) The Descent
tags: CodingGames
article_header:
  type: cover
  image: 
      src: ../assets/dargon-landing.jpg
---
이번에는 The Descent에 대한 풀이다.
지문에 대한 이해는 충분했으나 접근 방식에서부터 엄청 꼬인 느낌이었다.

나오는 값들을 우선 객체와 배열을 이용해서 저장하고 Math 객체를 통해서 최대값을 구하는 방식으로 접근했는데
코드가 작동을 하지 않을뿐더러 엄청 복잡해지기까지 했다.

나름 괜찮은 접근법이라고 생각했는데 결국 Hint에 있는 psuedo-code를 보면 풀고말았다.
의외로 풀이가 너무 단순해서 허망한 느낌이 들었던 문제. 어떤 문제를 해결할 때에 해결방안에 쉽게 접근하는 방식에 대해 계속 고민해볼 필요가 있을거 같다.

``` js
while (true) {

    let max = 0;
    let iMax = 0;
    
    for (let i = 0; i < 8; i++) {
        const mountainH = parseInt(readline()); // represents the height of one mountain.
        
        if(mountainH > max){
            max = mountainH;
            iMax = i;
        }
    }

    console.log(iMax);


}
```
