## 렌더링 성능을 최적화 하기 위해 React.memo, useMemo, useCallback을 어떻게 활용하는지?

컴포넌트 리렌더링 건너뛰기는 React.memo, 비싼 값 캐시는 useMemo, 함수 참조 고정은 useCallback 활용.

적용 전후를 Profiler로 측정하고, 의존성은 ESLint로 정확히 관리 필요.

## React.memo(Component)

**React.memo는 컴포넌트를 메모이제이션하여 props가 변경되지 않으면 리렌더링을 방지**합니다.

> 동일한 props가 들어오면 diff 조차 실행하지 않음.
>
> 사용 예시:
>
> - 부모가 자주 리렌더링되지만 자식 props가 자주 변하지 않을 때
> - 리스트 아이템, 카드 같은 잦게 그려지는 프레젠테이셔널 컴포넌트

## useMemo(compute, deps)

**useMemo는 계산 비용이 높은 값을 메모이제이션**합니다.

> 사용 예시:
>
> - 정렬/필터/집계 등 연산 비용이 큰 계산
> - 자식에게 넘기는 { options }, [] 같은 객체/배열 props 참조를 안정화해야 할 때
>
> 주의:
>
> - 사소한 계산을 과도하게 useMemo로 감싸면 메모 비용 > 이득이 될 수 있음. 측정 후 적용.

## useCallback(fn, deps)

**useCallback은 함수를 메모이제이션하여 불필요한 리렌더링을 방지**합니다.

> 콜백 함수의 참조(identity) 를 고정함.
>
> 사용 예시:
>
> - 콜백을 React.memo된 자식에게 props로 넘길 때
> - useEffect/useMemo 의존성에 안정된 함수가 필요할 때
>
> 주의:
>
> - 상위에서만 쓰고 하위로 넘기지 않는 '지역 핸들러'는 굳이 useCallback이 필요 없을 때가 많음.

---

## React 19 업데이트

React Compiler를 켜면 많은 경우 메모이제이션이 자동으로 대체되지만, 전부가 ‘불필요’해지는 건 아니며 여전히 상황에 따라 수동 최적화가 필요합니다.

> 컴파일러가 자동 메모이제이션을 적용해 불필요 리렌더링을 줄이고, 값/함수 참조를 안정화합니다.
> 이때 수동 memo/useMemo/useCallback의 필요성이 크게 줄 수 있습니다. 다만 프로젝트에 컴파일러를 켜는 설정이 필요합니다. ￼

그래도 수동 메모이제이션이 필요한 대표 상황

- 매우 비싼 계산을 확실히 캐시해야 하거나, 컴파일러가 최적화에서 bail out(보수적으로 포기)하는 경우

- 서드파티 라이브러리가 “항상 안정된 참조”를 요구하는 API를 갖는 경우

- 성능 경계를 명시적으로 설정해 프로파일링·튜닝을 제어하고 싶을 때(특정 서브트리 컷)

- 팀/빌드 환경상 아직 React Compiler를 도입하지 않았을 때 ￼

---

[참고]

https://bitbetter.vercel.app/blog/frontend/memo-usememo-usecallback

https://www.youtube.com/watch?v=S6GlgPX0Q7k
