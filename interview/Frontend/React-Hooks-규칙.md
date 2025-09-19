## React Hooks 규칙 (Rules of Hooks)

React 훅은 반드시 함수 컴포넌트나 커스텀 훅의 **최상위에서만 호출**해야 하고, 반복문이나 조건문 안에서 호출하면 안 됩니다. 호출 순서가 변하면 React의 상태 추적이 깨지기 때문입니다. 또 훅 이름은 use로 시작해야 하고, eslint-plugin-react-hooks로 규칙을 자동 검증할 수 있습니다.

---

1. 최상위에서만 호출한다

   반복문, 조건문, 중첩 함수 안에서 훅을 호출하면 안 됨.

   이유: React는 훅 호출 순서를 기반으로 내부 상태를 추적하기 때문에 호출 순서가 달라지면 상태가 꼬임.

2. 리액트 함수 컴포넌트 또는 커스텀 훅 안에서만 호출한다

   클래스 컴포넌트, 일반 JS 함수 안에서는 훅을 호출할 수 없음.

   가능한 위치:

   - React 함수형 컴포넌트

   - 커스텀 훅(use로 시작하는 함수)

3. 훅은 항상 같은 순서로 호출되어야 한다

   조건부 실행 금지(if, switch, for 내부 금지).

   올바른 예시:

   ```js
   function MyComponent(props) {
     const [count, setCount] = useState(0); // 항상 첫 번째 호출
     const [name, setName] = useState(""); // 항상 두 번째 호출
   }
   ```

4. 커스텀 훅도 훅 규칙을 따라야 한다

   커스텀 훅 내부에서 다른 훅을 쓸 수 있지만, 역시 최상위에서만 호출해야 하고 조건문/반복문 금지.

5. 훅 이름은 반드시 use로 시작해야 한다

   use prefix를 통해 React가 훅을 인식하고 lint 규칙(ESLint-plugin-react-hooks)으로 검사 가능.

6. ESLint 규칙 준수

   eslint-plugin-react-hooks를 사용하면 위의 규칙(의존성 배열 포함)을 자동 검사.

   react-hooks/rules-of-hooks: 훅 호출 위치 규칙 강제.

   react-hooks/exhaustive-deps: useEffect, useMemo, useCallback의 의존성 배열 검사.
