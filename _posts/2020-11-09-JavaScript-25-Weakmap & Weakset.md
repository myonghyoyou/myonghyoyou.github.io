---
title: JavaScript (25) Weakmap과 Weakset
tags: JavaScript
article_header:
  type: cover
  image:


---

### 개요

자바스크립트 엔진은 도달 가능한 (혹은 추후에 사용될 가능성이 있는) 값을 메모리에 유지한다. 

예시:

```` javascript
let john = {name: 'john'};

// 위 객체는 john이라는 참조를 통해 접근 가능하다.

// 그러나 참조를 null로 덮어쓰면 위 객체에 더 이상 도달이 가능하지 않게 된다.
john = null;

// 객체가 메모리에서 삭제된다.
````

자료구조를 구성하는 요소 (객체의 프로퍼티, 배열의 요소, 맵/셋의 요소)도 자신이 속한 자료구조가 남아있는 동안 대개 도달 가능한 값으로 취급되어 메모리에서 삭제되지 않는다. 

아래 코드를 통해 확인해보자.

```` javascript
let john = {name: 'John'};

let array = [ john ];

john = null; // 참조를 null로 덮어쓴다.

// john을 나타내는 객체는 배열의 요소이기 때문에 가비지 컬렉터의 대상이 되지 않는다.
// array[0]를 사용하면 해당 객체를 얻을 수 있다.
alert(JSON.stringify(array[0]));
````

`맵`에서 객체를 키로 사용한 경우 역시, `맵`이 메모리에 있는 한 객체도 메모리에 남는다.

예시 :

```` javascript
let john = { name: 'john' };

let map = new Map();
map.set (john, "...");

john = null; // john을 null로 덮어쓴다.

// john을 나타내는 객체는 map의 키로 저장되어있다.
// map.keys()를 활용해 해당 객체를 얻어낼 수 있다.
for (let obj of map.keys()) {
    alert(JSON.stringify(obj)); // { "name":"john" }
}

alert(map.size); // 1

````

---

### 위크맵 (Weakmap)

`위크맵(Weakmap)`은 일반 `맵`과 전혀 다른 양상을 보인다. 위크맵을 사용하면 키로 쓰인 객체가 가비지 컬렉션의 대상이 된다.

`맵`과 `위크맵`의 첫 번째 차이는 `위크맵`의 키가 반드시 객체여야 한다는 점이다. 원시값은 위크맵의 키가 될 수 없다.

```` javascript
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, '...'); // 정상적으로 동작한다. (객체 키)

// 문자열은 키로 사용할 수 없다.
weakMap.set('test', 'whoops'); // Error: ...
````

위크맵의 키로 사용된 객체를 참조하는 것이 아무것도 없다면 해당 객체는 메모리와 위크맵에서 자동으로 삭제된다.

```` javascript
let john = { name: 'john' };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // 참조를 null로 덮어쓴다.

// john을 나타내는 객체는 이제 메모리에서 사라진다.
````

`john`을 나타내는 객체는 오로지 `위크맵`의 키로만 사용되고 있으므로, 참조를 덮어쓰게 되면 이 객체는 위크맵과 메모리에서 자동으로 삭제된다.

`맵`과 `위크맵`의 두 번째 차이는 `위크맵`에서는 반복 작업과 `keys()`, `values()`, `entries()` 메서드를 지원하지 않는다는 점이다. 이 때문에 `워크맵`에서 키나 값 전체를 얻는 것은 불가하다.

`워크맵`이 지원하는 메서드는 아래와 같다.

* weakMap.get(key)
* weakMap.set(key, value)
* weakMap.delete(key)
* weakMap.has(key)

`위크맵`은 가비지 컬렉션의 동작 방식 때문에 위와 같이 적은 메서드만 제공할 뿐이다. 위의 예시처럼 `john`을 나타내는 객체처럼, 객체는 모든 참조를 잃게 되면 가비지 컬렉션의 대상이 된다.

그러나 대상이 됐다고 해서 그 즉시 메모리에서 제거됨을 의미하지는 않는다. 가비지 컬렉션이 일어나는 시점은 자바스크립트 엔진이 결정한다. 그 즉시 메모리에서 삭제될 수도 있고, 다른 삭제 작업이 있을 때까지 대기하다가 함께 삭제될 수도 있는 것이다. 이 때문에 `위크맵` 내부에 요소가 몇 개 있는지 파악이 불가능하다. 

---

### 유스 케이스 : 추가 데이터

`위크맵`은 부차적인 데이터를 저장할 곳이 필요할 때 사용한다.

서드 파티 라이브러리와 같은 외부 코드에 '속한' 객체를 가지고 작업을 해야한다고 가정해보자. 이 객체에 데이터를 추가해줘야 하는데, 추가해 줄 데이터는 객체가 살아있는 동안에만 유효한 상황이다. 이럴 때 `위크맵`을 사용할 수 있다.

`위크맵`에 원하는 데이터를 저장하고, 이 때 키는 객체를 사용하면 된다.  이렇게 하면 객체가 가비지 컬렉션의 대상이 될 때, 데이터도 함께 사라진다.

```` javascript
weakMap.set(john, "비밀문서");
// john이 사망하면, 비밀문서는 자동으로 파기된다.
````

좀 더 구체적인 예시를 보자.

사용자의 방문 횟수를 세어 주는 코드가 있다. 관련 정보는 맵에 저장하고 있는데 맵 요소의 키엔 특정 사용자를 나타내는 객체를, 값엔 해당 사용자의 방문 횟수를 저장한다. 어떤 사용자의 정보를 저장할 필요가 없어지면 해당사용자의 방문 횟수도 저장할 필요가 없어진다.

아래 예시는 `맵`을 사용해 사용자의 방문 횟수를 센다.

``` javascript
// visitsCount.js
let visitsCountMap = new Map(); // 맵에 사용자의 방문횟수를 지정한다.

// 사용자가 방문하면 방문횟수를 늘린다.
function countUser(user) {
    let count = visitsCountMap(user) || 0;
    visitsCountMap.set(user, count+1);
}
```

아래는 John이라는 방문자가 방문했을 때, 방문 횟수가 증가하는 모습이다.

``` javascript
// main.js
let john = { name: 'john' };

countUser(john); // John의 방문 횟수를 증가시킨다.

// John의 방문 횟수를 셀 필요가 없어지면 아래와 같이 john을 null로 덮어씌운다.
john = null;
```

`john`을 null로 덮어썼기 때문에 가비지 컬렉션의 대상이 되어야 하지만, `visitsCountMap`의 키로 사용되기 때문에 메모리에서 삭제되지 않는다.

특정 사용자를 나타내는 객체가 메모리에서 사라지면 해당 객체에 대한 정보(방문 횟수)도 직접 지워줘야 하는 상황이다. 이렇게 하지 않으면 `visitsCountMap`이 차지하는 메모리 공간이 한없이 커진다.

이런 문제를 `위크맵`을 통해 해결할 수 있다.

``` javascript
// visitsCount.js
let visitsCountMap = new WeakMap(); // 위크맵에 사용자의 방문횟수를 저장한다.

// 사용자가 방문하면 방문 횟수를 늘려준다.
function countUser(user) {
    let count = visitsCountMap.get(user) || 0;
    visitsCountMap.set(user, count+1); 
}

```

`위크맵`을 사용해 사용자 방문 횟수를 저장하면 `visitsCountMap`을 직접 청소해줄 필요가 없다. `john`을 가리키는 객체가 도달 불가한 상태가 되면 자동으로 메모리에서 삭제되기 때문이다. `위크맵`의 키(`john`)에 대응하는 값(방문 횟수)도 자동으로 가비지 컬렉션의 대상이 된다.

---

### 유스 케이스: 캐싱

위크맵은 캐싱(Caching)에도 유용하다. 동일한 함수를 여러번 호출해야 할 때, 최초 호출 시 반환된 값을 어딘가에 저장해 놓았다가 그 다음엔 함수를 호출하는 대신 저장된 값을 사용하는 게 캐싱의 실례이다.

아래 예시는 함수 연산 결과를 `맵`에 저장한다.

``` javascript
// cache.js
let cache = new Map();

// 연산을 수행하고 그 결과를 맵에 저장한다.
function process(obj) {
    if (!cache.has(obj)) {
        let result = /* 연산 수행 가정 */ obj;
        
        cache.set(obj, result);
    }
    
    return cache.get(obj);
}

// 함수 process()를 호출한다.

//main.js
let obj = {/*...객체...*/};

let result1 = process(obj); // 함수를 호출

// 동일한 함수를 두 번째 호출할 때,
let result2 = process(obj); // 연산을 수행할 필요 없이 맵에 저장된 결과를 가져온다.

// 객체가 쓸모 없어지면 아래와 같이 null로 덮어쓴다.
obj = null;

alert(cache.size); // 1 (cache의 키가 객체를 참조하므로 여전히 객체에 남아있다. 메모리 낭비.)
```

`process(obj)`를 여러 번 호출하면 최초 호출할 때만 연산이 수행되고, 그 후에는 연산된 결과를 `cache`에서 가져온다. 그러나 `맵`을 사용하고 있어서 객체가 필요 없어져도 `cache`를 직접 비워줘야 한다.

`위크맵`을 사용하게 되면 이런 불편함이 해소된다.

``` javascript
// cache.js
let cache = new WeakMap();

// 연산을 수행하고 그 결과를 위크맵에 저장한다.
function process(obj) {
    if (!cache.has(obj)) {
        let result = /* 연산 수행 */ obj;
        
        cache.set(obj, result);
    }
    
    return cache.get(obj);
}

// main.js
let obj = {/*...객체...*/};

let result1 = process(obj);
let result2 = process(obj);

// 객체가 쓸모 없어지면 null로 덮어준다.
obj = null;

// 맵에서 사용한 것과 같이 cache.size를 사용하려고 하면 불가하다.
// obj가 가비지 컬렉션의 대상이 되므로, 캐싱된 데이터 역시 삭제된다.
// 때문에 cache엔 어떤 데이터도 남지 않게 된다.
```

---

### 위크셋(Weakset)

* `위크셋`은 `셋`과 유사하지만, 객체만 저장할 수 있다는 점에서 다르다.
* `위크셋` 안의 객체는 도달 가능할 때만 메모리에 유지된다.
* `셋`과 마찬가지로 `위크셋`이 지원하는 메서드는 단출하다. `add`, `has`, `delete` 를 사용할 수 있고 반복 작업 관련 메서드는 사용할 수없다.

`위크맵`과 유사하게 `위크셋`도 부차적인 데이터를 저장할 때 사용할 수 있다. 다만, 위크셋엔 위크맵처럼 복잡한 데이터를 저장하지는 않는다. "예"나 "아니오" 같은 간단한 답변을 얻는 용도로 사용한다.  

아래 코드에선 사용자의 사이트 방문 여부를 추적하는 용도로 `위크셋`을 사용한다.

```` javascript
let visitedSet = new WeakSet();

let john = { name: 'John' };
let pete = { name: 'Pete' };
let mary = { name: 'Mary' };

visitedSet.add(john); // John이 사이트를 방문한다.
visitedSet.add(pete); // 그 다음 Pete가 방문한다.
visitedSet.add(john); // 다시 John이 사이트를 방문한다.

// visitedSet엔 두 명의 사용자가 저장된다.

// John의 방문 여부를 확인한다.
alert(visitedSet.has(john)); // true

// Mary의 방문 여부를 확인한다.
alert(visitedSet.has(mary)); // false

john = null;

// visitedSet에서 john을 나타내는 객체가 자동으로 삭제된다.
````

---

참조 : <br>

* https://ko.javascript.info/weakmap-weakset
