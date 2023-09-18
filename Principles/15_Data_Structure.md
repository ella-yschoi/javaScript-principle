# What is Data Structure?

<br/>

## 1. Set

### (1) Set이란?

- 데이터의 집합체
- 인덱스도 없고, 순서도 없음
- 대신 중복 불가

### (2) 용례

```javascript
// Set
const set = new Set([1, 2, 3]);
console.log(set); // Set(3) {1, 2, 3}

// 사이즈 확인
console.log(set.size); // 3

// 존재하는지 확인
console.log(set.has(2)); // true
console.log(set.has(6)); // false

// 순회
set.forEach((item) => console.log(item)); // 1 // 2 // 3
for (const value of set.values()) {
  console.log(value); // 1 // 2 // 3
}

// 추가
set.add(6);
console.log(set); // Set(4) {1, 2, 3, 6}

// 있는데 추가하면 무시됨: 중복 불가이기 때문
set.add(6);
console.log(set); // Set(4) {1, 2, 3, 6}

// 삭제
set.delete(6);
console.log(set); // Set(3) {1, 2, 3}

// 전부 삭제
set.clear();
console.log(set); // Set(0) {size: 0}

// object 세트
const obj1 = { name: '🍎', price: 8 };
const obj2 = { name: '🍌', price: 5 };
const objs = new Set([obj1, obj2]);
console.log(objs);
// Set(2) {
//   { name: '🍎', price: 8 },
//   { name: '🍌', price: 5 }
// }

// 응용 1: object는 shallow copy가 된다
obj1.price = 10;
objs.add(obj1);
console.log(objs);
// Set(2) {
//   { name: '🍎', price: 10 },
//   { name: '🍌', price: 5 }
// }

// 응용 2: obj3은 Heap에 새롭게 만들어지고, 메모리의 참조주소가 다름
const obj3 = { name: '🍌', price: 5 };
objs.add(obj3);
console.log(objs);
// Set(3) {
//   { name: '🍎', price: 10 },
//   { name: '🍌', price: 5 },
//   { name: '🍌', price: 5 }
// }
obj3.price = 8;
console.log(objs);
// Set(3) {
//   { name: '🍎', price: 10 },
//   { name: '🍌', price: 5 },
//   { name: '🍌', price: 8 }
// }
```

<br/>

## 2. Map

### (1) Map이란?

- 키와 값으로 이루어진 자료구조
- 순서가 중요하지 않음
- 키가 가리키고 있는 object 대상으로 자료구조를 찾고, 가지고 오기 때문에 순서가 거꾸로 되어 있어도 상관 없음
- 다만, map에서는 키가 '유일한 키'를 가지고 있어야 함
- 따라서 키만 다르다면 중복 가능
- object와 유사하여 object처럼 사용 가능

### (2) 용례

```javascript
const map = new Map([
  ['key1', '🍎'],
  ['key2', '🍌'],
]);
console.log(map); // Map(2) {'key1' => '🍎', 'key2' => '🍌'}

// 사이즈 확인
console.log(map.size); // 2

// 존재하는지 확인
console.log(map.has('key1')); // true
console.log(map.has('key6')); // false

// 순회
map.forEach((value, key) => console.log(key, value)); // key1 🍎 // key2 🍌
console.log(map.keys()); // MapIterator {'key1', 'key2'}
console.log(map.values()); // MapIterator {'🍎', '🍌'}
console.log(map.entries()); // MapIterator {'key1' => '🍎', 'key2' => '🍌'}

// 찾기
console.log(map.get('key1')); // 🍎
console.log(map.get('key2')); // 🍌
console.log(map.get('key4')); // undefined

// 추가
map.set('key3', '🥝');
console.log(map); // Map(3) {'key1' => '🍎', 'key2' => '🍌', 'key3' => '🥝'}

// 삭제
map.delete('key3');
console.log(map); // Map(2) {'key1' => '🍎', 'key2' => '🍌'}

// 전부  삭제
map.clear();
console.log(map); // Map(0) {size: 0}
```

### (3) object와의 차이점

```javascript
const key = { name: 'milk', price: 10 };
const milk = { name: 'milk', price: 10, description: '맛있는 우유' };
const obj = {
  [key]: milk,
};
// 1. object
console.log(obj); // {'[object Object]': { name: 'milk', price: 10, description: '맛있는 우유'}}
// 2. map
const map2 = new Map([[key, milk]]);
console.log(map2); // Map(1) {{ name: 'milk', price: 10} => { name: 'milk', price: 10, description: '맛있는 우유' }}

console.log(obj[key]); // {name: 'milk', price: 10, description: '맛있는 우유'}
console.log(map2[key]); // undefined: 동적 접근 불가
console.log(map2.get(key)); // {name: 'milk', price: 10, description: '맛있는 우유'}

// 차이점
// 1. map과 object는 서로 사용할 수 있는 함수가 다름
// 2. object에서는 키를 동적으로 접근 가능하나, map은 불가 → map에서는 get으로 접근
```

<br/>

## 3. 응용

### (1) Quiz.1

주어진 배열에서 중복을 제거 하기

```javascript
const fruits = ['🍌', '🍎', '🍇', '🍌', '🍎', '🍑'];
//  ['🍌', '🍎', '🍇', '🍑']
function removeDuplication(array) {
  // set에 있는 item들을 하나하나씩 풀어서 배열로 만들어 줌
  // set는 중복 불가하므로 중복 제거됨
  return [...new Set(array)];
}
console.log(removeDuplication(fruits));
```

### (2) Quiz.2

주어진 두 세트의 공통된 아이템만 담고 있는 세트 만들기

```javascript
const set1 = new Set([1, 2, 3, 4, 5]);
const set2 = new Set([1, 2, 3]);

function findIntersection(set1, set2) {
  return new Set([...set1].filter((item) => set2.has(item)));
}
console.log(findIntersection(set1, set2));
```

<br/>

## 4. Symbol

### (1) Symbol이란?

- 유일한 키를 생성할 수 있음

### (2) 용례

#### Symbol 사용 전

```javascript
const map = new Map();
const key1 = 'key';
const key2 = 'key';

// key1에 Hello라는 값 설정
map.set(key1, 'Hello');
// 변수 key1의 값을 'Hello'로 바꾸는 것이 아니라
// key1의 값인 'key'를 키로, 'Hello'를 값으로 하는 새로운 요소를 추가하겠다는 뜻

console.log(map.get(key2)); // Hello
// 다른 변수를 만들어 두긴 했지만,
// 문자열이 똑같은 값이니 key1으로 값을 넣어도 key2로 동일한 Hello를 가지고 나올 수 있음
// 원시타입이기 때문에 값이 똑같아서 동일한 키라고 간주하기 때문

console.log(key1 === key2); // true
```

#### Symbol 사용 후

```javascript
const map = new Map();
const key1 = Symbol('key');
const key2 = Symbol('key');

map.set(key1, 'Hello');
console.log(map.get(key2)); // undefined
console.log(key1 === key2); // false
// 이름은 똑같지만 symbol을 만들면 서로 다른 값이 만들어짐
// 따라서 유일한 값을 사용할 때 symbol을 사용하면 좋음
```

#### Symbol.for

- 동일한 이름으로 하나의 키를 사용하고 싶다면 → Symbol.for
- 전역 심볼 레지스트리 (Global Symbol Registry): 전역적으로 symbol을 만들고 관리하는 곳에서 이 이름의 심볼을 만들고 계속 재사용 가능

```javascript
const map = new Map();
const k1 = Symbol.for('key');
const k2 = Symbol.for('key');
console.log(k1 === k2); // true: 동일한 symbol을 리턴 및 재사용

console.log(Symbol.keyFor(k1)); // key: 해당 symbol이 가지고 있는 문자열에 관한 정보
console.log(Symbol.keyFor(key1)); // undefined
// keyFor은 Global Symbol Registry에 보관된 symbol에 한해서만 이름을 가져올 수 있음
// key1은 Registry에서 만든게 아니고, 새로운 symbol을 만든 것이기 때문에 문자열에 관한 정보를 가져올 수 없음
// 즉, 일반 symbol은 문자열에 관한 정보가 숨겨져 있고, Registry를 통해서 만든 symbol에 한해서만 문자열에 관한 정보 가져올 수 있음

// object에서도 사용
const obj = { [k1]: 'Hello', [Symbol('key')]: 1 };
console.log(obj);
console.log(obj[k1]); // Hello
console.log(obj[Symbol('key')]); // undefined: 위의 obj에서 선언된 symbol과 전혀 다른 유니크한 symbol이기에 접근 불가
```
