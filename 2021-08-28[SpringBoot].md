# 2021-08-26

# [SpringFramework & Spring Boot]

## SpringFramework

EJB 의 복잡성 > 경량 프레임워크 등장(Hibernate, Spring)

POJO + Framework

- EJB 같은 거창한 컨테이너 필요없다
- 오픈소스
- 많은 라이브러리
- 모든 플랫폼에서 사용 가능
- 웹, 앱 모든 분야에 적용

Spring 삼각형

- POJO (Plain Old Java Object) : 객체지향원리에 충실한 자바객체
- PSA(Portable Service Abstraction) : 일관된 방식으로 기술에 접근하게 하는 설계 원칙
- IoC/DI(Dependency Injection) : 유연하게 확장 가능한 객체를 만들어 두고 객체 간의 의존관계는 외부에서 다이나믹하게 설정
- AOP(Aspect Oriented Programming) : 관심사 분리를 통한 소프트웨어 모듈성 향상, 공통 모듈은 여러 코드 쉽게 적용

#### SpringFramework 특징

1. 경량컨테이너 : 자바 객체를 담고, 생성/소멸을 관리한다
2. DI 패턴 지원 : 객체간 의존관계 설정, 검색이 필요 없다
3. AOP, POJO 지원



### IoC 개념

- 객체 간의 결합도를 떨어뜨릴 수 있다. 높으면 유지 보수가 힘들거든

- 다형성을 통해 결합도 낮춤 - 상속 받으면 상속한 type으로 정의하자

- Factory 패턴 - factory를 만들고 interface로 활용

  이 factory 패턴이 적용된 것이 container의 기능이며 이 container읭 기능을 제공해주고자 하는 것이 IoC모듈이다.

- Assembler를 통해 결합도를 낮춤 - spring container가 외부조립기 역할을 한다.



## Spring Boot

- project에 따라 자주 사용되는 library들이 미리 조합되어 있다
- 복잡한 설정을 자동으로 처리
- 내장 서버를 포함해서 tomcat과 같은 WAS를 추가로 설치하지 않아도 개발가능
- WAS에 배포하지 않고도 실행할 수 있는 JAR파일로 Web Application을  개발할 수 있다.

#### 구성

- java : java source directory

- .java : application을 시작할 수 있는 main method가 존재하는 스프링 구성 메인 클래스

- static : css.js, img등 정적 resource

- templates : springboot에서 사용 가능한 view templates(Thymeleaf, Velocity, FreeMarket)

- application.properties : application 및 스프링 설정에 사용할 여러가지 property

- src/main : jsp등의 리소스 directory

  jsp? html코드에 java를 넣어 실행되면 서블릿으로 변환된다(java내에 html) > WAS에서 동작한다

  



### 회원가입, 조회 확인하기

1. 모든 controller는 실행될 때 생겨난다. 각각 service와 repository와는 bean을 통해 연결하는데 모든 것은 객체로 여겨진다. 즉 bean의 의미와 같이 spring이 실행되면 모든 연결된 java class들이 하나씩 만들어진다

   어디에? 자바 컨테이너 안에!!! 그래서 객체 지향 언어라 부르는 것이다

   **정형화된 것이 아니면 bean으로 연결 관계를 설정하는 것이 좋다**!! 왜? 코드 수정 없이 repository를 바꿀 수 있다.

   스프링 빈은 싱글톤으로 등록한다. 하나만 등록하고 공유한다는 것.

2. 또 중요한 점! 바로 DI dependency Injection

   spring의 특징이라 할 수 있는 성격으로, 객체간 의존 관계를 설정한다. 3가지 방법

   직접 필드 주입, 생성자 주입, setter 주입 - 이중 생성자 주입으로 한다 생각하자. 실행 후에는 변화가 없기 때문이다.

   

## JPA (Java Persistence API)

> ORM을 사용하기 위한 인터페이스를 모아둔 것

인터페이스라 JPA를 구현한 ORM 프레임워크를 사용해야한다. (Hibernate, EclipseLink, DataNucleus)

#### 장점

1. CRUD SQL을 작성할 필요가 없다
2. 조회 결과를 객체로 매핑하는것도 자동으로 처리한다
3. 작성 코드가 대폭 준다
4. 객체 중심 개발이라 생산성과 유지보수가 좋아진다

#### ORM

- Object-Relational-Mapping : 객체가 테이블이 되도록 매핑시켜주는 프레임워크

#### JDBC

- DB에 접근할 수 있도록 JAVA에서 제공하는 API
- 문제
  1. 계속 반복되는 작업
  2. SQL을 다 작성하기 때문에 너무 의존적이다. 수정시 다 수정해야한다

#### JPA의 문제해결

- 직접 SQL을 작성하는 것이 아닌, 객체를 변경하면 SQL 작성 및 처리해준다

1. 저장기능 : persist(member)
2. 조회기능 : jpa.find(Member.class, memberId);
3. 수정기능 : find 후, member.setName()