# What is Closure?

<br/>

## 1. Closure란

[MDN Closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

내부 함수에서 외부 함수에 있는 상태에 접근할 수 있는 권한을 주는 것. 즉, 자신이 생성될 때의 렉시컬 환경을 기억하는 함수

## 2. 용례

```javascript
const text = 'hello';
function func() {
  console.log(text);
}
func();

function outer() {
  const x = 0;
  function inner() {
    console.log(`inside inner: ${x}`); // 0
  }
  return inner;
}
const func1 = outer();
func1(); // 0

// 클로저에 의해 함수 뿐만 아니라
// 함수 외부에 있는 outer 렉시컬 환경도 함께 묶여서 클로저로 반환됨
// 따라서 inner에서도 여전히 outer에 있는 변수에 접근 가능
```

<br/>

## 2. Closure 활용하기

### (1) Closure의 기능

- 내부 정보를 은닉하고, 공개 함수(public, 외부)를 통한 데이터 조작을 위해 캡슐화와 정보 은닉
- 클래스의 private 필드 또는 메소드를 사용하는 효과와 동일함

```javascript
// closure의 특징을 이용해 함수로 은닉하고자 하는 데이터 상태를 감추고
// 오직 public 함수만을 통해서 내부 데이터를 조작 가능
function makeCounter() {
  let count = 0;
  function increase() {
    count++;
    console.log(count);
  }
  return increase;
}
const increase = makeCounter();
increase(); // 1
increase(); // 2
increase(); // 3

// 이제는 클래스를 사용하면 됨
class Counter {
  // private 필드를 0으로 초기화
  #count = 0;
  // 내부에서만 count 증가시킴
  increase() {
    this.#count++;
    console.log(this.#count);
  }
}
const counter = new Counter();
counter.increase();
```

<br/>

## 3. 응용

```javascript
function loop() {
  const array = [];
  // var과 let의 차이점
  // var는 블록 스코프가 없고, 함수 스코프만 있음
  for (var i = 0; i < 5; i++) {
    array.push(function () {
      console.log(i);
    });
  }
  return array;
}

const array = loop();
array.forEach((func) => func());
```
