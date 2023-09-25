# What is Desturcturing Assignment?

<br/>

## 1. 구조 분해 할당 (Desturcturing Assignment)

### (1) 구조 분해 할당은?

- 데이터 뭉치(그룹화)를 쉽게 만들수 있음

### (2) 용례

```javascript
const fruits = ['🍏', '🥝', '🍓', '🍌'];
// 변수 이름을 지을 때 그룹을 지어서 배열 할당하면
// 배열의 구조가 분해되어 순서대로 각각 매칭됨
const [first, second, ...others] = fruits;
console.log(first); // 🍏
console.log(second); // 🥝
console.log(others); // ['🍓', '🍌']

const point = [1, 2];
const [y, x, z = 0] = point;
console.log(x); // 2
console.log(y); // 1
console.log(z); // 0

function createEmoji() {
  return ['apple', '🍎'];
}
// 👍 자주 사용됨
const [title, emoji] = createEmoji();
console.log(title); // apple
console.log(emoji); // 🍎

const ella = { name: 'Ella', age: 20, job: 'FE developer' };
function display(person) {
// (1) 기존: person을 반복적으로 사용
  console.log('이름', person.name); // 이름 Ella
  console.log('나이', person.age); // 나이 20
  console.log('직업', person.job); // 직업 FE developer
// (2) 변경: 3가지 key의 object 구조를 분해 및 할당하여 person을 반복적으로 쓰지 않아도 됨
function display({ name, age, job }) {
  console.log('이름', name); // 이름 Ella
  console.log('나이', age); // 나이 20
  console.log('직업', job); // 직업 FE developer
}
display(ella);

// 그냥 변수 할당 시에도 활용 가능
// 기본 값 지정도 가능 (pet)
// 다른 이름으로 변경도 가능 (job: occupation)
const { name, age, job: occupation, pet = '시바견' } = ella;
console.log(name); // Ella
console.log(age); // 20
console.log(occupation); // FE developer (job → occupation으로 변경됨)
console.log(pet); // 시바견
```

### (3) 응용: 구조분해할당 중첩

```javascript
const prop = {
  name: 'Button',
  styles: {
    size: 20,
    color: 'black',
  },
};

// color라는 인자를 받아와 color를 출력
// prop.styles.color로 접근하는 것이 아니라 구조분해할당 활용해 바로 color에 접근
// 전달 받은 object에, styles 라는 이름의 key가 있는데,
// 그 이름의 key 안에서 styles라는 키를 가진 값의 object 안에서, color라는 이름을 구조분해할당
// 즉, 괄호 단위(개수)로 접근하면 이해하기 쉬움
function changeColor({ styles: { color } }) {
  console.log(color);
}
changeColor(prop); // black
```
