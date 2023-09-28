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

- object 동결: 추가 ❌, 삭제 ❌, 쓰기 ❌, 속성 재정의 ❌ (단, *얕은 동결)
- 얕은 동결이란, 아래 예시 코드에서 dog 안에 있는 ella가 중첩되어 있는데, `{ name: '시바견', emoji: '🐶', owner: ella }` 까지만 freeze되고, `{ name: '엘라' }` 까지는 freeze 안됨

```javascript
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

- object 밀봉: 수정 ✅, 추가 ❌, 삭제 ❌, 속성 재정의 ❌

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

- object 확장 금지: 수정 ✅, 삭제 ✅, 추가 ❌

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
