# What is Async?

<br/>

## 1. callstack

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

```javascript
function runInDelay(seconds) {
  return new Promise((resolve, reject) => {
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

## 4. Promise egg

### (1)

### (2)

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

getChicken()
  .catch(() => '🐔')
  .then(fetchEgg)
  .then(fryEgg)
  .then(console.log);
```

<br/>

## 5. Promise all

### (1)

### (2)

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

// 바나나과 사과를 같이 가지고 오기
getBanana() //
  .then((banana) =>
    getApple() //
      .then((apple) => [banana, apple])
  )
  .then(console.log);

// Promise.all 병렬적으로 한번에 모든 Promise들을 실행
Promise.all([getBanana(), getApple()]) //
  .then((fruits) => console.log('all', fruits));

// Promise.race 주어진 Promise중에 제일 빨리 수행된것이 이김
Promise.race([getBanana(), getApple()]) //
  .then((fruit) => console.log('race', fruit));

Promise.all([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log('all-error', fruits))
  .catch(console.log);

Promise.allSettled([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log('all-settle', fruits))
  .catch(console.log);
```

<br/>

## 6. async

### (1) 설명

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

// 바나나과 사과를 같이 가지고 오기
async function fetchFruits() {
  const banana = await getBanana();
  const apple = await getApple();
  return [banana, apple];
}

fetchFruits() //
  .then((fruits) => console.log(fruits));
```

### (2) 응용

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

## 7. json

### (1) 설명

- JSON: JavaScript Object Notation
- 서버와 클라이언트(브라우저, 모바일) 간의 HTTP 통신을 위한 오브젝트 형태의 텍스트 포맷
- stringify(object): JSON
- parse(JSON): object

### (2) 용례

```javascript
const ella = {
  name: 'ella',
  age: 20,
  eat: () => {
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
