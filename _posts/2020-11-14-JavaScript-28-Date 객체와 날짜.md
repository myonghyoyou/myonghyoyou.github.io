---
title: JavaScript (28) Date 객체와 날짜
tags: JavaScript
article_header:
  type: cover
  image:

---

### 개요
Date 객체는 생성 및 수정 시간을 저장하거나 시간을 측정, 현재 날짜를 출력하는 용도로 활용 가능하다.

---

### 객체 생성하기

`new Date()`를 통해 `Date` 객체를 생성할 수 있다. `new Date()`는 여러 형태로 호출될 수 있다.

#####  `new Date()`

인수 없이 호출하면 현재 날짜와 시간이 저장된 `Date` 객체가 반환된다.

```` javascript
let now = new Date();
alert(now); // Mon Nov 30 2020 15:58:44 GMT+0900 (대한민국 표준시)
````

##### `new Date(milliseconds)`

UTC 기준(UTC+0) 1970년 1월 1일 0시 0분 0초에서 `milliseconds` 밀리초 후의 시점이 저장된 `Date` 객체가 반환된다.

```` javascript
let Jan01_1970 = new Date(0);
alert( Jan01_1970 ); // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

let Jan02_1970 = new Date(24 * 3600 * 1000);
alert( Jan02_1970 ); // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
````

##### `new Date(datestring)`

인수가 하나인데, 문자열이면 해당 문자열은 자동으로 parse 된다.

```` javascript
let date = new Date("2020-11-30");

alert(date);
// 인수로 시간은 지정하지 않았기에 GTM 자정이라고 가정하고
// 코드가 실행되는 시간대(timezone)에 따라 출력 문자열이 바뀐다.
// Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
// 또는
// Wed Jan 25 2017 16:00:00 GMT-0800 (Pacific Standard Time)등이 출력됩니다.
````

##### `new Date(year, month, date, hours, minutes, seconds, ms)`

주어진 인수를 조합해 만들 수 있는 날짜가 저장된 객체가 반환된다. `year`와 `month`만 필수값이다.

* `year`는 반드시 네 자리 숫자여야 한다. `2013`은 괜찮고 `98`은 안된다.
* `month`는 `0`(1월)부터 `11`(12월) 사이의 숫자여야 한다.
* `date`는 일을 나타내는데, 값이 없는 경우에 1일로 처리된다.
* `hours/minutes/seconds/ms`에 값이 없는 경우엔 `0`으로 처리된다.

예시:

```` javascript
new Date(2011, 0, 1, 0, 0, 0, 0); // Sat Jan 01 2011 00:00:00 GMT+0900 (대한민국 표준시)
new Date(2011, 0, 1); // hours를 비롯한 인수는 기본값이 0이므로 위와 동일
````

최소 정밀도는 1밀리초이다.

```` javascript
let date = new Date(2011, 0, 1, 2, 3, 4, 567);
alert( date ); // Sat Jan 01 2011 02:03:04 GMT+0900 (대한민국 표준시)
````

---

### 날짜 구성요소 얻기

`Date` 객체를 이용하여 연, 월, 일 등의 값을 얻을 수 있다.

**getFullYear()**

연도(네 자릿수)를 반환한다.

**getMonth()**

월을 반환한다.(0 이상 11이하)

**getDate()**

일을 반환한다.  (1 이상 31 이하).

**getHours(), getMinutes(), getSeconds(), getMilliseconds()**

시, 분, 초, 밀리초를 반환한다.

**getDay()**

`0`부터 `6`까지의 숫자를 반환한다. `0`은 일요일, `6`은 토요일이다.

***위의 메서드 모두 현지 시간 기준 날짜 구성요소를 반환한다.***

위 메서드 이름에서 `get` 다음에 `UTC`를 붙여주면 표준시 기준의 날짜 구성 요소를 받을 수 있다.

``` javascript
// 현재 일시
let date = new Date();

// 현지 시간 기준 시
alert(date.getHours());

// 표준시간대 기준 시
alert(date.getUTCHours());
```

아래 두 메서드는 위의 메서드들과 달리 표준 기준의 날짜 구성 요소를 받을 수 없다.

**getTime()**

주어진 일시와 1970년 1월 1일 00시 00분 00초 사이의 간격인 타임스탬프를 반환한다.

**getTimezoneOffset()**

현지 시간과 표준 시간의 차이(분)를 반환한다.

``` javascript
alert( new Date().getTimezoneOffset() ); // -540
```

---

### 날짜 구성요소 설정하기

아래 메서드를 통해 날짜 구성요소를 설정할 수 있다.

* `setFullYear(year, [month], [date])`
* `setMonth(month, [date])`
* `setDate(date)`
* `setHours(hour, [min], [sec], [ms])`
* `setMinutes(min, [sec], [ms])`
* `setSeconds(sec, [ms])`
* `setMilliseconds(ms)`
* `setTime(milliseconds)` (1970년 1월 1일 00:00:00  UTC부터 밀리초 이후를 나타내는 날짜 설정)

`setTime()`을 제외한 모든 메서드는 `setUTCHours()`와 같이 표준시에 따라 날짜 구성 요소를 설정해주는 메서드가 있다.

`setHours`와 같은 메서드는 여러 개의 날짜 구성요소를 변경할 수 있지만 , 메서드의 인수에 없는 구성요소는 변경되지 않는다.

예시:

``` javascript
let today = new Date();

today.setHours(0);
alert(today); // 날짜는 변경되지 않고 시만 0으로 변경된다.

today.setHours(0, 0, 0, 0);
alert(today); // 날짜는 변경되지 않고 시, 분, 초가 모두 변경된다. (00시 00분 00초)
```

---

### 자동 고침

`Date` 객체엔 *자동 고침*이 내장되어 있다. 범위를 벗어나는 값을 설정하려고 하면 값이 자동으로 수정된다.

예시:

``` javascript
let date = new Date(2013, 0 ,32); // 2013년 1월 32일은 존재하지 않는다.
alert(date) // Fri Feb 01 2013 00:00:00 GMT+0900 (대한민국 표준시)
```

입력받은 날짜 구성 요소가 범위를 벗어나면 초과분은 자동으로 다른 날짜 구성요소에 배분된다.

---

### Date 객체를 숫자로 변경해 시간차 측정하기

`Date` 객체를 숫자형으로 변경하면 타임스탬프가 된다.

``` javascript
let date = new Date();
alert(+date); // 타임스탬프 출력(date.getTime()을 한 것과 동일)
```

---

### Date.now()

`Date.now()` 메서드는 현재 타임스탬프를 반환한다. 

중간에 `Date` 객체를 만들지 않아도 된다는 점 때문에 `new Date().getTime()`을 사용하는 것보다 빠르고 가비지 컬렉터의 일을 줄여준다는 장점이 있다.

---

### Date.parse와 문자열

메서드 Date.parse(str)를 사용하면 문자열에서 날짜를 읽어올 수 있다.

문자열의 형식은 아래와 같아야 한다.

* `YYYY-MM-DD` - 날짜(연 - 월 - 일)
* `"T"` - 구분 기호로 쓰임
* `HH:mm:ss.sss` - 시:분:초:밀리초
* `'Z'`(옵션) - `+-hh:mm` 형식의 시간대를 나타냄. `Z` 한 글자인 경우엔 UTC+0을 나타냄

쥐 조건을 만족하는 문자열을 대상으로 `Date.parse(str)`를 호출하면 문자열과 대응하는 날짜의 타임스탬프가 반환된다. 조건에 맞지 않는 경우에는 `NaN`이 반환된다.

예시:

``` javascript
let ms = Date.parse('2012-01-26T13:51:50.417-07:00');
alert(ms); // 1327611110417  (타임스탬프)
```

`Date.parse(str)`를 이용하여 타임스탬프만으로 새로운 `Date` 객체를 바로 만들 수 있다.

``` javascript
let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') );
alert(date);
```
