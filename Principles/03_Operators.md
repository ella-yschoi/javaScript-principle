# What is Operators?

<br/>

## 1. 표현식 (Expressions)

### (1) 리터럴 (Literal)

각각 다른 값을 나타내는 것으로, **코드에서 값을 나타내는 표기법**을 말함

- Template Literal: `${}`
- Function Literal: function() {}
- Bigint Literal: 123n
- Binary Literal: 0b101

### (2) 문 (Statement)

#### '문': 코드의 최소 실행 단위

- 선언문, 할당문, 조건문, 반복문 등
- 표현식(Expressions): **값**으로 평가될 수 있는 문

#### 표현식을 바라보는 포인트

- 코드가 실행되면 **어떤 일이 발생**하는가?
- 코드가 실행되면 **어떤 값이 생성**되는가?
- 이 변수에는 **어떤 값이 들어있는가?**

```javascript
1; // 숫자 리터럴 표현식
1+1; // 연산자 표현식
call(); // 함수 호출 표현식

let b; // 선언문은 값으로 평가될 수 없기에 표현식 ❌
b = 2; // 할당문은 값으로 평가될 수 있기에 표현식 🆗

let a = (b = 2);
console.log(a); // 2
```

<br/>

## 2. 산술 연산자 (Arithmetic Operators)

### (1) 참고할 것

```javascript
console.log(5 ** 2); // 거듭제곱: ES7에서 추가됨
console.log(Math.pow(5, 2)); // ** 전에 사용하던 메소드
```

### (2) 주의할 것

```javascript
let text = '엘라' + '안녕'; // 엘라 안녕 : 문자열 두 개를 더하면 문자열
text = '1' + 1; // 11 : 숫자와 문자열을 더하면 문자열로 변환됨
```

<br/>

## 3. 단항 연산자 (Unary Operators)

### (1) +와 - 연산자

```javascript
let a = 5;
a = -a; // -1*5인 셈
console.log(a); // -5
a = -a;
console.log(a); // 5
a = +a;
console.log(a); // 5
a = -a; // -5
a = +a; // +(-5)
console.log(a); // -5
```

### (2) ! (부정 연산자)

```javascript
let boolean = true;
console.log(boolean); // true
console.log(!boolean); // false
console.log(!!boolean); // true

// ! 한 개: 부정 연산자
console.log(!1); // false
// !! 두 개: 값을 boolean 타입으로 변환
console.log(!!1); // true
```

### (3) 숫자가 아닌 타입들을 숫자로 변환

```javascript
console.log(+false); // 0
console.log(+null); // 0
console.log(+''); // 0
console.log(+true); // 1
console.log(+'text'); // NaN: 문자열은 숫자로 변환 ❌
console.log(+undefined); // NaN: undefined은 숫자로 변환 ❌
```

<br/>

## 4. 할당 연산자 (Assignment Operators)

```javascript
// = 한 개: 할당과 선언
let a = 1;
a = a + 2;
console.log(a); // 3
a += 2; // a = a + 2; 축약 버전
console.log(a); // 5
a -= 2;
console.log(a); // 3
a *= 2;
console.log(a); // 6

a /= 2; // 2를 나누고 다시 할당
a %= 2; // 2로 나누고 다시 할당
a **= 2; // 2를 거듭제곱 후 다시 할당
```

<br/>

## 5. 증감 연산자 (Increment & Decrement Operators)

### (1) 증가 & 감소 연산자

```javascript
let a = 0;
console.log(a); // 0
a++; // a = a + 1;
console.log(a); // 1
a--; // a = a - 1;
console.log(a); // 0
```

### (2) 주의할 것

- `a++` 필요한 연산을 하고 → 그 뒤 값을 증가시킴
- `++a` 값을 먼저 증가시키고 → 필요한 연산을 함

```javascript
a = 0;
console.log(a++); // 0: 0을 먼저 연산&출력 후, 
console.log(a); // 1: 1을 증가시킴

let b = a++;
console.log(b); // 1: 1을 먼저 연산&출력 후,
console.log(a); // 2: 1을 증가시킴
```

<br/>

## 6. 비교 연산자 (Relational Operators)

```javascript
console.log(2 > 3); // false
console.log(2 < 3); // true
console.log(3 < 2); // false
console.log(3 > 2); // true
console.log(3 <= 2); // false
console.log(3 <= 3); // true
console.log(3 >= 3); // true
console.log(3 >= 2); // true
```

<br/>

## 7. 연산자 우선순위

[MDN 연산자 우선순위](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

```javascript
let a = 2;
let b = 3;
let result = ((a + b) * 4) / 5;
console.log(result); // 4
result = a++ + b * 4;
console.log(result); // 14
```

<br/>

## 8. 동등 비교 연산자 (Equality Operators)

- `==` 값이 같음
- `!=` 값이 다름
- `===` 값과 타입 모두 같음
- `!==` 값과 타입 모두 다름

```javascript
console.log(2 == 2); // true
console.log(2 != 2); // false
console.log(2 != 3); // true
console.log(2 == 3); // false
console.log(2 == '2'); // true
console.log(2 === '2'); // false(⚠️주의!)
console.log(true == 1); // true
console.log(true === 1); // false
console.log(false == 0); // true
console.log(false === 0); // false
console.clear();
```

```javascript
const obj1 = {
  name: 'js',
};
const obj2 = {
  name: 'js',
};

console.log(obj1 == obj2); // false(⚠️주의! 메모리 주소가 다름)
console.log(obj1 === obj2); // false
console.log(obj1.name == obj2.name); // true
console.log(obj1.name === obj2.name); // true

let obj3 = obj2;
console.log(obj3 == obj2); // true(동일한 메모리 주소를 가짐)
console.log(obj3 === obj2); // true
```
