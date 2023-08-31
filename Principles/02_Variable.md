# What is Variable?

<br/>

## 1. 우선, 메모리에 대해 알아보자

### (1) 컴퓨터의 구성요소

- 하드디스크: 파일이나 Application을 저장하는 저장장치
- CPU: 저장장치에서 데이터를 읽어와 연산하고 처리하는 장치
- 메모리: 데이터를 임시적으로 빠르게 읽고 쓰기 위한 장치

### (2) 컴퓨터에서 문서 파일을 열면 일어나는 일

- 하드디스크에 저장된 폴더 안의 파일 선택 → CPU가 처리해서 메모리로 가져옴 → 파일을 읽고 처리할 수 있는 Application을 모니터에 띄워줌 → 사용자가 수정하는 사항들을 메모리상에 주기적으로 업데이트 → 작업 내용 저장하고 종료하면 메모리 상의 데이터를 하드디스크에 다시 저장

### (3) 메모리

- 메모리 셀 이라고 불리는 각각의 칸(저장장치)의 연속으로 이루어진 것
- 각각의 메모리 셀은 1byte(8bit) 사이즈로 만들어 짐
- Appication을 사용하면서 메모리가 더 필요하다면 → 필요하지 않은 Application은 잠시 하드디스크에 넣어둠 → 필요한만큼 메모리를 늘렸다가 줄였다가 할 수 있음
- Application이 메모리에 올라왔을 때, 아래 네 가지 요소들로 구성됨
  - Code: Application 구동 시 필요한 코드들
  - Data: Application에 전반적으로 필요한 변수와 데이터
  - Stack: Application를 실행하는 함수를 호출하는 순서를 보관하는 곳
  - Heap: Application에서 복잡한 데이터를 묶어놓은 객체를 할당하는 곳

<br/>

## 2. 변수 선언 및 할당

### (1) 변수란

값을 저장하는 공간. 즉, 자료를 저장할 수 있는 **이름이 주어진 기억 장소**

```javascript
// a라는 변수 이름에, let이라는 키워드를 사용하여, 변수를 선언
let a;

// a라는 변수 이름에, let이라는 키워드를 사용하여, 변수를 선언 후, 값을 할당
let a = 0;

// 값의 재할당
a = 1;
```

- 값이 처음 할당될 때, 메모리 어딘가에 0이라는 데이터가 저장됨
- 이 메모리는 각각의 메모리 셀마다 그에 해당하는 메모리 주소 a가 있음
- 여기서 a는 변수이름 또는 식별자 라고 함

<br/>

## 3. 변수 이름 짓기

저장된 값을 잘 나타낼 수 있는 **의미 있는** 이름. 따라서 구체적 일수록 좋다. `이 변수에는 어떤 값이 들어 있는가?` 를 바로 알 수 있는 이름이면 더 좋다.

```javascript
// 명확한 의미를 알 수 없어서 ❌
let number = 20;
let audio1;
let audio2;

// 명확한 의미를 나타내므로 🆗
let myAge = 20;
let backgroundAudio;
let windAudio;

// 무엇인지를 먼저 나타내고, 구체적인 의미를 뒤에 쓰는 방식도 👍
// 추후 audio라는 것을 치면, 관련된 변수들이 쫙 나오기 때문
let audioBackground;
let audioWind;
```

### (1) 변수 규칙

- 라틴문자(0-9, a-z, A-Z), 하이픈(_) 🆗
- 대소문자를 구분함
- 한국어 ❌
- 예약어 ❌ [(MDN 예약어 종류)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#keywords)
- 숫자로 시작 ❌
- 특수문자 ❌ (_와 $는 🆗)
- 이모지 ❌
- 여러 개의 변수를 1, 2, 3 숫자로 구분 ❌

<br/>

## 4. 데이터 타입

### (1) 원시(primitive)

- 단일 데이터. 즉, 딱 하나의 데이터를 담을 수 있음
- number, string, boolean, null, undefined, symbol

### (2) 객체(object)

- 복합 데이터. 즉, 여러 가지 데이터를 한번에 묶을 수 있음
- array, function

<br/>

## 5. 숫자 타입 (number)

```javascript
let integer = 123; // 정수
let negative = -123; // 음수
let double = 1.23; // 실수

let binary = 0b1111011; // 123 (2진수)
let octal = 0o173; // 123 (8진수)
let hex = 0x7b; // 123 (16진수)

console.log(0 / 123); // 0
console.log(123 / 0); // Infinity
console.log(123 / -0); // -Infinity
console.log(123 / 'text'); // NaN (Not a Number)

let bigInt = 1234567890123456789012345678901234567890n;
// 1234567890123456789012345678901234567890n
```

<br/>

## 6. 문자열 타입 (string)

[MDN String](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)

```javascript
let string = '안녕하세요'; // 안녕하세요
string = `안녕!`; // 안녕

// 특수 문자 출력하기
string = "'안녕!'"; // '안녕!'

// 줄바꿈, 탭, 유니코드
string = '안녕!\n엘라야!\t\t내 이름은\\\u09AC';

// 일반적인 변수 출력
let id = '엘라';
let greetings = "'안녕!, " + id + "✋\n즐거운 하루 보내요!'";

// Template Literal 사용한 출력
greetings = `'안녕, ${id}✋
즐거운 하루 보내요!'`;
```

<br/>

## 7. 불리언 타입 (boolean)

[MDN Boolean](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

```javascript
// boolean 값을 가지고 있는 변수는 is~ 라고 짓는다
let isFree = true;
let isActivated = false;

// !!를 붙이면 값을 true나 false로 변환해 줌
// Falshy 거짓인 값
console.log(!!0); // 0은 false로 간주됨
console.log(!!-0);
console.log(!!'');
console.log(!!null);
console.log(!!undefined);
console.log(!!NaN);

// Truthy 참인 값
console.log(!!1);
console.log(!!-1);
console.log(!!'text');
console.log(!!{}); // 비어있는 Object
console.log(!!Infinity);
```

<br/>

## 8. null과 undefined 타입

```javascript
let variable; // undefined

variable = null;
console.log(variable); // null

let activeItem; // undefined : 아직 활성화된 아이템이 있는지 없는지 모르는 상태
activeItem = null; // 활성화된 아이템이 없는 상태

// null이라는 object가 할당되어 확실하게 비어있다는 상태
console.log(typeof null); // object

// 아직 정해지지 않았다는 의미
console.log(typeof undefined); // undefined
```

### (1) value

- 값이 들어 있는 상태

### (2) null

- 담고 있지 않은 **텅텅 비어있는 상태**

### (3) undefined

- 값이 있는지 없는지, 변수인지 아닌지 **아직 확정되지 않은 상태**

<br/>

## 9. 객체 타입 (object)

### (1) 객체

- 연관 있는 데이터들을 묶어서 보관할 수 있는 큰 object
- 여러가지 데이터를 묶는 상태 뿐만 아니라, 상태와 연관된 행동(function 등)도 묶어줌
- `{key:value}` 에서 value에는 원시 타입이나 또다른 객체를 넣을 수도 있음

### (2) 원시 타입과 객체 타입

- 원시 타입
  - 어디에 변수가 선언되어있느냐에 따라 **Data나 Stack에 들어가 있음**

- 객체 타입
  - object가 key와 value 쌍으로 이루어져 있기에 사이즈가 정해져 있지 않음.
  - 즉, 메모리 셀 안에 한번에 다 들어갈 수 없기 때문에 **Heap 이라는 메모리 공간에 객체가 할당됨**

### (3) 객체를 보관하는 방법

```javascript
let apple = {
  name: 'apple',
  color: 'red',
  display: '🍎',
};
console.log(apple); // {  name: 'apple', color: 'red', display: '🍎' }
console.log(apple.name); // apple
```

- 위와 같이 객체 자체는 Heap 어딘가에 메모리에 저장됨
- 메모리 셀 하나 안에 커다랗고 뚱뚱한 객체가 들어갈 수 없기 때문에, 조금 멀리 있는 Heap이라는 셀 여러 개에 걸쳐서 object(`{}` 안에 있는 것)가 할당됨
- apple 이라는 변수 이름은 메모리 셀을 가리키는데, 메모리 셀 안에는 **object가 들어있는 메모리 주소를 가리킴**
- 즉, **object 부분은 Heap이라는 메모리 공간에 할당**되고, **할당된 여러 개의 셀 중 시작하는 메모리 주소가 보관된 메모리 주소를 변수가 가지고 있는 것**

<br/>

## 10. 값 vs. 참조

### (1) 원시 타입의 값

- 원시 타입은 **값 자체**가 메모리 셀에 들어가 있음
- 아래와 같이, a가 가리키고 있는 메모리 값에 있는 1이 b에도 할당됨
- b에 변수 a를 할당하면 a 메모리에 들어있는 **값인 1이 복사되어서 값 자체가 b에도 할당됨**
- 따라서, 원시 타입은 **값 자체가 복사되어서 할당됨** → **copy by Value**

```javascript
// 원시타입은 값이 복사되어 전달됨
let a = 1;
let b = a; // 1
b = 2;

// a와 b는 각각 독립적
console.log(a); // 1
console.log(b); // 2
```

### (2) 객체 타입의 참조

```javascript
// 객체타입은 참조값(메모리 주소, 레퍼런스)가 복사되어 전달됨
let apple = {
  // 0x1234
  name: '사과',
};
let orange = apple; // 0x1234
orange.name = '오렌지';

// 동일한 object를 가리키고 있기에 둘 다 업데이트 됨
console.log(apple); // { name: '오렌지' }
console.log(orange); // { name: '오렌지' }
```

- 객체는 참조 값, 즉 **메모리 주소**가 변수에 들어 있음
- 아래와 같이, orange 라는 변수에는 apple이 가리키고 있는 **메모리에 있는 주소값이 복사되어 할당됨**
- 따라서, 객체는 메모리 셀 안에 Reference가 들어있기에 이 Reference 자체가 복사됨 → **copy by Reference**

<br/>

## 11. let vs. const

### (1) let

재할당 🆗, object 안에 있는 데이터 변경 🆗

```javascript
let a = 1;
a = 2; // 재할당 가능
```

### (2) const

재할당 ❌, object 안에 있는 데이터 변경 🆗 (메모리 셀 안에 있는 데이터를 변경하는 것이 아니라, 가리키고 있는 특정한 object 내부를 수정하는 것이므로)

```javascript
const text = 'hello';
// text = 'hi'; ❌

// 1. '상수'라고 불림
const MAX_FRUITS = 5;

// 2. '재할당 불가능한 상수변수' 또는 '변수'라고 불리기도 함
const apple = {
  name: 'apple',
  color: 'red',
  display: '🍎',
};
// apple = {}; ❌

// apple에 다른 메모리 주소를 담을 수는 없지만 (재할당 불가)
// object를 가리키는 곳에 있는 object 안에 있는 데이터는 변경 가능
console.log(apple); // { name: 'apple', color: 'red', display: '🍎' }
apple.name = 'orange'; // { name: 'orange', color: 'red', display: '🍎' }
apple.display = '🍏'; // { name: 'orange', color: 'red', display: '🍏' }
```

<br/>

## 12. 타입

### (1) 정적 타입

- Java, C+, C++ 같은 프로그래밍 언어는 **컴파일러**로 실행 파일로 변환해야 실행 가능
- 컴파일 시 코드에 있는 모든 데이터 타입들이 **정적**으로 정해져 있음 → **Static Type**
- 할당된 값에 따라 타입이 결정되기에 약하게 타입이 결정되지 않음 → **Strongly Type**

### (2) 동적 타입

- JavaScript 엔진은 **인터프리터**로, 런타임 시 동적으로 코드를 한 줄씩 번역해서 실행
- 따라서 타입들이 **동적**으로 결정됨 → **Dynamic Type**
- 할당된 값에 따라 타입이 결정되기에 약하게 타입이 결정됨 → **Weekly Type**

### (3) 타입 확인 방법 : typeof

typeof: 값을 타입 문자열로 반환해 주는 연산자

```javascript
let variable;
console.log(typeof variable);

variable = ''; // string
variable = 123; // number
variable = {}; // object
variable = function () {}; // function
variable = Symbol(); // symbol

console.log(typeof 123); // number
console.log(typeof '123'); // string
```
