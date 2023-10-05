# What is Closure?

<br/>

## 1. Closure

```javascript
const text = 'hello';
function func() {
  console.log(text);
}
func();

function outer() {
  const x = 0;
  function inner() {
    console.log(`inside inner: ${x}`);
  }
  return inner;
}
const func1 = outer();
func1();
```

<br/>

## 2. Why

### (1) 설명

- 내부 정보를 은닉하고, 공개 함수(public, 외부)를 통한 데이터 조작을 위해 캡슐화와 정보은닉
- 클래스 private 필드 또는 메소드를 사용하는 효과와 동일함

```javascript

function makeCounter() {
  let count = 0;
  function increase() {
    count++;
    console.log(count);
  }
  return increase;
}
const increase = makeCounter();
increase();
increase();
increase();

class Counter {
  #count = 0;
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
