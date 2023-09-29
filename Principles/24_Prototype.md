# What is Prototype?

<br/>

## 1. Prototype(프로토타입)

### (1) 의미

- 원형, 공통적인 특징, 초기 단계 예제 등 완성되기 전 대략적인 스케치
- 자바스크립트는 프로토타입을 기반으로 한 객체 지향 프로그래밍 언어

### (2) 특징

- 자바스크립트에서 모든 객체들은 Object라는 [[Prototype]]을 가지고 있음
- 즉, 모든 객체는 **객체간 상속을 위해** 내부에 숨겨진 [[Prototype]]을 가지고 있음
- Array과 같은 객체 또한 최종적으로는 object prototype을 상속받고 있기에 toString(), valueOf()와 같은 공통적인 함수 사용 가능한 것임
- 외부에서는 직접 접근 불가하여 `_proto_` 등으로 접근해야 함
- 모든 자바스크립트 객체들은 개별적인 object prototype을 상속하는게 아니라, 동일한 단 하나의 object prototype을 상속받음
- 즉, 객체 간 상속의 연결 고리는 **프로토타입 체인으로 연결**되어 있음

```javascript
const obj1 = {};
const obj2 = {};

obj1._proto_ === obj2._proto_ // true
```

### (3) 용례

```javascript
const dog = { name: '시바견', emoji: '🐶' };

// key만 찾기
console.log(Object.keys(dog));
// value를 찾기
console.log(Object.values(dog));
// key와 value를 페어로 받기
console.log(Object.entries(dog));

// in으로 객체 안에 특정 key 찾기
console.log('name' in dog);
// hasOwnProperty 함수로 특정 key 찾기
console.log(dog.hasOwnProperty('name'));

// 오브젝트의 각각의 프로퍼티는 프로퍼티 디스크립터라고 하는 객체로 저장됨
const descriptors = Object.getOwnPropertyDescriptors(dog);
console.log(descriptors);

// 어떤 객체에 있는 하나의 key만 받아오기
const desc = Object.getOwnPropertyDescriptor(dog, 'name');
console.log(desc);

// 수정 가능
Object.defineProperty(dog, 'name', {
  value: '멍멍',
  writable: false,
  enumerable: false,
  configurable: false,
});

// 열거 및 수정 불가
console.log(dog.name);
console.log(Object.keys(dog));
delete dog.name;
console.log(dog.name);
```

```javascript
// 전체 객체에서 모든 Properties들을 정의
// 특정 key가 수정되면 안되거나 열거 및 수정되면 안되는 경우 설정 가능
const student = {};
Object.defineProperties(student, {
  firstName: {
    value: '연수',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: '최',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  fullName: {
    get() {
      return `${lastName} ${firstName}`;
    },
    set(name) {
      [this.lastName, this.firstName] = name.split(' ');
    },
    configurable: true,
  },
});
console.log(student);
```

<br/>

## 2. 객체 불변성

### (1) Object.freeze

object 동결: 추가 ❌, 삭제 ❌, 쓰기 ❌, 속성 재정의 ❌ (단, *얕은 동결)

```javascript
// 얕은 동결이란, 아래 예시 코드에서 dog 안에 있는 ella가 중첩되어 있는데,
// { name: '시바견', emoji: '🐶', owner: ella } 까지만 freeze되고, { name: '엘라' } 까지는 freeze 안됨
const ella = { name: '엘라' };
const dog = { name: '시바견', emoji: '🐶', owner: ella };
Object.freeze(dog);
dog.name = '멍멍';
console.log(dog);
dog.age = 4;
console.log(dog);
delete dog.name;
console.log(dog);

// 얕은 동결로 인해 ella.name은 동결되지 않아 ella 객체를 참조하는 dog.owner가 변경됨
// 즉, 참조하는 객체까지는 freeze가 되지는 않음
ella.name = '클로이';
console.log(dog); // {name: '시바견', emoji: '🐶', owner: '클로이'}
```

### (2) Object.seal value

object 밀봉: 수정 ✅, 추가 ❌, 삭제 ❌, 속성 재정의 ❌

```javascript
// 객체 복사 가능 (spread syntax 사용 or assign static function 사용)
const cat = { ...dog };
// Object.assign(cat, dog);

Object.seal(cat);
console.log(cat);
cat.name = '냐옹';
console.log(cat);
delete cat.emoji;
console.log(cat);

// 여부 확인
console.log(Object.isFrozen(dog));
console.log(Object.isSealed(cat));
```

### (3) preventExtensions

object 확장 금지: 수정 ✅, 삭제 ✅, 추가 ❌

```javascript
const tiger = { name: '어흥' };
Object.preventExtensions(tiger);
console.log(Object.isExtensible(tiger)); // false

tiger.name = '호랑';
console.log(tiger);
delete tiger.name;
console.log(tiger);
tiger.age = 1;
console.log(tiger);
```

<br/>

## 3. 프로토타입 레벨 함수

```javascript
// const dog1 = { name: '뭉치', emoji: '🐶' };
// const dog2 = { name: '코코', emoji: '🐩' };

function Dog(name, emoji) {
  this.name = name;
  this.emoji = emoji;
  // 인스턴스 레벨의 함수: 메모리 낭비
  /* this.printName = () => {
    console.log(`${this.name} ${this.emoji}`);
  }; */
}

// 프로토타입 레벨의 함수: 메모리 절약
Dog.prototype.printName = function () {
  console.log(`${this.name} ${this.emoji}`);
};
const dog1 = new Dog('뭉치', '🐶');
const dog2 = new Dog('코코', '🐩');
console.log(dog1, dog2);
dog1.printName();
dog2.printName();

// 오버라이딩
// 인스턴스 레벨에서(자식 레벨) 동일한 이름으로 함수를 재정의(오버라이딩) 하면
// 프로토타입 레벨의(부모 레벨) 함수의 프로퍼티는 가려진다(섀도잉 됨)
dog1.printName = function () {
  console.log('안녕!!');
};
dog1.printName();

// 정적 레벨
Dog.hello = () => {
  console.log('Hello!');
};
Dog.hello();
Dog.MAX_AGE = 20;
```

<br/>

## 4. 프로토타입을 이용한 상속

```javascript
function Animal(name, emoji) {
  this.name = name;
  this.emoji = emoji;
}

Animal.prototype.printName = function () {
  console.log(`${this.name} ${this.emoji}`);
};

function Dog(name, emoji, owner) {
  // super(name, emoji) 대신 .call로 생성자 함수 호출
  Animal.call(this, name, emoji);
  this.owner = owner;
}
// Dog.prototype = Object.create(Object.prototype);
Dog.prototype = Object.create(Animal.prototype);

Dog.prototype.play = () => {
  console.log('같이놀자');
};

function Tiger(name, emoji) {
  Animal.call(this, name, emoji);
}

Tiger.prototype = Object.create(Animal.prototype);
Tiger.prototype.hunt = () => {
  console.log('사냥하자');
};

const dog1 = new Dog('멍멍', '🐶', '엘라');
dog1.play();
dog1.printName();
const tiger1 = new Tiger('어흥', '🐯');
tiger1.printName();
tiger1.hunt();

// 상속도 확인하는 방법 (instanceof): 어떤 프로토타입을 따르는가
console.log(dog1 instanceof Dog); // true
console.log(dog1 instanceof Animal); // true
console.log(dog1 instanceof Tiger); // false
console.log(tiger1 instanceof Dog); // false
console.log(tiger1 instanceof Animal); // true
console.log(tiger1 instanceof Tiger); // true
```

<br/>

## 5. Mixin

object는 단 하나의 prototype을 가리킬 수 있다 (부모는 단 하나, 다중 상속 X), 하지만 여러 개의 함수들을 상속하고 싶다면 Mixin 사용

```javascript
const play = {
  play: function () {
    console.log(`${this.name} 놀아요`);
  },
};

const sleep = {
  sleep: function () {
    console.log(`${this.name} 자요`);
  },
};

function Dog(name) {
  this.name = name;
}

// static 함수 assign으로 prototype에 객체 할당해서 믹스
Object.assign(Dog.prototype, play, sleep);
const dog = new Dog('멍멍');
console.log(dog);
dog.play(); // 멍멍 놀아요
dog.sleep(); // 멍멍 자요

// 클래스에도 활용 가능
class Animal {}
class Tiger extends Animal {
  constructor(name) {
    super();
    this.name = name;
  }
}

// Tiger(class) prototype에 play와 sleep 믹스
Object.assign(Tiger.prototype, play, sleep);
const tiger = new Tiger('어흥');
tiger.play(); // 어흥 놀아요
tiger.sleep(); // 어흥 자요
```

<br/>

## 6. 클래스를 베이스로한 객체 지향 프로그래밍

```javascript
class Animal {
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }
  printName() {
    console.log(`${this.name} ${this.emoji}`);
  }
}

class Dog extends Animal {
  play() {
    console.log('같이놀자');
  }
}
class Tiger extends Animal {
  hunt() {
    console.log(`사냥하자`);
  }
}

const dog1 = new Dog('뭉치', '🐶');
const tiger1 = new Tiger('어흥', '🐯');
dog1.printName();
tiger1.printName();
dog1.play();
tiger1.hunt();

console.log(dog1 instanceof Dog);
console.log(dog1 instanceof Animal);
console.log(dog1 instanceof Tiger);
console.log(tiger1 instanceof Tiger);
```
