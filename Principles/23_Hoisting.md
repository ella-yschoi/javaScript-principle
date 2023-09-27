# What is Hoisting?

<br/>

## 1. Hoisting(호이스팅)

### (1) 의미

- 자바스크립트 엔진(인터프리터)이 코드를 실행하기 전에 변수, 함수, 클래스의 선언문을 **끌어올리는 것**
- 변수의 선언과 초기화를 분리한 후, 선언만 코드의 최상단으로 옮김

### (2) 응용

- 함수의 호이스팅은 함수의 **선언문 전에 호출이 가능**하게 해줌
- 함수의 선언문은 **선언 이전에도 호출 가능**함

```javascript
print();

function print() {
  console.log('Hello');
}
```

```javascript
// 변수(let, const)와 클래스는 선언만 호이스팅이 되고, 초기화는 안됨
// 초기화 전, 변수에 접근하면 컴파일(빌드) 에러가 발생
// console.log(hi);
let hi = 'hi';
let func1 = function () {};

// const cat = new Cat();
class Cat {}

let x = 1;
{
  console.log(x); // Cannot access 'x' before initialization
  let x = 2;
}
```

<br/>

## 2. Var

### (1) Var 사용 시 단점

- 일반 코딩 방식과 어긋나기에 개발하면서도 혼란스러움
- 코드의 가독성과 유지보수성에 좋지 않음

### (2) 특징과 용례

변수 선언하는 키워드 없이 선언 & 할당이 가능하여 선언인지, 재할당인지 구분하기 어려움

```javascript
// var 사용하는 것과 같음
something = '💩';
console.log(something);
```

중복 선언이 가능하여 (특히 협업 시) 실수 발생

```javascript
var poo = '💩';
var poo = '💩';
console.log(poo);
```

블록 레벨 스코프 안됨

```javascript
// const ver.
// 블록 밖에서는 내부에 접근 불가
const banana = '바나나';
{
  const banana = '🍌';
  {
    const banana = '😋';
  }
}
console.log(banana); // 바나나

// var ver.
// 블록 밖인데도 내부에 접근 가능,,
var apple = '사과';
{
  var apple = '🍎';
  {
    var apple = '🍏';
  }
}
console.log(apple); // 🍏
```

함수 레벨 스코프만 지원 됨

```javascript
// 함수 밖에서 내부에서 사용하는 변수에 접근 불가
function example() {
  var dog = '🦮';
}
console.log(dog); // Uncaught ReferenceError: dog is not defined
```

<br/>

## 3. Strict mode

### (1) 특징

> [MDN: Strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

- 리액트와 같은 프레임워크 사용시 기본적으로 엄격 모드로 실수를 만회할 수 있게 해줌
- 하지만 순수 자바스크립트에서는 기본으로 설정되어 있지 않기에 상단에 strict mode 설정 필요

### (2) 사용하기

```javascript
'use strict';
// var x = 1;
// delete x;

function add(x) {
  var a = 2;
  // strict mode에서 키워드 생략 불가
  var b = a + x;
  console.log(this);
}
add(1);

const array = [1, 2, 3];
// strict mode에서는 num 선언 필요
for (const num of array) {
  console.log(num);
}
```
