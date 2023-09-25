# What is Async?

<br/>

## 1. Callstack

### (1) 자바스크립트의 실행 순서

- 자바스크립트는 single context stack → single thread
- 따라서 자바스크립트는 **기본적으로 동기적으로 진행**

### (2) 용례

```javascript
function a() {
  // 이렇게 무거운 작업을 수행하면 좋지 않다
  for (let i = 0; i < 1000000000000000; i++);
  return 1;
}

function b() {
  return a() + 1;
}

function c() {
  return b() + 1;
}

console.log('시작');
const result = c();
console.log(result); // 시작
```

<br/>

## 2. Timeout

### (1) 설명

- 자바스크립트 언어 자체는 동기적으로 수행되지만
- 다양한 Web APIs를 통해 비동기적으로 코드 수행 가능

### (2) 용례

```javascript
function execute() {
  console.log('1');
  setTimeout(() => {
    console.log('2');
  }, 2000);
  console.log('3');
}
execute(); // 1 // 3 // (3초 뒤) 2
```

### (3) 응용

- 주어진 seconds(초)가 지나면 callback 함수를 호출함
- 단, seconds가 0보다 작으면 에러를 던지기

```javascript
function runInDelay(callback, seconds) {
  if (!callback) {
    throw new Error('callback 함수 전달 해야 함');
  }
  if (!seconds || seconds < 0) {
    throw new Error('seconds는 0보다 커야 함');
  }
  setTimeout(callback, seconds * 1000);
}
try {
  runInDelay(() => {
    console.log('타이머 완료');
  }, 2);
} catch (error) {}
```

<br/>

## 3. Promise

### (1) promise란?

- 무겁고 오래 걸리는 일이 있다면 코드 내부에서 비동기적으로 처리할 수 있게 도와줌
- '일이 끝나면 알려줄게, 약속해줄게!'

### (2) 용례

#### Promise 객체 만들기

1. new 연산자 사용
2. 생성자 안에는 promise를 만들 수 있는 콜백 함수를 전달
3. 콜백함수는 promise에 의해 호출될 것 (성공 시 호출: resolve, 실패 시 호출: reject)
4. 코드블록 안에서 각각의 비동기적인 일을 수행

```javascript
function runInDelay(seconds) {
  return new Promise((resolve, reject) => { // 두 가지 인자를 전달받아 처리하는 콜백함수
    if (!seconds || seconds < 0) {
      reject(new Error('seconds가 0보다 작음'));
    }
    setTimeout(resolve, seconds * 1000);
  });
}

runInDelay(2)
  .then(() => console.log('타이머 완료')) // 필요한 일을 수행
  .catch(console.error) // 에러를 처리
  .finally(() => console.log('끝남')); // 최종적으로 무조건 호출
```

<br/>

## 4. Promise Function

```javascript
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.reject(new Error('치킨을 가지고 올 수 없어'));
  //return Promise.resolve(`🪴 => 🐓`);
}

// chaining
getChicken()
  .catch(() => '🐔') // error가 발생할 수 있는 promise에는 항상 catch 등록 필수
  .then(fetchEgg)
  .then(fryEgg)
  .then(console.log); // 🐔 => 🥚 => 🍳
```

<br/>

## 5. Promise all

```javascript
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}

// 바나나와 사과를 같이 가지고 오기
// 바나나 1초 + 사과 3초 => 총 4초 소요
getBanana() //
  .then((banana) =>
    getApple() //
      .then((apple) => [banana, apple])
  )
  .then(console.log);

// Promise.all 병렬적으로 한번에, 성공했을 때만 모든 Promise들을 실행
// 사과 3초 => 총 3초 소요
Promise.all([getBanana(), getApple()]) //
  .then((fruits) => console.log('all', fruits));

// Promise.race 주어진 Promise중에 제일 빨리 수행된 것이 이김
// fruits가 아닌 fruit 단수: 가장 먼저 달려온 과일 하나
// 바나나 1초 => 총 1초 소요
Promise.race([getBanana(), getApple()]) //
  .then((fruit) => console.log('race', fruit));

// 에러 발생 getOrange까지 묶어서 병렬적으로
// getOrange 에러때문에 성공 못했으므로 => then 실행 X
Promise.all([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log('all-error', fruits))
  .catch(console.log); // getOrange 넣었으니 catch 꼭 넣어주기

// Promise.allSettled 모든 결과에 대해 받아보고 싶을 때 사용
Promise.allSettled([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log('all-settle', fruits))
  .catch(console.log);
```

<br/>

## 6. Async

### (1) 설명

- 동기적인 코드로 보이지만, 비동기적인 코드를 작성

### (2) 용례

```javascript
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}

// 바나나와 사과를 같이 가지고 오기
async function fetchFruits() {
  const banana = await getBanana();
  const apple = await getApple();
  return [banana, apple];
}

fetchFruits() //
  .then((fruits) => console.log(fruits));

// 원리
/*
1. Promise를 리턴하는 함수 호출 시, await 사용해서 기다렸다가
2. Promise 값이 resolve 되면 그 값을 반환 및 변수 (banana, apple)에 할당
3. 그냥 값을 return 할지라도 async 키워드가 있기 때문에 함수의 값은 배열[banana, apple]을 resolve하는 promise가 만들어짐
*/
```

### (3) 응용

```javascript
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.reject(new Error('치킨을 가지고 올 수 없어'));
  //return Promise.resolve(`🪴 => 🐓`);
}

async function makeFriedEgg() {
  let chicken;
  try {
    chicken = await getChicken();
  } catch {
    chicken = '🐔';
  }
  const egg = await fetchEgg(chicken);
  return fryEgg(egg);
}

makeFriedEgg().then(console.log);
```

<br/>

## 7. JSON

### (1) 설명

- JSON: JavaScript Object Notation
- 서버와 클라이언트(브라우저, 모바일) 간의 HTTP 통신을 위한 오브젝트 형태의 텍스트 포맷
- stringify(object): JSON
- parse(JSON): object

### (2) 용례

```javascript
const ella = {
  name: 'ella', // 객체의 프로퍼티는 json에 포함됨
  age: 20, // 객체의 프로퍼티는 json에 포함됨
  eat: () => { // 함수는 데이터 상태가 아니므로 json에 포함되지 않음
    console.log('eat');
  },
};

// 직렬화 Serializing: 객체를 문자열로 변환
const json = JSON.stringify(ella);
console.log(ella);
console.log(json);

// 역직렬화 Deserializing: 문자열 데이터를 자바스크립트 객체로 변환
const obj = JSON.parse(json);
console.log(obj);
```
