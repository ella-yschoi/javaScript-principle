# What is Iterator?

<br/>

## 1. 이터러블, 이터레이션

### (1) 이터레이션(Iteration)이란?

> [MDN Iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

#### Iteration Protocol

- **반복/순회가 가능한 규격/약속/인터페이스**
- Iteration Protocol을 따르는 객체는 `for...of`, `spread` 연산자 사용 가능
- Iteration Protocol을 따르는 자료구조: `Array`, `String`, `Map`, `Set`

#### Iterable Protocol을 따른다는 것

- 어떤 객체든지 순회 가능하기 위해서는 Iterable Protocol(Interface)을 따라야 함
- 순회가 가능한 object가 되려면 내 object 안에 `Symbol.iterator()`를 만들고, 그 함수를 호출하면 Iterator Protocol을 따르는 객체를 리턴해야 함
- Iterator Protocol은 `next()`가 있어서 다음 값을 계속 리턴하도록 만들면 됨

### Iterator 정의

```javascript
interface Array<T> {
  [Symbol.iterator()]: IterableIterator<T>;
}
```

- Array 라는 Interface는 iterator 규격사항을 따르며, `Symbol.iterator()`를 호출하면 Iterable한 Iterator를 리턴한다. 따라서 Array는 Iteration Protocol을 따라간다.
- 따라서 `for..of` 연산자를 사용했을 때 Array 안에 있는 `Symbol.iterator()`를 호출해서 반환된 Iterator를 가지고 하나하나씩 `next()`를 호출하면서 값을 순회한다.

```javascript
const array = [1, 2, 3];
console.log(array.values());
console.log(array.entries());
console.log(array.keys());

// for...of를 사용해도 되지만, 수동적으로 iterator.next() 사용 가능
const iterator = array.values();
console.log(iterator.next().value); // 1
console.log(iterator.next().done); // false
console.log(iterator.next().value); // 3
console.log(iterator.next().value); // undefined
console.log(iterator.next().done); // true

// for...of의 내부적인 작동 원리
const iterator = array.values();
// (1) done이 true일 때 반복을 끝냄
while (true) {
  // (2) 반복문 돌면서 iterator에 있는 item을 next로 받아서
  const item = iterator.next();
  // (3) done이 true이면 즉, iteration 끝나면 while문 종료
  if (item.done) break; 
  // (4) 아니면, item 계속 출력해서 value를 확인하자
  console.log(item.value);
  // 1
  // 2
  // 3
}

for (let item of array) {
  console.log(item); 
  // 1
  // 2
  // 3
}

const obj = { id: 123, name: 'Ella' };
for (const key in obj) {
  console.log(key);
  // 1
  // 2
  // 3
  // id
  // name
}
```

<br/>

## 2. 응용

### (1) Quiz.1

- 0부터 10 이하까지 숫자의 2배를 순회하는 이터레이터(반복자) 만들기

```javascript
const multiple = {
  [Symbol.iterator]: () => {
    const max = 10;
    let num = 0;
    // next 함수 호출 시 iterator 반환
    return {
      next() {
        // next()를 호출할 때마다 value & done 이라는 키를 가진 객체 반환
        // value로 새로운 값을 만들고 & done으로 true/false 여부 확인
        return { value: num++ * 2, done: num > max };
      },
    };
  },
};

console.clear();
for (const num of multiple) {
  console.log(num);
}
```

### (2) Quiz.1 함수 버전

```javascript
function makeIterable(initialValue, maxValue, callback) {
  return {
    [Symbol.iterator]: () => {
      const max = maxValue;
      let num = initialValue;
      return {
        next() {
          // 콜백함수를 활용해 숫자를 하나씩 더하고 반환하고 리턴
          return { value: callback(num++), done: num > max };
        },
      };
    },
  };
}

const multiple = makeIterable(0, 20, (num) => num * 2);
console.clear();
for (const num of multiple) {
  console.log(num);
}

const single = makeIterable(0, 20, (num) => num);
for (const num of single) {
  console.log(num);
}
```
