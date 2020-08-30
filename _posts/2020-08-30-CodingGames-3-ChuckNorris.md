---
title: CodingGames (4) Chuck Norris 
tags: CodingGames
article_header:
  type: cover
  image: 
      src: 
---
이번에는 Chuck Norris에 대한 풀이다.

문자열을 binary 코드로 바꾸는 데에는 성공했으나 0과 1의 배치에 따라 0을 규칙에 맞게 출력시켜야 하는 부분에서 막혔다.

다른 사람들의 풀이를 참조하며 풀어간 문제. 객체를 활용하여 0과 1에 따라 다른 출력이 나오게끔 한 부분이나, 삼항 연산자를 통해 조건을 걸어준 것도 인상적이었다.
비슷한 부분이 나온다면 활용할 수 있도록 해야겠다.

``` javascript
const MESSAGE = readline();

function textToBin(text){
    let ascii = "";

    for(let i = 0; i < text.length; i++){
        
        let bin = text.charCodeAt(i).toString(2);

        if(bin.length < 7){
            bin = '0' + bin;
        }

        ascii += bin;
    }

    return ascii;
}

let encode = {
    "0" : "00",
    "1" : "0"
}

let binaryText = textToBin(MESSAGE);

let currentSign = binaryText[0];

let output = encode[currentSign]+" "+"0"

for(var i = 1 ; i < binaryText.length ; i++)
{
    let nextBit = binaryText[i];
    output += ( nextBit != currentSign ) ? " "+encode[nextBit]+" "+"0" : "0";
    currentSign = nextBit;
}

console.log(output);
```
