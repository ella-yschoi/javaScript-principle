# What is Generator?

<br/>

## 1. 제너레이터 (Generator)

### (1) 제너레이터란?

- iterator와 마찬가지로 iteration protocol을 준수하나, 더 간편하게 iterator를 만들 수 있는 방법
- 값을 생성할 수 있는 생성기

### (2) 용례

```javascript
// const multiple = {
//   [Symbol.iterator]: () => {
//     const max = 10;
//     let num = 0;
//     return {
//       next() {
//         return { value: num++ * 2, done: num > max };
//       },
//     };
//   },
// };

// 꼭 *를 붙여주기
function* multipleGenerator() {
  try {
    for (let i = 0; i < 10; i++) {
      console.log(i);
      // return은 바로 return하나, yield는 next() 호출할때까지 기다렸다가 하나하나씩 리턴
      yield i ** 2;
    }
  } catch (error) {
    console.log(error);
  }
}
const multiple = multipleGenerator();
let next = multiple.next();
console.log(next.value, next.done);

// multiple.return(); : 끝내기
multiple.throw('Error!'); // multiple 안으로 error를 던지기

next = multiple.next();
console.log(next.value, next.done);
```
