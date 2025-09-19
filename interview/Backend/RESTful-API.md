## REST API란?

**REST API는 자원을 URI로 표현하고, HTTP 메서드로 해당 자원에 대한 동작을 정의하는 아키텍처 스타일**입니다.
RESTful하려면 클라이언트-서버 구조, 무상태성, 캐시 가능, 계층화 시스템, 일관된 인터페이스, 필요 시 코드 온 디맨드 같은 원칙을 지켜야 합니다.
이를 통해 확장성과 일관성 있는 API 설계가 가능합니다.

### 왜 RESTful하게 설계해야 하나요?

RESTful하게 설계하면 API가 직관적이고 일관성 있게 동작하기 때문에 재사용성과 유지보수성이 좋아집니다. 또한 HTTP 표준을 활용할 수 있어 확장성과 성능 최적화에도 유리합니다. 결국 RESTful 설계는 개발자 간의 협업 효율과 시스템의 안정성을 높이기 위한 원칙이라고 생각합니다.

## RESTful 하게 API 를 디자인 한다는 것은 무엇을 의미하는가.(요약)

1. '리소스'와 '행위'를 명시적이고 직관적으로 분리한다.

- 리소스는 URI로 표현되는데 리소스가 가리키는 것은 명사로 표현되어야 한다.
- 행위는 HTTP Method로 표현하고, GET(조회), POST(생성), PUT(기존 entity 전체 수정), PATCH(기존 entity 일부 수정), DELETE(삭제)을 분명한 목적으로 사용한다.

2. Message 는 Header 와 Body 를 명확하게 분리해서 사용한다.

- Entity 에 대한 내용은 body 에 담는다.
- 애플리케이션 서버가 행동할 판단의 근거가 되는 컨트롤 정보인 API 버전 정보, 응답받고자 하는 MIME 타입 등은 header 에 담는다.
- header 와 body 는 http header 와 http body 로 나눌 수도 있고, http body 에 들어가는 json 구조로 분리할 수도 있다.

3. API 버전을 관리한다.

- 환경은 항상 변하기 때문에 API 의 signature 가 변경될 수도 있음에 유의하자.
- 특정 API 를 변경할 때는 반드시 하위호환성을 보장해야 한다.

4. 서버와 클라이언트가 같은 방식을 사용해서 요청하도록 한다.

- 브라우저는 form-data 형식의 submit 으로 보내고 서버에서는 json 형태로 보내는 식의 분리보다는 json 으로 보내든, 둘 다 form-data 형식으로 보내든 하나로 통일한다.
- 다른 말로 표현하자면 URI 가 플랫폼 중립적이어야 한다.

어떠한 장점이 존재하는가?

- Open API 를 제공하기 쉽다
- 멀티플랫폼 지원 및 연동이 용이하다.
- 원하는 타입으로 데이터를 주고 받을 수 있다.
- 기존 웹 인프라(HTTP)를 그대로 사용할 수 있다.

단점은 뭐가 있을까?

- 사용할 수 있는 메소드가 한정적이다.
- 분산환경에는 부적합하다.
- HTTP 통신 모델에 대해서만 지원한다.

## REST 원칙 (6가지 제약 조건)

1. 클라이언트-서버 구조 (Client-Server)
   - 클라이언트는 UI/사용자 경험에 집중, 서버는 데이터 처리와 비즈니스 로직에 집중.
   - 서로 독립적으로 발전 가능.
2. 무상태성 (Stateless)
   - 각 요청은 필요한 모든 정보를 담아야 하며, 서버는 클라이언트 상태를 세션으로 저장하지 않음.
   - 서버 확장성과 단순성 증가.
   - 예: 로그인 상태도 토큰(JWT 등)으로 매 요청마다 전달.
3. 캐시 처리 가능 (Cacheable)
   - 응답 데이터에 캐시 가능 여부를 명시(Cache-Control, ETag 등).
   - 네트워크 효율성과 성능 향상.
4. 계층화 시스템 (Layered System)
   - 클라이언트는 최종 서버인지 중간 프록시/게이트웨이인지 알 필요 없음.
   - 보안, 로드밸런싱, 캐싱 계층 추가 가능.
5. 인터페이스 일관성 (Uniform Interface)
   - 리소스 식별: URI를 통해 자원(리소스) 식별.
   - 표현을 통한 리소스 조작: JSON, XML 등 여러 표현 방식 가능.
   - 자기 서술적 메시지(Self-descriptive message): 요청/응답 메시지 구조가 명확해야 함.
   - HATEOAS(Hypermedia As The Engine Of Application State): 응답에 관련 링크를 담아 다음 행동을 안내할 수 있음.
6. 코드 온 디맨드 (Code on Demand, 선택적)
   - 필요 시 서버가 클라이언트에 스크립트/코드를 전달해 실행할 수 있음 (자주 쓰이진 않음).

### REST, REST API, RESTful API

💡 REST

REpresentational State Transfer 의 약자, 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것입니다. HTTP URI를 통해 자원을 명시하고 HTTP 메서드(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD를 적용하는 것을 말합니다. 즉, 자원 기반의 구조 설계의 중심에 자원이 있고, HTTP 메서드를 통해 이를 처리합니다.

💡 REST API

- 말 그대로 REST 아키텍처 스타일을 따른 API.
- 즉, 자원을 URI로 식별하고 HTTP 메서드로 CRUD를 수행하는 인터페이스를 뜻합니다.

💡 RESTful API

- REST 원칙을 충분히 지킨 API를 강조할 때 쓰는 표현.

- 모든 REST API가 RESTful한 것은 아닙니다.

💡 인터페이스

- 프로그램이 서로 통신하고 데이터를 교환하기 위해 사용하는 규칙이나 프로토콜을 의미합니다.

---

[참고]

https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-5/rest-api.md

https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-4/restful-api.md

https://github.com/jbee37142/Interview_Question_for_Beginner/tree/main/Development_common_sense#restful-api
