# How to make Module?

<br/>

## 1. Module

### (1) 모듈(Module)이란?

- 파일간 서로 coupling이 심하게 되어 있다면(다른 파일에서 변수 수정이 가능하다면) 버그에 치명적
- 이를 방지하기 위해 JavaScript에서는 각 파일을 모듈처럼 만들어 캡슐화 가능

### (2) 용례

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <!-- type 설정 가능 -->
    <script type="module" src="counter.js"></script>
    <script type="module" src="main.js"></script>
  </head>
  <body></body>
</html>
```

```javascript
// counter.js

let count = 0;
// 변수는 외부로 노출하지 않고, 외부에서는 increase 함수만 export하고, default 사용
// 여러 개 노출 시 default 붙이지 않음
export function increase() {
  count++;
  console.log(count);
}
export function getCount() {
  return count;
}
```

```javascript
// main.js

// import { increase as increase1 } from './counter.js'; 원하는 이름 지정 가능
// import { increase, getCount } from './counter.js';
// increase(); // 1
// increase(); // 2
// increase(); // 3
// console.log(getCount());

import * as counter from './counter.js';
counter.increase(); // 1
counter.increase(); // 2
counter.increase(); // 3
console.log(counter.getCount());
```
