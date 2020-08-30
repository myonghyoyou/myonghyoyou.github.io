---
title: CodingGames (3) ASCII Art
tags: CodingGames
article_header:
  type: cover
  image: 
      src: ../assets/images.jfif
---
이번에는 ASCII에 대한 풀이다.
지문 이해부터 쉽지 않았찌만 파악할 수 있었고 겨우내 풀어냈다.

알파벳에 포함되지 않는 문자가 들어올 경우 '?'로 표시해야하는 부분에서 한 번 막혔고, 배열 내에서 인덱스 번호를 구해내서 문자를 출력하는 부분에서 또 막혔다.

쉽게 갈 수 있는 걸 자꾸 어렵게 가는 버릇? 습관?이 있는거 같다.

``` js
const L = parseInt(readline());
const H = parseInt(readline());
const T = readline();

let alphabet = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z','?'];
let row = [];
let upperT = T.toUpperCase();
let upperTArray = upperT.split("");
    
for (let i = 0; i < H; i++) {
    const ROW = readline();

    for(let j=0; j<alphabet.length; j++){
        row.push(ROW.slice(j*L, (j+1)*L));
    }

    let word = ""; 
    for(let k=0; k<upperTArray.length; k++){
        
        let idx = alphabet.indexOf(upperTArray[k]);
        
        if(idx !== -1){
            word = word + row[idx+(27*i)];
        } else {
             word = word + row[idx + (27*(i+1))];
        }
    }

    console.log(word);
}
```
