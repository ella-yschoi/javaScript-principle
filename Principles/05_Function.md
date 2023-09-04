# What is Function?

<br/>

## 1. 함수

### (1) 함수란?

- 특정한 일을 수행하는 코드의 집합
- 한 군데에 잘 모아두면 유지보수성, 재사용성, 가독성 확보 가능

### (2) 함수 정의와 호출

- 프로그램 상에서 중복되는 일들은 함수 단위로 묶자
- 수행하는 일을 잘 나타내는 이름을 짓자
- 매개변수(인자)의 이름도 의미있게 짓자

#### 함수 정의

```javascript
function add(a, b) {
  return a + b;
};
```

- function: 함수 정의 키워드
- add: 수행하는 일을 잘 나타내는 함수 이름
- (a, b): 외부에서 전달받을 수 있는 매개변수(인자)를 통해 외부로부터 데이터를 받아와
- return: 특정한 일 수행 후, 처리한 값을 외부로 다시 반환

#### 함수 호출

```javascript
add(1, 2)
```

- 함수 이름과 함께 필요한 데이터를 전달하며 함수를 호출
- 인자 1과 2가 함수의 매개변수로 전달되어, 3이 리턴

### (3) 함수도 객체다

```javascript
add(1, 2)
```

- 함수를 정의하면, 함수의 object가 Heap이라는 메모리 공간에 할당됨
- 정보가 가득 담겨있기에 메모리 셀 하나가 아닌, 그 이상이 필요함
- 함수의 이름 add는 결국, 함수 object가 담겨있는 메모리 주소를 가지고 있음
- 따라서, `const sum = add;` 처럼 또다른 변수에 함수 이름 add를 할당하면 변수 sum은 동일하게 add 함수 정의 객체가 들어있는 곳을 가리킨다.
- 즉, **함수의 이름은 함수를 참조하고 있음 (=함수 객체의 주소를 가지고 있음)**

<br/>

## 2. 함수의 기본

### (1) 함수 기본 예제

```javascript
function add(num1, num2) {
  const result = num1 + num2;
}

function sum(a, b) {
  console.log('function'); // 밑에서 호출해주어야 출력됨
  return a + b;
}
const result = sum(1, 2); // 함수 호출
console.log(result); // 3
```

### (2) 함수의 재사용성과 유지보수성

```javascript
// 수정이 필요할 때, 아래와 같이 함수를 만들어 함수 한 곳에서만 수정하면
function fullName(firstName, lastName) {
  return `${firstName} ${lastName} ✋`;
}

// 아래의 두 개를 모두 수정하지 않아도 됨 → 유지보수성, 재사용성 확보
let lastNameKr = '최';
let firstNameKr = '연수';
console.log(fullName(firstNameKr, lastNameKr));

let lastNameEn = 'Choi';
let firstNameEn = 'Yeonsu';
console.log(fullName(firstNameEn, lastNameEn));
```

<br/>

## 3. 함수와 메모리

### (1) 중요한 원리

- 함수 이름 `sum` 자체는 함수를 가리키고 있는 변수 `add` 와 동일하다.
- 따라서 **함수 이름을 어딘가에 할당**한다는 것은, **함수를 가리키고 있는 메모리 주소를 복사해서 할당하는 것과 동일**하다.

### (2) 용례

```javascript
function add(a, b) {
  console.log(a); // 1
  console.log(b); // 2
  return a + b;
}
const sum = add;

// 아래와 같이 sum과 add 둘 다 함수처럼 사용 가능
console.log(sum(1, 2)); // 3
console.log(add(1, 2)); // 3
console.log(add(1)); // NaN (undefined이므로)
console.log(add()); // NaN (undefined+undefined이므로)
```

- 변수 sum에 함수 이름 add를 할당한다는 것은 add가 가리키는 메모리 주소를 그대로 가진다는 의미
- 따라서 sum과 add는 동일한 코드블럭을 가진다고 보면 됨

<br/>

## 4. 반환 (return)

### (1) return 명시

- return을 명시적으로 하지 않으면 자동으로 `undefined` 이 반환됨

```javascript
function add(a, b) {
  //return a + b;
  return undefined;
}
const result = add(1, 2);
console.log(result);
```

- return을 함수 중간에 하면 함수가 종료됨
- 조건에 맞지 않는 경우, 아래와 같이 함수 도입부분에서 함수를 일찍 종료함
- 즉, 함수에서 무거운 일들을 수행하기 이전에, 함수가 수행할 조건에 맞는지 아닌지, 유효한지 아닌지 먼저 판단함

### (2) 일찍 종료

```javascript
function print(num) {
  if (num < 0) {
    return; // 일찍 종료하는 역할
  }
  console.log(num); // 0이 아닌 경우에만 출력
}
print(12);
print(-12);
```

<br/>

## 5. 함수의 인자 (매개변수)

### (1) 매개 변수 기본값

- 매개변수의 기본값은 무조건 **undefined**
- 매개변수의 정보는 함수 내부에서 접근이 가능한 arguments 객체에 저장됨
- 매개변수 기본값 Default Parameters a = 1, b = 1 (undefined일 경우에만)

```javascript
function add(a = 1, b = 1) { // 기본값 지정
  console.log(a); // 1
  console.log(b); // 1
  console.log(arguments);
  console.log(arguments[1]);
  return a + b;
}
add();
```

### (2) Rest 매개변수 (Rest Parameters)

- 얼마나 많은 갯수의 인자가 전달될지 모를 때, 모든 것을 배열로 바꾸고자 할 때 사용

```javascript
function sum(a, b, ...numbers) {
  console.log(a); // 1
  console.log(b); // 2
  console.log(numbers); // 그 외 나머지 숫자들 [3, 4, 5, 6, 7, 8]
}
sum(1, 2, 3, 4, 5, 6, 7, 8);
```

<br/>

## 6. 함수를 표현하는 여러가지 방법

### (1) 함수 표현식

```javascript
let add = function (a, b) {
  return a + b;
};
console.log(add(1, 2));
```

- `const name = function () { }`
- 함수 이름을 변수 형태로 만들어, 함수를 할당함
- 표현식 사용 시, 함수 이름이 없는 무명함수를 할당

### (2) 화살표 함수

```javascript
add = (a, b) => a + b;
console.log(add(1, 2));
```

- `const name = () => { }`
- function 키워드 생략, 인자를 받아 => 화살표로 실행하고자 하는 코드를 가리킴
- 코드블럭 안에서 별도의 연산을 하지 않고 바로 리턴하는 경우라면, 위와 같이 ()와 return 생략 가능

### (3) 생성자 함수

```javascript
(function run() {
  console.log('🏃‍♂️');
})();
```

- `const object = new Function();`
- 함수를 ()로 묶으면 함수가 값으로 변환됨
- 함수를 정의하며 바로 호출하고 싶을 때 사용
- IIFE (Immediately-Invoked Function Expressions)라고 함

<br/>

## 7. 콜백함수

### (1) 용어 정리

#### 일급객체 (first-class object)

- 일반 객체처럼 모든 연산이 가능한 것
- 함수의 매개변수로 전달 가능
- 함수의 반환값으로 사용 가능
- 할당 명령문으로 사용 가능
- 동일 비교 가능

#### 일급함수 (first-class function)

- **함수가** 일반 객체처럼 모든 연산이 가능한 것
- 함수의 매개변수로 전달 가능
- 함수의 반환값으로 사용 가능
- 할당 명령문으로 사용 가능
- 동일 비교 가능

#### 고차함수 (Higher-order function)

- 인자로 함수를 받거나(콜백함수)
- 함수를 반환하는 함수

### (2) 용례

```javascript
const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

function calculator(a, b, action) {
  // calculator는 고차함수이며 숫자 a와 b를 받아서
  // 어떤 계산을 하고 싶은지 콜백함수 action의 명령을 받음
  if (a < 0 || b < 0) {
    return;
  }
  let result = action(a, b); // 전달된 action은 콜백함수
  console.log(result);
  return result;
}

// 어떤 함수를 호출할 것인지 선택
// add와 multiply는 호출이 아닌 이름만 전달했으므로
// 위에서 선언된 주소값을 콜백함수의 형태로 전달하는 것
calculator(1, 2, multiply); // 2

// 위의 조건문 안의 조건에 해당하므로 호출되지 않음
// 즉, 콜백함수는 고차함수 내부에서 필요한 순간에만 호출
calculator(-1, -1, add); // undefined
```

- 전달될 당시, 함수를 바로 호출해서 반환된 값을 전달 ❌
- 함수를 가리키고 있는 함수의 레퍼런스(참조값)가 전달됨 🆗
- 따라서 함수는 고차함수 calculator 안에서 필요한 순간에 나중에 호출됨

### (3) 정리하자면

- 콜백함수 action는 add와 multiply를 호출해서 호출된 값을 전달하는 것이 아니라, 함수의 참조값을 action으로 전달할 것이며, action은 어떠한 특정한 함수를 가리키고 있음
- 맨 위의 calculator 함수 내부에서는 정의하는 시점에서는 어떤 함수일지 모름
- 외부에서 calculator를 호출할 때마다 전달하는 함수에 따라 역할이 달라짐

### (4) 응용

#### 문제

- 주어진 숫자 만큼 0부터 순회하는 함수
- 순회하면서 주어진 특정한 일을 수행해야 함
- 순회하는 숫자를 모두 출력
- 순회하는 숫자의 두배값을 모두 출력

#### 풀이

```javascript
// 주어진 숫자 만큼 0부터 순회하는 함수
function iterate(max, action) {
  for (let i = 0; i < max; i++) {
    action(i);
  }
}

// 순회하는 숫자를 모두 출력
// ver.1
function log(num) {
  console.log(num);
}
iterate(3, log) // log를 호출
// ver.2
iterate(3, (num) => console.log(num));

// 순회하는 숫자의 두배값을 모두 출력
// ver.1
function doubleAndLog(num) {
  console.log(num * 2);
}
iterate(3, log * 2) // log를 호출
// ver.2
iterate(3, (num) => console.log(num * 2));

// setTimeout() 고차함수
setTimeout(() => {
  console.log('3초뒤 함수 실행');
}, 3000);
```

<br/>

## 8. 불변성 (Immutability, Unchangable)

### (1) 코드 작성 시 중요한 개념

- 함수 내부에서 외부로부터 주어진 인자의 값을 변경하면서 재할당은 ❌
- **⚠️상태 변경이 필요한 경우에는, 새로운 상태를(오브젝트, 값) 만들어서 반환해야 함**
- 원시값: 값 자체가 복사되어 전달되기에 큰 문제가 없지만
- 객체값: 참조에 의한 복사 즉, 메모리 주소가 전달되어 결국 동일한 오브젝트를 가리키기에 심각한 문제 발생

### (2) 용례

```javascript
function display(num) {
  num = 5; // ❌
  console.log(num);
}
const value = 4;
display(value); // 5
console.log(value); // 4
```

```javascript
function displayObj(obj) {
  obj.name = 'Chloe'; // ❌ 외부로부터 주어진 인자(오브젝트)를 내부에서 변경하는 것은 ❌
  console.log(obj);
}
const ella = { name: 'Ella' };
displayObj(ella);
console.log(ella);
```

```javascript
// 부득이한 경우, 함수 이름부터 변경하여 변경되었다는 느낌을 주기
// 반환할 때는 새로운 object를 만들어야 주기
function changeName(obj) {
  return { ...obj, name: 'Chloe' };
}
```
