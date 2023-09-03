# What is Control Flow Statement?

<br/>

## 1. 제어문 (Control Flow Statement)

### (1) 제어문이란?

- 코드의 흐름을 개발자가 제어할 수 있는 것

### (2) 종류

- 조건문, 반복문

<br/>

## 2. 조건문 (Conditional Statement)

### (1) 형식

- if(조건) { }
- if(조건) { } else { }
- if(조건1) { } else if(조건2) { } else { }

### (2) 용례

```javascript
let fruit = 'orange';
if (fruit === 'apple') {
  console.log('🍎');
} else if (fruit === 'orange') {
  console.log('🍊');
} else {
  console.log('🍌');
}

// if 안에는 true나 false로 평가될만한 표현식
if (2 < 1) { // false가 되는 것
  console.log('출력되면 안됨');
}
```

<br/>

## 3. 삼항 연산자 (Ternary Operator)

### (1) 형식

- 조건식 ? 참인 경우 : 거짓인 경우

### (2) 용례

#### 조건문 ver.

```javascript
let fruit = 'apple';
if (fruit === 'apple') {
  console.log('🍎');
} else {
  console.log('🍊');
}
```

#### 삼항 연산자 ver.1

```javascript
fruit === 'apple' ? console.log('🍎') : console.log('🍊');
```

#### 삼항 연산자 ver.2

```javascript
let emoji = fruit === 'apple' ? '🍎' : '🍊';
// 변수를 선언한 후, 값은 삼항 연산자를 사용할 것인데,
// fruit이 apple인지 아닌지에 따라 사과라면 🍎 할당, 아니라면 할당
```

<br/>

## 4. 스위치

### (1) 형식

- if else if else if else if ... else 처럼 if/else 반복하기보다,
- **정해진 범위 안의 값**에 대해 **특정한 일을 해야 하는 경우** switch 사용

### (2) 용례

#### if와 else ver.

```javascript
// 0: 월요일, 1: 화요일... 6: 일요일.. 코드가 지저분함
let day = 10;
let dayName;
if (day === 0) {
  dayName = '월요일';
} else if (day === 1) {
  dayName = '화요일';
} else if (day === 2) {
  dayName = '수요일';
} else if (day === 3) {
  dayName = '목요일';
} else if (day === 4) {
  dayName = '금요일';
} else if (day === 5) {
  dayName = '토요일';
} else if (day === 6) {
  dayName = '일요일';
}
```

#### switch ver.

```javascript
// case별로 나누어서 원하는 동작 수행하도록 만들기
let day = 6;
switch (day) {
  case 0:
    dayName = '월요일';
    break; // 그 다음 케이스를 확인하지 않도록 코드를 멈춤
  case 1:
    dayName = '화요일';
    break;
  case 2:
    dayName = '수요일';
    break;
  case 3:
    dayName = '목요일';
    break;
  case 4:
    dayName = '금요일';
    break;
  case 5:
    dayName = '토요일';
    break;
  case 6:
    dayName = '일요일';
    break;
  default: // 모든 조건에 해당되지 않는 경우 실행
    console.log('해당하는 요일이 없음!');
}
console.log(dayName);
// 만약 중간중간에 break를 사용하지 않으면 맨 마지막의 '일요일' 출력됨
```

#### switch & break ver.

```javascript
// break를 사용하지 않는 경우: 하나 이상의 여러 케이스가 동일한 코드를 수행하는 경우
let condition = 'bad'; // okay, good -> 좋음, bad -> 나쁨
let text;
switch (condition) {
  case 'okay': // 이거나 → 여기서는 break 걸지 않음
  case 'good': // 이라면
    text = '좋음'; // 동일하게 '좋음'으로 할당
    break; // → 여기서는 break 걸음
  case 'bad':
    text = '나쁨';
    break;
}
console.log(text);
```

<br/>

## 5. 반복문(Loop Statement) for

### (1) 형식

- for(변수선언문; 조건식; 증감식) { }

### (2) 실행 순서

- 변수선언문으로 변수 초기화
- 조건식의 값이 참이면  { } 코드블럭을 수행
- 증감식을 수행
- 조건식이 거짓이 될때까지 2번과 3번을 반복함

### (3) 용례

#### 일반 for문

```javascript
// 여기서 변수는 for문을 얼만큼 반복할 것인지 기억하는 변수
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

#### 중첩 for문

```javascript
for (let i = 0; i < 5; i++) { // (1)
  for (let j = 0; j < 10; j+2) { // (2)
    console.log(i, j);
    // (1) for문 조건에 맞으면, 새로운 (2) for문이 도는게 무한정 반복
  }
}
```

#### ⚠️ 주의: 무한 루프

```javascript
for (;;) {
  // 조건이 없거나, 조건이 영원히 true인 경우
  // 따라서 조건은 언젠가 끝날 수 있도록 코딩 필요
}
```

#### 반복문 제어(1): break;

```javascript
for (let i = 0; i < 20; i++) {
  if (i === 10) {
    console.log('드디어 i가 10임');
    break;
    // i가 10이 되었을 때 for loop를 그만둠
  }
  console.log(i); // 0 1 2 3 4 5 6 7 8 9 드디어 i가 10임
}
```

#### 반복문 제어(2): continue;

```javascript
for (let i = 0; i < 20; i++) {
  if (i === 10) {
    continue;
    // 아래에 어떤 코드가 있더라도 그 코드는 무시되며 다시 증감되고,
    // 조건이 맞으면 다음 증감으로 넘어감
    console.log('드디어 i가 10임');
  }
  console.log(i); // 0 1 2 3 4 5 6 7 8 9 11 12 ... 19
}

for (let i = 0; i < 20; i++) {
  if (i === 10) {
    console.log('드디어 i가 10임');
    continue;
  }
  console.log(i); // 0 1 2 3 4 5 6 7 8 9 드디어 i가 10임 11 ... 19
}
```

<br/>

## 6. 반복문(Loop Statement) while

### (1) 형식

- while(조건) {}
- 조건이 false가 될때까지 {} 코드를 반복 실행

### (2) 용례

#### 일반 while문

```javascript
let num = 5;
while (num >= 0) {
  console.log(num); // 5 4 3 2 1 0
  num--; // 하나 줄이고 다시 while문으로 감
}
```

#### while & break문

```javascript
let isActive = false;
let i = 0;
while (isActive) { // isActive가 false가 될 때까지 계속 실행
  console.log('아직 살아있어');
  if (i === 1000) {
    break;
    // break 사용으로, i가 1000이 되면 while문 전부 실행 멈춤
    // 만약 이번 순간에만 아래 코드를 수행하지 않고 다음으로 넘어가고자 한다면, continue 사용
  }
  i++; // 이게 없으면 i는 계속 0으로 머물러 있을 것이기에 무한루프
}
```

#### do while 문

```javascript
do { // 일단 실행하고 나서
  console.log('do-while 아직 살아있어');
} while (isActive); // 조건을 검사한다
```

### (3) 정리하자면

- while: 조건이 맞을 때 실행
- do while: 꼭 한번은 실행

<br/>

## 7. 제어문에서 자주 쓰이는 논리연산자 (Logical operator)

### (1) 종류

- && 그리고
- || 또는
- ! 부정
- !! boolean 값으로 변환

### (2) 용례

```javascript
let num = 8;
if (num >= 0 || num > 20) { // 둘 중 하나만 true여도 출력
  console.log('👍');
}
if (num != 9) {
  console.log('🙏');
}
```

```javascript
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false

console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false

console.log(!'text'); // false
console.log(!!'text'); // true
```
