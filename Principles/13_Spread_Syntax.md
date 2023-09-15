# What is Spread Syntax?

<br/>

## 1. Spread 연산자, 전개구문

### (1) Spread 연산자란?

- 모든 Iterable은 Spread 될 수 있음
- 순회가 가능한 모든 것들은 쫙 펼쳐 질 수 있음
- ECMAScript9(2018)에 업데이트 됨

### (2) 용례

```javascript
// func(...iterable): 함수에서 다수의 인자를 받아올 때
// [...iterable]: 배열에서 하나하나씩 펼쳐 담기
// { ...obj }: Object를 하나하나씩 펼쳐 담기

function add(a, b, c) {
  return a + b + c;
}

const nums = [1, 2, 3];
console.log(add(...nums)); // 6

// Rest parameters
function sum(first, second, ...nums) {
  console.log(nums);
}
// 0, 1, 2, 4는 nums라는 인자에 전달됨
sum(1, 2, 0, 1, 2, 4); // [0, 1, 2, 4]

// Array Concat
const fruits1 = ['🍏', '🥝'];
const fruits2 = ['🍓', '🍌'];
let arr = fruits1.concat(fruits2);
console.log(arr); // ['🍏', '🥝', '🍓', '🍌']
arr = [...fruits1, '🍓', ...fruits2];
console.log(arr); // ['🍏', '🥝', '🍓', '🍓', '🍌']

// Object
const ella = { name: 'ella', age: 20, home: { address: 'home' } };
const updated = {
  ...ella, // ella를 쫙 펼쳐서 name과 age를 넣음
  job: 'FE developer',
};
console.log(ella);
console.log(updated);
```
