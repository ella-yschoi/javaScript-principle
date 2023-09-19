# What is ?

<br/>

## 1. 논리연산자 Logical Operator

### (1) 종류

- && 그리고
- || 또는
- 단축평가: short-circuit evaluation

### (2) 용례

### 단축평가의 예

```javascript
// 조건문 안에서 쓰면 모두 평가해야 함
const obj1 = { name: '🐶' };
const obj2 = { name: '🐱', owner: 'Ella' };

if (obj1 || obj2) { // true와 true
  console.log('모두 true'); // 모두 true
}

// 조건문 밖에서 쓰면 단축평가
// 어차피 obj1이 true이니, 뒤의 obj2는 평가 대신 result에 할당 → 단축평가
let result = obj1 && obj2;
console.log(result); // {name: '🐱', owner: 'Ella'}

// 어차피 obj1이 true이니, 뒤의 obj2는 평가하지 않아도 됨 → 단축평가
result = obj1 || obj2;
console.log(result); // {name: '🐶'}

// 조건이 truthy일 때 && 무언가를 해야 할 경우
// 조건이 falsey일 때 || 무언가를 해야 할 경우
function changeOwner(animal) {
  if (!animal.owner) {
    throw new Error('주인이 없어');
  }
  animal.owner = '바뀐 주인';
}
function makeNewOwner(animal) {
  if (animal.owner) {
    throw new Error('주인이 있어');
  }
  animal.owner = '새로운 주인';
}

// obj1이 false이기 때문에 뒤는 실행 X
obj1.owner && changeOwner(obj1);
// obj2가 true이기 때문에 뒤에 실행 O
obj2.owner && changeOwner(obj2);
console.log(obj1); // {name: '🐶'}
console.log(obj2); // {name: '🐱', owner: '바뀐 주인'}

obj1.owner || makeNewOwner(obj1);
obj2.owner || makeNewOwner(obj2);
console.log(obj1); // {name: '🐶', owner: '새로운 주인'}
console.log(obj2); // {name: '🐱', owner: '바뀐 주인'}
```

### null 또는 undefined인 경우를 확인할 때

```javascript
let item = { price: 1 };
const price = item && item.price;
console.log(price); // 1

// 기본값을 설정
// default parameter 전달하지 않거나, undefined인 경우에만 설정
// || 값이 falshy한 경우 설정(할당): 0, -0, null, undefined, ''
function print(message) {
  const text = message || 'Hello';
  console.log(text);
}
print(); // Hello
print(undefined); // Hello
print(null); // Hello
print(0); // Hello
```

<br/>

## 2. 옵셔널 체이닝 연산자 (Optional Chaining Operator)

### (1) 정의와 종류

- ECMAScript11(2020)에서 추가됨
- ?.
- null 또는 undefined을 확인할 때

### (2) 용례

```javascript
let item = { price: 1 };
const price = item?.price;
console.log(price); // 1

let obj = { name: '🐶', owner: { name: '엘라' } };
function printName(obj) {
  const ownerName = obj?.owner?.name; // obj가 있다면, 그리고 owner가 있다면 name에 접근
  console.log(ownerName); // 엘라
}
printName(obj);
```

<br/>

## 3. Nullish Coalescing Operator

### (1) 정의와 종류

- ECMAScript11(2020)에서 추가됨
- ??: null과 undefined인 경우에만 뒤의 코드를 수행하고 싶을 때 사용
- ||: falshy한 경우 설정(할당) 0, -0, ''

### (2) 용례

```javascript
let num = 0;
console.log(num || '-1'); // -1: 여기서는 num이 0(falsey)인 경우
console.log(num ?? '-1'); // 0: num 값이 없을 때만 -1를 설정
```
