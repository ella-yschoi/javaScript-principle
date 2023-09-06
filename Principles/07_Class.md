# What is Class?

<br/>

## 1. 클래스의 기본

### (1) 클래스란

- 객체를 생성할 수 있는 일종의 **템플릿**
- 원래는 프로토타입을 기반으로 했지만, ES6부터 class 기반으로 **객체 지향 프로그래밍**을 할 수 있음
- 즉, 절차적으로 코드를 작성하기보다, 서로 밀접하게 연관되어 있는 것들을 객체로 구성하며, 객체끼리 서로 호환 가능하도록 함

### (2) 용어 정리

- 인스턴스(Instance): 클래스를 이용해 만들어진 객체
- 객체는 그냥 만들어진 객체고, 인스턴스는 어떤 클래스를 통해 만들어진 객체

### (3) 용례

```javascript
// 생성자 함수의 function 대신 class로 작성
// 중괄호로 묶고 생성자 constructor 지정
class Fruit {
  // 생성자: new 키워드로 객체를 생성할때 호출되는 함수
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }
  // 생성자 밖에서 함수를 정의
  display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
}

// apple은 Fruit 클래스의 인스턴스임
const apple = new Fruit('apple', '🍎');
// orange은 Fruit 클래스의 인스턴스임
const orange = new Fruit('orange', '🍊');

console.log(apple);
console.log(orange);
console.log(apple.name);
console.log(apple.emoji);
apple.display();

// obj는 객체고, 그 어떤 클래스의 인스턴스도 아님
// 객체는 그냥 만들어진 객체고, 인스턴스는 어떤 클래스를 통해 만들어진 객체
const obj = { name: 'ella' };
```

<br/>

## 2. 재사용성 높이기

### (1) 인스턴스 vs. 클래스

- 인스턴스 레벨의 프로퍼티와 메소드는 각각 객체마다 서로 다른 데이터를 가지고 있지만, 모든 객체마다 동일하게 참조해야 하는 속성이나 함수가 있다면
- `static` 키워드로 클래스 레벨의 프로퍼티와 메소드를 만들면 됨
- 이는 만들어진 인스턴스에 포함되지 않고, 클래스에 그대로 남아있게 됨
- 즉, 클래스에 딱 한번만 만들어주고, 각각의 인스턴스에는 들어있지 않기에 클래스에 한번만 정의해서 재사용 가능

```javascript
// static 정적 프로퍼티, 메서드
class Fruit {
  static MAX_FRUITS = 4;
  // 생성자: new 키워드로 객체를 생성할때 호출되는 함수
  constructor(name, emoji) {
    this.name = name;
    this.emoji = emoji;
  }

  // 클래스 레벨의 메서드
  static makeRandomFruit() {
    // 클래스 레벨의 메서드에서는 아무것도 채워지지 않은 템플릿이기에 this 참조 불가
    return new Fruit('banana', '🍌');
  }

  // 인스턴스 레벨의 메서드
  display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
}

const banana = Fruit.makeRandomFruit();
console.log(banana);
console.log(Fruit.MAX_FRUITS);
// apple은 Fruit 클래스의 인스턴스이다.
const apple = new Fruit('apple', '🍎');
// orange은 Fruit 클래스의 인스턴스이다.
const orange = new Fruit('orange', '🍊');

console.log(apple);
console.log(orange);
console.log(apple.name);
console.log(apple.emoji);
apple.display();

// static 예시: object 굳이 만들지 않아도 됨
Math.pow();
Number.isFinite(1);
```

<br/>

## 3. 필드

### (1) 접근제어자 (캡슐화)

- 한번 만들어진 다음에 외부에서 변경이 불가능하도록 만들고 싶을 때 사용
- 즉, 내부상으로 필요한 데이터를 외부에서 보이지 않도록 숨겨놓는 것
- private(#), public(디폴트 상태), protected

### (2) 용례

```javascript
class Fruit {
  #name;
  #emoji;
  #type = '과일';
  constructor(name, emoji) {
    this.#name = name;
    this.#emoji = emoji;
  }
  #display = () => {
    console.log(`${this.#name}: ${this.#emoji}`);
  };
}

const apple = new Fruit('apple', '🍎');
//apple.#name = '오렌지'; // #field는 외부에서 접근이 불가능
console.log(apple);
```

<br/>

## 4. 접근자 프로퍼티 (Accessor Property) - 세터와 게터

### (1) 접근자 프로퍼티란?

- 클래스의 상태처럼 보이기는 하나, 실제로는 함수를 말함

### (2) 용례

```javascript
class Student {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  // 단순히 first name과 last name 이라는 속성을 조합해서 보여주는 것
  // 때문에 속성의 한 부분이라고 간주되는 것들을 함수로 만들어야 할 때 get 사용
  get fullName() { // 접근자 프로퍼티로 함수 호출
    return `${this.lastName} ${this.firstName}`;
  }

  // set은 할당할 때 함수가 호출되며, 할당하는 value도 받아올 수 있음
  set fullName(value) {
    console.log('set', value); // 인자로 전달됨
  }
}

const student = new Student('연수', '최');
student.firstName = '엘라';
console.log(student.firstName);
console.log(student.fullName); // get 호출
student.fullName = '김철수'; // 할당하면 set 호출되어 위에 인자로 전달됨
```

<br/>

## 5. 상속

### (1) 상속을 받지 않는 경우

- 공통의 행동들이 있기에 중복 발생

```javascript
class Tiger {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }
  sleep() {
    console.log('잔다');
  }
}

class Dog {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }
  sleep() {
    console.log('잔다');
  }
  play() {
    console.log('놀자아~!');
  }
}
```

### (2) 상속을 받는 경우

- 위와 같이 중복으로 쓰지 않고, 공통의 템플릿을 만들어 줌

```javascript
class Animal {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹는다');
  }
  sleep() {
    console.log('잔다');
  }
}

// (1) Tiger는 extends를 통해 그냥 Animal이라는 템플릿 가져옴
class Tiger extends Animal {}
const tiger = new Tiger('노랑');
console.log(tiger); // Tiger {color: '노랑'}
tiger.sleep(); // 잔다
tiger.eat(); // 먹는다

// (2) Dog는 상속 + 클래스 하나 더 추가
class Dog extends Animal {
  // 자식 클래스에서 constructor 정의하면
  constructor(color, ownerName) {
    // super로 부모에서 만들었던 것 다 상속받고
    super(color);
    // this로 자기 자신의 것을 받아옴
    this.ownerName = ownerName;
  }
  // dog는 자식 클래스 하나 더 있어서 추가
  play() {
    console.log('논다');
  }

  // 오버라이딩(overriding): 부모의 행동을 내 행동으로 덮어씌우기
  eat() {
    // 부모의 행동을 유지하면서 추가로 무언가를 만든다
    super.eat(); // 먹자
    console.log('강아지가 먹는다'); // 강아지가 먹는다
  }
}
const dog = new Dog('초록', '엘라');
console.log(dog);
dog.sleep();
dog.eat();
dog.play();
```
