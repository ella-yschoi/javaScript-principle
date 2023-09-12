# What is Array?

<br/>

## 1. 배열

### (1) 배열 생성 방법

```javascript
// 배열의 사이즈 정의
let array = new Array(3);
console.log(array); // [empty × 3]

// 생성자 함수: 실제 아이템 전달
array = new Array(1, 2, 3);
console.log(array); // [1, 2, 3]

// static 함수: 요소들의 집합으로부터 새로운 배열을 반환
array = Array.of(1, 2, 3, 4, 5);
console.log(array); // [1, 2, 3, 4, 5]

// 배열 리터럴
const anotherArray = [1, 2, 3, 4];
console.log(anotherArray); // [1, 2, 3, 4]
// 기존의 배열로부터 새로운 배열 생성
array = Array.from(anotherArray);
console.log(array); // [1, 2, 3, 4]
```

### (2) 중요한 개념

- 일반적으로 배열은 동일한 메모리 크기를 가지며, 연속적으로 이어져 있어함
- 하지만 **JavaScript에서의 배열은 연속적으로 이어져 있지 않고 Object와 유사함**
- JavaScript의 배열은 일반적인 배열의 동작을 **흉내낸 특수한 객체임**
- 이를 보완하기 위해 타입이 정해져 있는 **타입 배열**이 있음 (Typed Collections)

```javascript
// object로부터 배열을 만들기
array = Array.from({
  0: '안',
  1: '녕',
  length: 2,
});
console.log(array); // ['안', '녕']
```

<br/>

## 2. 배열에서 요소 참조/추가하기

### (1) 배열에서 아이템을 참조하는 방법

```javascript
const fruits = ['🍌', '🍎', '🍇', '🍑'];
console.log(fruits[0]); // 🍌
console.log(fruits[1]); // 🍎
console.log(fruits[2]); // 🍇
console.log(fruits[3]); // 🍑
console.log(fruits.length); // 4

for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]); // 모든 아이템 출력 가능
}
```

### (2) 배열에서 아이템을 추가/삭제 하기: 좋지 않은 방식 ❌

- 인덱스로 바로 접근하는 방법은 별로 좋지 않음

```javascript
const fruits = ['🍌', '🍎', '🍇', '🍑'];
// 추가 case.1
fruits[3] = '🍓';
console.log(fruits); // ['🍌', '🍎', '🍇', '🍓']
// 추가 case.2
fruits[6] = '🍓';
console.log(fruits); // ['🍌', '🍎', '🍇', '🍑', empty × 2, '🍓']
// 추가 case.3
fruits[fruits.length] = '🍓';
console.log(fruits); // ['🍌', '🍎', '🍇', '🍑', '🍓']

// 삭제 case
delete fruits[1];
console.log(fruits); // ['🍌', empty, '🍇', '🍑']
```

<br/>

## 3. 배열의 함수들

### (1) 배열 자체를 업데이트

```javascript
// 배열 자체를 변경하는지, 새로운 배열을 반환하는지
const fruits = ['🍌', '🍎', '🍋'];

// 특정한 오브젝트가 배열인지 체크
console.log(Array.isArray(fruits)); // true
console.log(Array.isArray({})); // false

// 특정한 아이템의 위치를 찾을때
console.log(fruits.indexOf('🍎')); // 1

// 배열안에 특정한 아이템이 있는지 체크
console.log(fruits.includes('🍎')); // true

// 추가 - 제일 뒤
let length = fruits.push('🍑');
console.log(fruits); // ['🍌', '🍎', '🍋', '🍑']
console.log(length); // 4

// 추가 - 제일 앞
length = fruits.unshift('🍓');
console.log(fruits); // ['🍓', '🍌', '🍎', '🍋', '🍑']
console.log(length); // 5

// 제거 - 제일 뒤
let lastItem = fruits.pop();
console.log(fruits); // ['🍓', '🍌', '🍎', '🍋']
console.log(lastItem); // 🍑

// 제거 - 제일 앞
lastItem = fruits.shift();
console.log(fruits); // ['🍌', '🍎', '🍋']
console.log(lastItem); // 🍓

// 중간에 추가 또는 삭제
const deleted = fruits.splice(1, 1);
console.log(fruits); // ['🍌', '🍋']
console.log(deleted); // ['🍎']: 배열 형태로 반환
fruits.splice(1, 1, '🍎', '🍓');
console.log(fruits); // ['🍌', '🍎', '🍓'] // 지우고 추가
```

### (2) 새로운 배열로 업데이트

```javascript
// 잘라진 새로운 배열을 만듬
let newArr = fruits.slice(0, 2); // 0은 포함 2는 제외해서
console.log(newArr); // ['🍌', '🍎'] // 0부터 1까지만 잘라진 새로운 배열
console.log(fruits); // ['🍌', '🍎', '🍓'] // 기존 배열은 유지된 상태
newArr = fruits.slice(-1); // 뒤에서부터
console.log(newArr); // ['🍓']

// 여러 개의 배열을 붙여줌
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = arr1.concat(arr2);
console.log(arr1); // [1, 2, 3]
console.log(arr2); // [4, 5, 6]
console.log(arr3); // [1, 2, 3, 4, 5, 6]

// 순서를 거꾸로
const arr4 = arr3.reverse(); // 기존 arr3을 거꾸로 하는 새로운 arr4
console.log(arr4); // [6, 5, 4, 3, 2, 1]
console.clear();

// 중첩 배열을 하나의 배열로 펼치기
let arr = [
  [1, 2, 3],
  [4, [5, 6, [3, 4]]],
];
console.log(arr); // [[1, 2, 3],[4, [5, 6, [3, 4]]],]
console.log(arr.flat()); // 기본적으로 한 단계만 중첩 풀어줌
console.log(arr.flat(3)); // [1, 2, 3, 4, 5, 6, 3, 4] : 모든 중첩 풀어줌
arr = arr.flat(3);

// 특정한 값으로 배열을 채우기
arr.fill(0); // 배열 자체를 수정
console.log(arr); // [0, 0, 0, 0, 0, 0, 0, 0]

arr.fill('s', 1, 3); // 1부터 2까지
console.log(arr); // [0, 's', 's', 0, 0, 0, 0, 0]

arr.fill('a', 1); // 1부터 쭉
console.log(arr); // [0, 'a', 'a', 'a', 'a', 'a', 'a', 'a']

// 배열을 문자열로 합하기
let text = arr.join();
console.log(text); // 0,a,a,a,a,a,a,a
text = arr.join(' | ');
console.log(text); // 0 | a | a | a | a | a | a | a
```

<br/>

## 4. 얕은 복사 (Shallow Copy)

### (1) 얕은 복사란?

- 얕은 복사: 객체는 메모리 주소 전달
- JavaScript에서 object 복사할 때는 **항상 얕은 복사**가 이루어짐
- Array.from, concat, slice, spread(...), Object.assign

### (2) 용례

```javascript
const pizza = { name: '🍕', price: 2, owner: { name: 'Ella' } };
const ramen = { name: '🍜', price: 3 };
const sushi = { name: '🍣', price: 1 };
const store1 = [pizza, ramen];
const store2 = Array.from(store1);
console.log('store1', store1);
console.log('store2', store2);

store2.push(sushi);
console.log('store1', store1);
console.log('store2', store2); // sushi 추가됨 → store1과 서로 다른 배열

pizza.price = 4;
// 배열 자체를 수정하지 않아도 price가 2 → 4로 변경됨: 얕은 복사
// object 변경되더라도 새로운 object를 가리키는게 아니라, 동일한 object를 가리키고 있음
// object가 메모리 주소가 전달되는 것
console.log('store1', store1);
console.log('store2', store2);
```

<br/>

## 5. 응용

### (1) Quiz.1

- 주어진 배열 안의 딸기 아이템을 키위로 교체하는 함수를 만들기
- 단, 주어진 배열을 수정하지 않고, 새로운 배열 반환

```javascript
// input: ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🥝', '🍇', '🥝' ]

function replace(array, from, to) { // from과 to를 사용해 재사용성
  const replaced = Array.from(array); // 새로운 배열
  for (let i = 0; i < replaced.length; i++) {
    if (replaced[i] === from) { // 딸기라면
      replaced[i] = to; // 키위로 바꿔줌
    }
  }
  return replaced; // 새로운 배열 리턴
}

const array = ['🍌', '🍓', '🍇', '🍓'];
const result = replace(array, '🍓', '🥝');
console.log(result);
```

### (2) Quiz.2

- 배열과 특정한 요소를 전달받아,
- 배열안에 그 요소가 몇개나 있는지 카운트 하는 함수 만들기

```javascript
// input: [ '🍌', '🥝', '🍇', '🥝' ], '🥝'
// output: 2

function count(array, item) {
  let counter = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i] === item) {
      counter++;
    }
  }
  return counter;
}

console.log(count(['🍌', '🥝', '🍇', '🥝'], '🥝'));
```

### (3) Quiz.3

- 배열1, 배열2 두개의 배열을 전달받아,
- 배열1 아이템중 배열2에 존재하는 아이템만 담고 있는 배열 반환

```javascript
// array1: ['🍌', '🥝', '🍇'],  array2: ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🍇' ]

function match(array1, array2) {
  const result = [];
  for (let i = 0; i < array1.length; i++) {
    // array2 배열 안에 array1 인덱스가 중복되어 있다면
    if (array2.includes(array1[i])) {
      // 중복된 요소를 담는 result 배열에 해당 요소 넣기
      result.push(array1[i]);
    }
  }
  return result;
}
console.log(match(['🍌', '🥝', '🍇'], ['🍌', '🍓', '🍇', '🍓']));
```
