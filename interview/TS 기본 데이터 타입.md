## TypeScript 기본 데이터 타입 정리

원시 타입: number, bigint, string, boolean, undefined, null, symbol

객체 타입: object, array, function

TS 전용: any, unknown, void, never, tuple, enum,
literal

타입 조합/재사용: type alias, union(|), intersection(&)

---

### 원시 타입 (Primitive, JavaScript와 동일)

- number → 정수, 실수, NaN, Infinity

- bigint → 큰 정수 (123n)

- string → 문자열 ("abc")

- boolean → true / false

- undefined → 값이 할당되지 않음

- null → 의도적으로 "없음"

- symbol → 고유한 식별자

### 객체 타입 (Reference)

- object → 비-원시 타입(object, array, function 등)

```ts
let obj: object = { x: 1 }; // 속성 타입 미상 → 접근/제약 어려움
let rec1: Record<string, unknown> = { x: 1, y: "a" };
let rec2: { [key: string]: unknown } = { a: true };
```

- 배열 (Array<T> / T[])

```ts
let arr1: number[] = [1, 2, 3];
let arr2: Array<string> = ["a", "b"];
```

- 함수 (Function)

```ts
// 지양: 너무 광범위하여 타입 안전성 낮음
const fnAny: Function = () => {};

// 권장: 구체적 시그니처 사용
const add: (a: number, b: number) => number = (a, b) => a + b;

// 또는 타입 별칭으로 표현
type ToString = (value: number) => string;
```

### 추가된 타입 (TypeScript 전용)

- any

  모든 타입 허용 (타입 검사 회피)

  최대한 사용 지양

  ```ts
  let value: any = 10;
  console.log(value.length); // undefined
  ```

- unknown

  any와 유사하지만, 안전한 any

  사용 전 타입 확인 필요

  ```ts
  let v: unknown = 123;
  // console.log(v + 1); // ❌ 오류
  if (typeof v === "number") {
    console.log(v + 1); // ✅ 안전
  }
  ```

- void

  주로 함수 반환 타입으로 사용

  ```ts
  function log(): void {
    console.log("hello");
  }
  ```

- never

  절대 발생하지 않는 값, 함수가 끝나지 않거나 항상 오류를 던지는 경우

  ```ts
  function fail(msg: string): never {
    throw new Error(msg);
  }
  ```

- tuple

  길이와 타입이 고정된 배열

  > 단, push, splice 같은 메서드는 막을 수 없음 (컴파일러 한계)

  ```ts
  let tuple: [string, number] = ["age", 27];
  ```

  불변이 필요하면 `readonly` 튜플 사용

  ```ts
  const rtuple: readonly [string, number] = ["age", 27];
  // rtuple[0] = "x" // 오류
  ```

- enum

  열거형 (숫자/문자 기반)

  ```ts
  enum Direction {
    Up,
    Down,
    Left,
    Right,
  }
  ```

- literal

  특정 값만 허용

  ```ts
  let yes: "Y" | "N";
  yes = "Y"; // ✅
  yes = "N"; // ✅
  // yes = "Maybe"; // ❌
  ```

### 타입 조합

- 타입 별칭(Type Alias)

  type 키워드로 정의

  복잡한 타입을 재사용 가능

  ```ts
  type ID = string | number;

  let id1: ID = "abc";
  let id2: ID = 123;
  ```

- 유니온 (Union, |)

  여러 타입 중 하나만 가능

  ```ts
  let value: string | number;
  value = "hi"; // 가능
  value = 123; // 가능
  // value = true; // 오류
  ```

  주로 리터럴 타입과 조합

  ```ts
  type Direction = "left" | "right" | "up" | "down";
  let move: Direction = "left";
  ```

- 교차 (Intersection, &)

  여러 타입을 합쳐 모두 만족해야 함

  보통 객체 타입 결합에 사용

  ```ts
  type Person = { name: string };

  type Worker = { job: string };

  type Employee = Person & Worker;

  let e: Employee = {
    name: "Raina",
    job: "Developer",
  };
  ```

### type vs interface

#### 공통점

둘 다 객체 타입 정의 가능

#### 차이점

TypeScript에서 interface와 type은 모두 타입 정의에 쓰이지만,

interface는 객체 구조 정의와 선언 병합에 강점이 있고 extends로 확장하며,

type은 유니언·튜플·원시 타입 별칭 등 더 넓은 표현이 가능하고 교차 타입(&)으로 조합합니다.

따라서 객체나 클래스 구조에는 interface, 복잡한 타입 조합에는 type을 주로 사용합니다.

(팀 컨벤션에 따라 상이)

```ts
// interface 확장
interface Person {
  name: string;
}
interface Person {
  age: number;
}
// 병합됨
const p: Person = { name: "Raina", age: 27 };

// type은 병합 불가
type User = { name: string };
// type User = { age: number }; // 오류
```

### 타입 호환성

TypeScript는 구조적 타이핑(structural typing) 기반 → 모양이 같으면 호환 가능

```ts
type Point = { x: number; y: number };
type Coord = { x: number; y: number };

let p: Point = { x: 1, y: 2 };
let c: Coord = p; // 가능 (구조가 같음)
```

## 엄격한 타입 설정

### strictNullChecks에서 null/undefined 할당

`strictNullChecks`가 켜져 있으면 `null`/`undefined`는 명시적으로 포함하지 않는 한 다른 타입에 할당 불가.

```ts
// tsconfig.json: "strictNullChecks": true
let s: string;
// s = null;      // ❌ 오류
s = "hi"; // ✅
let sn: string | null = null; // ✅
```

### any 사용을 엄격하게 제한하기 (so-called "strict any")

단일 옵션은 없지만, 다음 설정 조합으로 any 사용을 강하게 제한할 수 있습니다.

- `noImplicitAny`: 암시적 any 금지 (또는 `strict: true`로 일괄 활성화)
- `noImplicitThis`: `this`의 암시적 any 금지
- `useUnknownInCatchVariables`: `catch (e)`를 `unknown`으로 처리
- `suppressImplicitAnyIndexErrors: false` 유지로 암시적 any 인덱스 에러 억제 금지
- 명시적 any 자체 금지는 컴파일러가 아닌 ESLint 규칙 활용

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "noImplicitThis": true,
    "useUnknownInCatchVariables": true,
    "suppressImplicitAnyIndexErrors": false
  }
}
```

```json
// .eslintrc.json
{
  "rules": {
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

---

💡 타입이란

값이 가질 수 있는 유효한 범위의 집합을 의미함
