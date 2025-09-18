## JavaScript 기본 데이터 타입

원시 타입 7개: number, bigint, string, boolean, undefined, null, symbol

참조 타입: object (배열, 함수 포함)

### 원시 타입 (Primitive, 7개)

- number → 정수/실수, NaN, Infinity

- bigInt → 큰 정수 (123n)

- string → 문자열 ("abc")

- boolean → true / false

- undefined → 값이 할당되지 않음

- null → 의도적으로 "없음"

- symbol → 고유한 식별자

### 참조 타입 (Reference)

- object

- 일반 객체 { key: value }

- 배열 []

- 함수 function() {}, () => {}

- 내장 객체 (Date, RegExp, Map, Set, ...)

### 특징

원시 타입: 값 자체가 복사됨

참조 타입: 메모리 주소가 복사됨

```js
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 ✅ 값 복사
```

```js
let obj1 = { x: 1 };
let obj2 = obj1;
obj2.x = 2;
console.log(obj1.x); // 2 ❌ 참조 복사
```

```js
typeof 결과;
typeof 123; // "number"
typeof 123n; // "bigint"
typeof "abc"; // "string"
typeof true; // "boolean"
typeof undefined; // "undefined"
typeof null; // "object" (버그)
typeof Symbol(); // "symbol"
typeof {}; // "object"
typeof []; // "object"
typeof (() => {}); // "function"
```
