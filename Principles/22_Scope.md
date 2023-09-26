# What is Scope?

<br/>

## 1. 스코프(Scope)

### (1) 스코프(Scope)란?

- 변수를 참조할 수 있는(접근할 수 있는) 유효한 범위
- 또는 식별자(변수, 함수, 클래스 이름)가 유효한 범위
- 즉, 선언된 위치에 따라 유효 범위가 결정됨
- 블록 안의 변수는 블록 안에서만 유효함 (for. 이름 충돌 방지, 메모리 절약)
- 따라서 **변수는 최대한 필요한 곳에서 정의해야 함**

### (2) 용례

```javascript
// 코드 블록: { }, if() { }, for() { }, function() { }

// 블록 외부에서는 블록 내부의 변수를 참조 ❌
{
  const a = 'a';
}
console.log(a);
const b = 'b'; // Uncaught ReferenceError: a is not defined

// 함수 외부에서는 함수 내부의 변수를 참조 ❌
function print() {
  const message = 'Hello World';
  console.log(message);
}
console.log(message);

// 함수 외부에서는 함수의 매개변수를 참조 ❌
function sum(a, b) {
  console.log(a, b);
}
console.log(a, b);
```

<br/>

## 2. Scope 응용

```javascript
{
  const x = 1;
  {
    const y = 2;
    // 아무리 중첩되어 있어도 자식 블록은 부모 블록의 변수를 참조 가능 (단 외부에서 내부 참조는 X)
    console.log(x); // 1
  }
  console.log(x); // 1
  // console.log(y); // error
}
```

```javascript
const text = 'global';
{
  {
    console.log(text); // global
  }
}
```

```javascript
// 전역 변수(글로벌 변수), 전역 스코프(글로벌 스코프)
const text = 'global';
{
  // 지역 변수(로컬 변수), 지역 스코프(로컬 스코프)
  const text = 'inside block1';
  {
    console.log(text); // 가장 근접한 inside block1 출력
  }
}
```

```javascript
const text = 'global';
{
  const text = 'inside block1';
  {
    const text = 'inside block2';
    console.log(text); // inside block2
  }
}
```

<br/>

## 3. 가비지 컬렉터 (Garbage Colletor)

### (1) 동작 원리

- JavaScript Engine Background Process
- 그 어떤 것도 object를 참조하고 있지 않으면 메모리에서 GC가 자동으로 깨끗하게 청소해 줌
- 다만, GC 동작이 너무 빈번하면 리소스 낭비이므로 불필요한 메모리 할당/재할당보다, 필요한 만큼만 사용하는 게 좋음

### (2) 용례

```javascript
// 글로벌 변수는 앱이 종료될 때까지 계속 메모리에 유지됨
// 따라서 가급적 글로벌 변수 선언 및 사용은 지양 (+ 이름 충돌의 가능성)
const global = 1;
{
  // 블록 내부에서만 존재하고
  const local = 1;
}
// 블록이 끝나면 GC에 의해 자동으로 소멸됨
// 청소도 바로 이루어지기 보다 GC가 원할 때마다 주기적으로 메모리 검사 후 청소

function print() {
  // 함수 내부에서도 블록 안에서 필요한 경우에는
  // 필요한 곳(블록 안에서) 변수를 선언하고 사용해야 함
  if (true) {
    let temp = 0;
  }
}
```

<br/>

## 4. 실행 컨텍스트 (Execution Context)

### (1) 정의

- 코드의 실행 순서와 스코프를 기억

### (2) 동작 원리

- 각각의 블록은 **렉시컬 환경(Lexical Environment)** 이라는 내부 object를 가지고 있음
- 렉시컬 환경은 각각 블록마다 아래와 같은 정보들을 가지고 있음
  - 환경 레코드 (Environment Record): 어떤 변수들이 들어 있는지 등 블록의 정보
  - 외부 환경 참조 (Outer Lexical Environment Reference): 부모는 누구 인지
- 스코프 체인으로 스코프들이 서로 연결되어 있어 외부 변수 참조 가능
- 여러 블록이 중첩되어 있으면 여러 스코프를 돌아다니며 참조해야하므로, **변수는 필요한 곳에서만 선언하기**
