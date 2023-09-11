# What is Built-in Objects?

<br/>

## 0. 내장 객체

### (1) 용어 정리

- 자바스크립트 안에는 유용한 내장객체들이 존재해, 조금 더 쉽게 코딩할 수 있음
- 다만 자바스크립트만으로는 브라우저에 화면을 보여주거나, 네트워크 전송을 하거나, 파일을 읽을 수 없기에 런타임 환경에서 제공하는 호스트 객체(Host Objects)를 사용함
- 호스트 객체란, 브라우저 런타임 환경이라면 브라우저 호스트가 제공하는 Browser APIs, 노드 환경이라면 노트 환경에서 제공하는 Node APIs가 있음
- 뿐만 아니라, 사용자 정의 객체(User-defined Objects)가 있음

## 1. 래퍼 객체 (Wrapper Objects)

### (1) 래퍼 객체란?

- 원시값을 **필요에 따라** 관련된 빌트인 객체로 변환, 필요하지 않다면 다시 원시값으로 되돌린다.
- 객체는 값 뿐만 아니라 다양한 정보들을 가지고 있기에 원시 타입보다 무겁고 메모리를 많이 차지함
- 값을 만들 때마다 객체를 생성하면 애플리케이션이 무거워짐
- 따라서 가능하면 원시 타입을 쓰다가, 필요할 때 객체로 변환하고, 다시 원시 타입으로 변환

### (2) 용례

```javascript
const number = 123; // number 원시 타입
console.log(number.toString()); // number 원시타입을 감싸고 있는 < Number 객체로 감싸짐
console.log(number); // number 원시 타입

const text = 'text'; // string 문자열 원시 타입
console.log(text);
text.length; // String 객체
text.trim();
```

<br/>

## 2. 글로벌 객체 (Global Objects)

### (1) 전역적으로 사용할 수 있는 것들

```javascript
console.log(globalThis);
console.log(this); // 전역 객체
console.log(Infinity);
console.log(NaN);
console.log(undefined);

eval('const num = 2; console.log(num)'); // 2
console.log(isFinite(1)); // true
console.log(isFinite(Infinity)); // false

console.log(parseFloat('12.43')); // 12.43 → 숫자로 나타냄
console.log(parseInt('12.43')); // 12 → 실수를 정수로 변환
console.log(parseInt('11')); // 11
```

```javascript
// URL (URI, Uniform Resource Identifier 하위 개념)
// 아스키 문자로만 구성되어야 함
// 한글이나 특수문자는 이스케이프 처리 (정해진 다른 문자로 변환) 해야 함
const URL = 'https://구글.com';
const encoded = encodeURI(URL);
console.log(encoded); // https://%EA%B5%AC%EA%B8%80.com

const decoded = decodeURI(encoded);
console.log(decoded); // https://구글.com

// 전체 URL이 아니라 부분적인 것은 Component이용
const part = '구글.com';
console.log(encodeURIComponent(part)); // %EA%B5%AC%EA%B8%80.com
```

<br/>

## 3. 불리언 함수들

### (1) 용례

```javascript
const isTrue = new Boolean(true); // 하지만 메모리 많이 잡아먹으므로
const isTrue = true // 그냥 true로
console.log(isTrue.valueOf());
```

### (2) Falshy

- 0
- -0
- null
- NaN
- undefined
- ''

### (3) Truthy

- 1
- -1
- '0'
- 'false': 주의⚠️
- []
- {}: object 자체가 값

<br/>

## 4. 숫자 함수들

```javascript
// 숫자 표기 방법
const num1 = new Number(123); // 하지만 메모리 많이 잡아먹으므로
const num2 = 123; // 그냥 숫자로
console.log(num1); // Number {123}
console.log(typeof num1); // object
console.log(num2); // 123
console.log(typeof num2); // number

// 상수 변수들
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308 (e+308은 10의 308승)
console.log(Number.MIN_VALUE); // 5e-324
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
console.log(Number.NaN); // NaN
console.log(Number.NEGATIVE_INFINITY); // -Infinity
console.log(Number.POSITIVE_INFINITY); // Infinity

// 범위 안에 있는지 여부 확인
if (num1 < Number.MAX_VALUE) {
}
if (Number.isNaN(num1)) {
}

// 지수표기법 (매우 크거나 작은 숫자를 표기할 때 사용, 10의 n승으로 표기)
const num3 = 102;
console.log(num3.toExponential()); // 1.02e+2

// 실수를 반올림하여 문자열로 변환
const num4 = 1234.12;
console.log(num4.toFixed()); // 1234
console.log(num4.toString()); // 1234.12 → 문자열로 반환
console.log(num4.toLocaleString('ar-EG')); // ١٬٢٣٤٫١٢ → 해당 나라의 언어로 표기

// 원하는 자릿수까지 유효하도록 반올림
console.log(num4.toPrecision(5)); // 1234.1
console.log(num4.toPrecision(4)); // 1234
console.log(num4.toPrecision(2)); // 1.2e+3 → 전체 자릿수 표기가 안될 때는 지수표기법

// 0과 1사이에서 나타낼 수 있는 가장 작은 숫자
if (Number.EPSILON > 0 && Number.EPSILON < 1) {
  console.log(Number.EPSILON); // 2.220446049250313e-16
}
const num = 0.1 + 0.2 - 0.2; // 0.1 이라고 예상할 수 있지만
console.log(num); // 0.10000000000000003임 (10진수를 각각 2진수로 변환해 계산하고, EPSILON는 작은 오차까지도 나타내므로)

// 미세한 차이가 감지되는 경우
function isEqual(original, expected) {
  return original === expected;
}
console.log(isEqual(1, 1)); // true
console.log(isEqual(0.1, 0.1)); // true
console.log(isEqual(num, 0.1)); // false

// 미세한 차이를 똑같다고 간주하고 싶다면
function isEqual(original, expected) {
  return Math.abs(original - expected) < Number.EPSILON;
}
console.log(isEqual(1, 1)); // true
console.log(isEqual(0.1, 0.1)); // true
console.log(isEqual(num, 0.1)); // true
```

<br/>

## 5. 수학 관련 함수들

```javascript
// 오일러의 상수, 자연로그의 밑
console.log(Math.E); // 2.718281828459045
// 원주율 PI값
console.log(Math.PI); // 3.141592653589793

// 절대값
console.log(Math.abs(-10)); // 10
// 소수점 이하를 올림
console.log(Math.ceil(1.4)); // 2
// 소수점 이하를 내림
console.log(Math.floor(1.4)); // 1
// 소수점 이하를 반올림
console.log(Math.round(1.4)); // 1
console.log(Math.round(1.7)); // 2
// 정수만 반환
console.log(Math.trunc(1.5432)); // 1

// 최대, 최소값을 찾기
console.log(Math.max(1, 3, 4)); // 4
console.log(Math.min(1, 3, 4)); // 1

// 거듭제곱
console.log(3 ** 2); // 9
console.log(Math.pow(3, 2)); // 9

// 제곱근
console.log(Math.sqrt(9)); // 3

// 랜덤한 값을 반환
console.log(Math.random()); // 0.8629275904622469

// 1~10 사이의 랜덤한 값을 반환
console.log(Math.floor(Math.random() * 10 + 1)); // 3
```

<br/>

## 6. 문자열 함수들

```javascript
const textObj = new String('Hello World!');
const text = 'Hello World!';
console.log(textObj); // String {'Hello World!'}
console.log(text); // Hello World!
console.log(text.length); // 12

console.log(text[0]); // H
console.log(text[1]); // e
console.log(text[2]); // l
console.log(text.charAt(0)); // H
console.log(text.charAt(1)); // e
console.log(text.charAt(2)); // l

console.log(text.indexOf('l')); // 2
console.log(text.lastIndexOf('l')); // 9

console.log(text.includes('tx')); // false
console.log(text.includes('h')); // false → 대소문자 구분
console.log(text.includes('H')); // true

console.log(text.startsWith('He')); // true
console.log(text.endsWith('!')); // true

console.log(text.toUpperCase()); // HELLO WORLD!
console.log(text.toLowerCase()); // hello world!

console.log(text.substring(0, 2)); // He
console.log(text.slice(2)); // llo World!
console.log(text.slice(-2)); // d! → 뒤에서부터

const space = '            space       ';
console.log(space.trim()); // space → 공백 제거

const longText = 'Get to the, point';
console.log(longText.split(' ')); // ['Get', 'to', 'the,', 'point']
console.log(longText.split(', ', 2)); // ['Get to the', 'point']
```

<br/>

## 7. 날짜 관련 함수들

```javascript
// UTC 기준 (협정 세계시, 1970년 1월 1일 UTC 자정과의 시간 차이를 밀리초 단위로 표기)
console.log(new Date()); // Mon Sep 11 2023 12:33:52 GMT+0900
console.log(new Date('Jun 5, 2022')); // Sun Jun 05 2022 00:00:00 GMT+0900
console.log(new Date('2022-12-17T03:24:00')); // Sat Dec 17 2022 03:24:00 GMT+0900

console.log(Date.now()); // 1694403232031
console.log(Date.parse('2022-12-17T03:24:00')); // 1671215040000

const now = new Date();
now.setFullYear(2023);
now.setMonth(0); // 0: 1

console.log(now.getFullYear()); // 2023
console.log(now.getDate()); // 11 → 0이 1월임 
console.log(now.getDay()); // 3 → 0이 일요일, 1... 6이 토요일 
console.log(now.getTime()); // 1673408408427
console.log(now); // Wed Jan 11 2023 12:33:52 GMT+0900

console.log(now.toString()); // Wed Jan 11 2023 12:33:52 GMT+0900
console.log(now.toDateString()); // Wed Jan 11 2023
console.log(now.toTimeString()); // 12:33:52 GMT+0900
console.log(now.toISOString()); // 2023-01-11T03:33:52.031Z → ISO 8601 형식
console.log(now.toLocaleString('en-US')); // 1/11/2023, 12:33:52 PM → 미국 형식
console.log(now.toLocaleString('ko-KR')); // 2023. 1. 11. 오후 12:33:52 → 한국 형식
```

<br/>

## 8. 응용

### (1) 문자열의 모든 캐릭터를 하나씩 출력

```javascript
const text = 'Hello World!';
for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
// H
// e
// ..
// !
}
```

### (2) 사용자들의 id를 잘라내어 각각의 id를 배열로 보관

```javascript
const ids = 'user1, user2, user3, user4';

// 나의 풀이
console.log(Array.from(ids.split(' ')));
// ['user1', 'user2', 'user3', 'user4']

// 다른 풀이
const array = ids.split(', ');
console.log(array);
// ['user1', 'user2', 'user3', 'user4']
```

### (3) 1초에 한번씩 시계를 (날짜 포함) 출력

```javascript
// 기본 틀: 우리가 원하는 콜백함수를 첫 번째 인자로 전달 후, 1초 단위로 출력
setInterval(() => {

}, 1000)

// 나의 풀이
const intervalID = setInterval(myCallback, 1000);
function myCallback() {
  console.log(new Date);
}
// Mon Sep 11 2023 16:49:09 GMT+0900 (Korean Standard Time)

// 나의 풀이: 화살표 함수 사용
setInterval(() => {
  const now = new Date();
  console.log(now);
}, 1000);
// Mon Sep 11 2023 16:49:09 GMT+0900 (Korean Standard Time)

// 다른 풀이: 날짜 형식 변환
setInterval(() => {
  const now = new Date();
  console.log(now.toLocaleString('ko-KR'));
}, 1000);
// 2023. 9. 11. 오후 4:49:23
```
