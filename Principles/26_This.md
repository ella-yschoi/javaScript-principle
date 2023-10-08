# What is ?

<br/>

## 1. This

### (1) This란?

- 문맥에 따라 '이것'이 가리키는 것이 달라짐
- 앞으로 만들어질 인스턴스 혹은 지금의 객체 자기 자신을 가리키는 용도로 사용됨
- 런타임 상에서 this 바인딩이 동적으로 결정됨

### (2) 글로벌 컨텍스트의 this

- 브라우저: window
- 노드: 모듈

```javascript
'use strict';

const x = 0;
module.exports.x = x;
console.log(this);
console.log(globalThis);
// 전역 객체 출력
// globalThis.setTimeout()
// setTimeout()
console.clear();
```

### (3) 함수 내부에서의 this

- 엄격 모드에서는 undefined
- 느슨한 모드에서는 globalThis

```javascript
function fun() {
  console.log(this);
}
fun();
```

### (4) 생성자 함수 또는 클래스에서의 this

```javascript
// 앞으로 생성될 인스턴스 자체를 가리킴
function Cat(name) {
  this.name = name;
  this.printName = function () {
    console.log(this.name);
  };
}
const cat1 = new Cat('냐옹');
const cat2 = new Cat('미야옹');
cat1.printName();
cat2.printName();
```

<br/>

## 2. Binding

### (1) This 바인딩

- 자바, C#, C++ 대부분의 객체지향 프로그래밍 언어에서는 this는 항상 자신의 인스턴스 자체를 가리킴 즉, 정적으로 인스턴스가 만들어지는 시점에 this가 결정됨.
- 하지만 자바스크립트에서는 누가 호출하냐에 따라서 this가 달라짐. 즉, this는 호출하는 사람(caller)에 의해 동적으로 결정됨.

### (2) 용례

```javascript
function Cat(name) {
  this.name = name;
  this.printName = function () {
    console.log(`고양이 이름: ${this.name}`);
  };
}

function Dog(name) {
  this.name = name;
  this.printName = function () {
    console.log(`강아지 이름: ${this.name}`);
  };
}

const cat = new Cat('냐옹');
const dog = new Dog('멍멍');
cat.printName();
dog.printName();

dog.printName = cat.printName;
dog.printName();
cat.printName();

function printOnMonitor(printName) {
  console.log('모니터 준비, 전달된 콜백 실행');
  printName();
}

printOnMonitor(cat.printName);
```

<br/>

## 3. Binding 응용

```javascript
function Cat(name) {
  this.name = name;
  // 2. arrow 함수를 사용: arrow 함수는 렉시컬 환경에서의 this를 기억함
  // 화살표 함수 밖에서 제일 근접한 스코프의 this를 가리킴
  this.printName = () => {
    console.log(`고양이 이름: ${this.name}`);
  };
  // 1. bind 함수를 이용해서 수동적으로 바인딩 해주기
  // this.printName = this.printName.bind(this);
}

function Dog(name) {
  this.name = name;
  this.printName = function () {
    console.log(`강아지 이름: ${this.name}`);
  };
}

const cat = new Cat('냐옹');
const dog = new Dog('멍멍');
cat.printName();
dog.printName();

dog.printName = cat.printName;
dog.printName();
cat.printName();

function printOnMonitor(printName) {
  console.log('모니터 준비, 전달된 콜백 실행');
  printName();
}

printOnMonitor(cat.printName);
```

<br/>

## 4. Arrow Function

### (1) 화살표 함수란

- 함수처럼 사용, 생성자 함수로 사용 (클래스)
- 하지만, 이걸 위해서 불필요한 무거운 프로토타입(많은 데이터를 담고 있는 객체) 생성됨

### (2) 화살표 함수 특징

- 문법이 깔끔함
- 생성자 함수로 사용이 불가능 (무거운 프로토타입을 만들지 않음)
- 함수 자체 arguments
- this에 대한 바인딩이 정적으로 결정됨
- 함수에서 제일 근접한 상위 스코프의 this에 정적으로 바인딩됨

### (3) 용례

```javascript
const dog = {
  name: 'Dog',
  play: function () {
    // 💩
    console.log('논다');
  },
};
dog.play();
const obj = new dog.play(); // 좋지 않음
console.log(obj);

// ES6
const cat = {
  name: 'cat',
  play() {
    // 객체의 메서드 (오브젝트에 속한 함수)
    console.log('냐옹');
  },
};
cat.play();
// const obj1 = new cat.play(); // 생성자 함수로 사용 ❌

function sum(a, b) {
  console.log(arguments);
}
sum(1, 2);

const add = (a, b) => {
  console.log(arguments); // arrow 함수 외부의 arguments를 참조만 함
};
add(1, 2);

const printArrow = () => {
  console.log(this);
};
printArrow();
cat.printArrow = printArrow;
cat.printArrow();
```
