# How to Error Handling?

<br/>

## 1. Error Handling

### (1) throw error

아래와 같이 바로 error를 throw하면 application이 바로 crash될 수 있음

```javascript
function readFile(path) {
  throw new Error('에러닷');
  return '파일의 내용';
}

function processFile(path) {
  const content = readFile(path);
  const result = 'hi ' + content;
  return result;
}

const result = processFile('경로');
console.log(result); // Uncaught Error: 에러닷
```

### (2) try catch finally

```javascript
function readFile(path) {
  // throw new Error('에러닷');
  return '파일의 내용';
}

function processFile(path) {
  let content;
  try {
    content = readFile(path);
  } catch (error) {
    console.log(error);
    content = '기본 내용';
  } finally {
    console.log('성공하든 실패하든 마지막으로 리소스 정리 가능');
  }
  const result = 'hi ' + content;
  return result;
}

const result = processFile('경로');
console.log(result); // hi 파일의 내용
```

<br/>

## 2. Error Bubbling

### (1) Bubbling up, Propagating(전파)

- 에러는 최종적으로 호출한 곳까지 전파(버블링)되어 최종적으로 에러 캐치 가능
- 혹은, 제일 근접한 곳에서 잡고 싶다면 이것도 가능
- 즉, 에러가 전파되는 특징을 활용해 잡을 수 있는 적합한 곳에서 에러를 처리

```javascript
function a() {
  throw new Error('에러닷');
}

function b() {
  try {
    a();
  } catch (error) {
    console.log('이 에러는 핸들링이 어려울 것 같다면');
    throw error; // 여기서 throw해서 바로 잡기
  }
}

function c() {
  b();
}

try {
  c();
} catch (error) {
  console.log('Catched'); // Catched
}
console.log('done'); // done
```
