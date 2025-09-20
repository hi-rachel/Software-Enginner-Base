## this

this는 함수가 어떻게 호출되었는지에 따라 동적으로 결정되는 특별한 식별자입니다.

1. 기본 window

   > window = global object 전역변수 보관소 = 맨 위 {}

   strict mode 환경 일반 함수 내에서 undefined

2. object 내 함수를 쓰면
   그 함수를 가지고 있는 오브젝트를 뜻함

3. 화살표 문법 사용시 상위 요소 그대로 물려받음

   (내부의 this 값을 변화시키지 않음, 외부 this 값 그대로 재사용 가능)

   > 화살표 문법 사용시 this 바인딩 문제 많은 부분 해결

4. constructor 안에서의 this = 새로 생성되는 오브젝트 (인스턴스)

5. 이벤트 리스너 안에서의 this = e.currentTarget (지금 이벤트 동작하는 곳)

---

### 바인딩 우선순위

1. new 바인딩 (constructor)
   > this는 새로 생성된 인스턴스
2. 명시적 바인딩
   > call, apply, bind로 this 직접 지정
   > call은 개별 인수 전달(, 구분), apply는 인수 배열을 전달, bind는 새 함수 반환
3. 암시적 바인딩
   > 객체의 메서드로 호출, this는 해당 객체
4. 기본
   > 전역 객체(window) 또는 strict mode 환경 일반 함수 내에서 undefined

---

[참고]

https://codingapple.com/unit/es6-1-this-keyword-object/?id=2447

https://bitbetter.vercel.app/blog/javascript/this-binding
