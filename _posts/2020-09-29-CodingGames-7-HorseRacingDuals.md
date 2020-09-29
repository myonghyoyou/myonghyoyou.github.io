---
title: CodingGames (7) Horse-Racing Duals 
tags: CodingGames
article_header:
  type: cover
  image: 
      src: 
---

이번에 풀어본 문제는 Horse-Racing Duals 이다.

주어지는 각 변수들 간의 차이를 구한 다음, 그 중 가장 작은 값을 출력하는 문제이다.

문제를 풀기 위해 접근한 방법은 2가지였다. \
첫번째 접근법은 주어지는 변수들을 모두 배열에 담은 뒤 for문 안에서 다 한번씩 뺄셈을 진행하는 것이었다. \
뺄셈을 통해 나오는 결과값을 또 다른 배열에 저장한 뒤, 해당 배열에서 최소값을 찾아 출력하게끔 코드를 짰다.

하지만 이런 식의 접근은  원하는 값을 얻을 수 있는 알고리즘이라 하더라도 \
변수가 많아질수록 연산에 걸리는 시간이 급증하는 탓에 실패로 돌아갔다.

두번째 접근도 주어지는 변수들을 모두 배열에 담는 것까지는 동일하나 배열에 담긴 값을 오름차순으로 정렬하는 과정을 추가했다. \
그렇게 되면 앞뒤로 위치한 숫자와만 뺄셈을 하면 되기 때문에 소요되는 시간을 엄청나게 절약할 수 있다. \
뺄셈을 통해 나온 결과값 중 최소값을 구해 출력하니 문제를 해결할 수 있었다.

요즘 계속 코딩을 하면서 느끼는 것은 원하는 결과를 만들어내는 것도 중요하지만, 그 결과가 만들어지는 과정도 그만큼 신경을 쓸 때에 좋은 개발자가 될 수 있다는 것이다. \
장인정신을 가지고 개발하는 개발자가 되어야겠다.

``` javascript
const N = parseInt(readline());

let piArray = [];
let diffArray = [];

for (let i = 0; i < N; i++) {
    const pi = parseInt(readline());
    piArray.push(pi);
}

piArray = piArray.sort(function(piArray,b){return piArray-b});

for(let j = 0; j < piArray.length; j++){
    
    if(piArray[j-1] == undefined){
        continue;
    }

    let diffValue = piArray[j] - piArray[j-1];

    diffArray.push(diffValue);    
}

let result = Math.min.apply(null,diffArray);

console.log(result);

```
