---
title: CodingGames (5) MIME Type 
tags: CodingGames
article_header:
  type: cover
  image: 
      src: 
---
MIME Type을 풀어보았다.
파일의 확장자명에 따른 MIME Type을 출력하는 문제로 split과 toLowerCase를 써서 해결해야하는 케이스도 주어졌다.
복잡하지 않은 문제 덕에 쉽게 풀 수 있었으나 코드가 뭔가 조잡한 느낌이 든다....ㅠ

``` javascript
const N = parseInt(readline());
const Q = parseInt(readline());

let extArray = [];
let mtArray = [];


for (let i = 0; i < N; i++) {
    var inputs = readline().split(' ');
    const EXT = inputs[0];
    const MT = inputs[1];

    extArray.push(EXT.toLowerCase());
    mtArray.push(MT);
}
for (let i = 0; i < Q; i++) {
    const FNAME = readline();


    let ext = (FNAME.toLowerCase()).split(".");

    console.error(ext);

    let length = ext.length;

    if(length === 1){
        ext = undefined
    } else {
        ext = ext[length-1];
    }

    if(ext !== undefined){
        let idx = extArray.indexOf(ext);
        if(mtArray[idx] !== undefined){
            console.log(mtArray[idx]);
        } else {
            console.log("UNKNOWN");
        }
    } else {
        console.log("UNKNOWN");
    }

}
```
