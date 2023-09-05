# What is Object?

<br/>

## 1. 객체

### (1) 객체란?

- 서로 연관있는 속성(데이터 및 프로퍼티)과 행동(함수 및 메소드)을 묶어주기 위해 사용됨
- 따라서 밀접하게 관련된 상태와 행동을 객체로 묶어야 함

### (2) 객체 리터럴 (Object literal)

```javascript
let apple = {
  name: 'apple',
  'hello-bye': '✋', // '-'로 여러 문자열 연결 가능
  0: 1, // 숫자 입력 가능
  ['hello-bye1']: '✋', // 대괄호 안에 문자열 입력 가능
  helloBye: '✋' // 다만, 웬만하면 camelCase로 입력
};
```

- `{ key: value }`
- `new Object()`
- `Object.create()`
- key - 문자, 숫자, 문자열, 심볼
- value - 원시값, 객체 (함수)

### (3) 속성, 데이터에 접근하는 방법

```javascript
// (1) 마침표 표기법 dot notation
apple.name; 

// (2) 대괄호 표기법 bracket notation
console.log(apple['hello-bye1']);
apple['name'];

// 속성 추가
apple.emoji = '🍎';
console.log(apple.emoji);
console.log(apple['emoji']);

// 속성 삭제
delete apple.emoji;
console.log(apple);
```

<br/>

## 2. 객체 동적으로 접근하기

[MDN 객체로 작업하기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects)

```javascript
const obj = {
  name: '엘라',
  age: 20,
};

// 코딩하는 시점에, 정적으로 접근이 확정될 때 아래와 같이 사용
obj.name;
obj.age;

// 동적으로 속성에 접근하고 싶을 때, 대괄호 표기법 사용
function getValue(obj, key) {
  // return obj.key; 는 동작하지 않음
  // object 안에 key라는 이름의 key가 없기 때문 (e.g. key: 20)

  // 따로 정해져 있는게 아니라, 전달되는 문자열에 따라 key의 값을 찾을 것
  return obj[key];
}
// name이라는 obj의 key값을 리턴
console.log(getValue(obj, 'name')); // 엘라

// 동적으로 obj 추가
function addKey(obj, key, value) {
  obj[key] = value;
}
addKey(obj, 'job', 'engineer');
console.log(obj);

// 동적으로 obj 삭제
function deleteKey(obj, key) {
  delete object[key];
}
```

<br/>

## 3. 객체 축약 버전

```javascript
const x = 0;
const y = 0;

// key 이름과 참고하는 변수의 이름이 동일한 경우, 간략하게 작성 가능
const coordinate = { x, y }; // 원래 { x: x, y: y }; 임
console.log(coordinate); // { x: 0, y: 0 }

function makeObj(name, age) {
  return {
    // 기존: name: name,
    // 기존: age: age,
    name,
    age,
  };
}
```

<br/>

## 4. 객체 안의 함수

```javascript
const apple = {
  name: 'apple',
  display: function () { // 값은 함수

    // 객체 안에서 자기 자신의 데이터에 접근할 때는 'this.key' 형태로 작성
    console.log(`${this.name}: 🍎`);
  },
};

apple.display();
```

<br/>

## 5. 생성자 함수

### (1) 생성자 함수란?

- 특정한 **템플릿**에 맞게, 객체를 쉽게 만들 수 있는 함수
- 양식을 한번만 만들어두면, new 라는 키워드로 새로운 객체들 생성 가능

### (2) 용례

```javascript
function Fruit(name, emoji) { // 첫 번째 문자를 대문자로 입력
  this.name = name; // 이름은 전달받은 name 이라는 값을 할당
  this.emoji = emoji;

  this.display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
  // return this; // 생략 가능
  // 생성자 함수에서는 자동으로 return이 생성
}

const apple = new Fruit('apple', '🍎');
const orange = new Fruit('orange', '🍊');

console.log(apple);
console.log(orange);
console.log(apple.name);
console.log(apple.emoji);
apple.display();
```
