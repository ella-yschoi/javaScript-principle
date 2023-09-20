# How to write Comments?

<br/>

## 1. 주석 처리 하는 방법

### (1) 주석 (Comments)

- 한 줄 짜리 주석을 작성할 때 사용
- TODO(엘라): 해야 할 일을 작성 (e.g. TODO(엘라): 로그인 기능 구현하기)
- 주석은 코드 자체를 설명하는 것이 아니라, 왜(WHY)와 어떻게(HOW)를 설명하기
- 단, 주석은 정말 필요한 경우만 사용

### (2) JSDoc

외부에서 많이 쓰이는 함수 API인 경우 JSDoc을 사용하면 좋음

```javascript
/**
 * 주어진 두 인자를 더한 값을 반환함
 * @param {*} a 숫자 1
 * @param {*} b 숫자 2
 * @returns a와 b를 더한값
 */
function add(a, b) {
  return a + b;
}
[].map;
```
