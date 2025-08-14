# Spring Q&A

## 스프링이란?
Java 엔터프라이즈 애플리케이션 개발을 위한 오픈소스 프레임워크입니다. 의존성 주입(DI)과 관점 지향 프로그래밍(AOP)을 기반으로 합니다.

**주요 특징**:
- 경량 컨테이너
- IoC/DI 지원
- AOP 지원
- POJO 기반
- 트랜잭션 관리
- 다양한 API 연동

### IoC란?
Inversion of Control(제어의 역전)의 약자로, 객체의 생성과 생명주기 관리를 개발자가 아닌 스프링 컨테이너가 담당하는 것입니다.

**기존 방식**:
```java
public class UserService {
    private UserDAO userDAO = new UserDAO(); // 직접 생성
}
```

**IoC 적용**:
```java
public class UserService {
    private UserDAO userDAO; // 스프링이 주입
}
```

**장점**:
- 결합도 감소
- 테스트 용이성
- 유연한 구조

### DI란?
Dependency Injection(의존성 주입)의 약자로, IoC를 구현하는 방법 중 하나입니다. 외부에서 객체를 생성하여 필요한 곳에 주입합니다.

**DI 방법**:
1. **생성자 주입** (권장)
```java
@Service
public class UserService {
    private final UserDAO userDAO;
    
    public UserService(UserDAO userDAO) {
        this.userDAO = userDAO;
    }
}
```

2. **Setter 주입**
```java
@Service
public class UserService {
    private UserDAO userDAO;
    
    @Autowired
    public void setUserDAO(UserDAO userDAO) {
        this.userDAO = userDAO;
    }
}
```

3. **필드 주입**
```java
@Service
public class UserService {
    @Autowired
    private UserDAO userDAO;
}
```

### 디스패처 서블릿이란?
Spring MVC의 중앙 집중식 요청 처리 서블릿입니다. 모든 HTTP 요청을 받아 적절한 컨트롤러로 분배합니다.

**역할**:
- 요청 URL 매핑
- 핸들러 매핑
- 뷰 리졸버 관리
- 예외 처리

### 디스패처 서블릿으로 인한 web.xml 역할 축소
**기존 web.xml**:
- 서블릿 매핑
- 필터 설정
- 리스너 설정
- 보안 설정

**Spring MVC 이후**:
- DispatcherServlet 하나만 등록
- 나머지 설정은 Spring 설정 파일로 이관
- Java Config 사용 시 web.xml 완전 제거 가능

### AOP란?
Aspect-Oriented Programming(관점 지향 프로그래밍)의 약자로, 횡단 관심사를 분리하여 모듈화하는 프로그래밍 패러다임입니다.

**횡단 관심사**: 로깅, 보안, 트랜잭션, 예외처리 등

**장점**:
- 코드 중복 제거
- 비즈니스 로직과 부가 기능 분리
- 유지보수성 향상

### AOP 용어 설명

- **Aspect**: 횡단 관심사를 모듈화한 것
- **Target**: AOP가 적용될 대상 객체
- **Advice**: 실제 부가 기능을 담은 구현체
- **JoinPoint**: Advice가 적용될 위치
- **Pointcut**: JoinPoint의 상세한 스펙을 정의
- **Weaving**: Pointcut에 의해 결정된 Target의 JoinPoint에 Advice를 삽입하는 과정

**Advice 종류**:
- `@Before`: 메서드 실행 전
- `@After`: 메서드 실행 후
- `@AfterReturning`: 메서드 정상 실행 후
- `@AfterThrowing`: 예외 발생 시
- `@Around`: 메서드 실행 전후

### AOP 사용 경험: 공통 로그 구현

```java
@Aspect
@Component
public class LoggingAspect {
    
    @Around("execution(* com.example.service.*.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
        
        String methodName = joinPoint.getSignature().getName();
        String className = joinPoint.getTarget().getClass().getSimpleName();
        
        log.info("[{}] {}.{} 시작", startTime, className, methodName);
        
        try {
            Object result = joinPoint.proceed();
            long endTime = System.currentTimeMillis();
            log.info("[{}] {}.{} 완료 ({}ms)", endTime, className, methodName, endTime - startTime);
            return result;
        } catch (Exception e) {
            log.error("[{}] {}.{} 실패: {}", System.currentTimeMillis(), className, methodName, e.getMessage());
            throw e;
        }
    }
}
```

### 필터와 인터셉터 차이

| 구분 | Filter | Interceptor |
|------|--------|-------------|
| 관리 컨테이너 | 웹 컨테이너 | Spring 컨테이너 |
| 실행 시점 | DispatcherServlet 전후 | Controller 전후 |
| Spring Bean 접근 | 어려움 | 가능 |
| 용도 | 인코딩, 보안, 로깅 | 인증, 권한 검사 |
| 설정 방법 | web.xml | Spring 설정 |

**요청 흐름**:
`Filter → DispatcherServlet → Interceptor → Controller`

### 스프링 MVC 구조 흐름

1. **클라이언트 요청**: HTTP 요청 발생
2. **DispatcherServlet**: 요청 접수
3. **HandlerMapping**: 요청 URL에 맞는 컨트롤러 검색
4. **HandlerAdapter**: 컨트롤러 실행
5. **Controller**: 비즈니스 로직 처리
6. **ModelAndView**: 결과 데이터와 뷰 정보 반환
7. **ViewResolver**: 뷰 이름을 실제 뷰 객체로 변환
8. **View**: 모델 데이터를 렌더링
9. **응답**: 클라이언트에게 결과 전송

## 스프링과 스프링 부트 차이

| 구분 | Spring | Spring Boot |
|------|--------|-------------|
| 설정 | XML/Java Config 수동 설정 | 자동 설정 (Auto Configuration) |
| 의존성 관리 | 개별 의존성 관리 | Starter 의존성 제공 |
| 서버 | 외부 서버 필요 | 내장 서버 (Tomcat, Jetty) |
| 설정 파일 | 복잡한 설정 | application.properties/yml |
| 개발 시간 | 오래 걸림 | 빠른 개발 |

## 스프링 어노테이션

### 어노테이션 종류

**핵심 어노테이션**:
- `@Component`: 스프링 빈 등록
- `@Service`: 비즈니스 로직 처리
- `@Repository`: 데이터 접근 계층
- `@Controller`: 웹 요청 처리
- `@RestController`: REST API 컨트롤러

**DI 관련**:
- `@Autowired`: 의존성 자동 주입
- `@Qualifier`: 특정 빈 지정
- `@Primary`: 우선 순위 빈 지정
- `@Value`: 프로퍼티 값 주입

**웹 관련**:
- `@RequestMapping`: URL 매핑
- `@GetMapping`, `@PostMapping`: HTTP 메서드별 매핑
- `@RequestParam`: 요청 파라미터 바인딩
- `@PathVariable`: URL 경로 변수 바인딩
- `@RequestBody`: JSON 요청 바디 바인딩
- `@ResponseBody`: JSON 응답 반환

**설정 관련**:
- `@Configuration`: 설정 클래스
- `@Bean`: 빈 등록 메서드
- `@EnableAutoConfiguration`: 자동 설정 활성화
- `@SpringBootApplication`: Spring Boot 애플리케이션

## 스프링 버전별 기능

| 버전 | 주요 기능 |
|------|-----------|
| Spring 2.0 | XML 네임스페이스, AspectJ 지원 |
| Spring 2.5 | 어노테이션 기반 설정 |
| Spring 3.0 | Java Config, SpEL, REST 지원 |
| Spring 3.1 | 프로파일, 캐시 추상화 |
| Spring 4.0 | Java 8 지원, WebSocket |
| Spring 5.0 | Reactive Programming, Kotlin 지원 |
| Spring 6.0 | Java 17+, Jakarta EE |

## MVC 1과 MVC 2의 차이

### MVC 1 (JSP 중심)
- **구조**: JSP에서 뷰와 컨트롤러 역할 모두 수행
- **장점**: 단순한 구조, 빠른 개발
- **단점**: 코드 재사용 어려움, 유지보수 힘듦

### MVC 2 (서블릿 중심)
- **구조**: 서블릿(Controller), JSP(View), JavaBean(Model) 분리
- **장점**: 역할 분담, 유지보수 용이, 재사용성
- **단점**: 복잡한 구조, 개발 시간 증가

## ORM
Object-Relational Mapping의 약자로, 객체와 관계형 데이터베이스 간의 매핑을 자동화하는 기술입니다.

**장점**:
- 객체 지향적 코드
- 데이터베이스 독립성
- 생산성 향상

**단점**:
- 성능 이슈
- 복잡한 쿼리 작성 어려움
- 학습 비용

## JPA
Java Persistence API의 약자로, Java에서 ORM을 위한 표준 스펙입니다.

**주요 구성 요소**:
- **EntityManager**: 엔티티 관리
- **JPQL**: 객체 지향 쿼리 언어
- **Criteria API**: 타입 안전한 쿼리
- **Metamodel**: 엔티티 메타 정보

**어노테이션**:
- `@Entity`: 엔티티 클래스
- `@Id`: 기본키
- `@GeneratedValue`: 자동 생성
- `@Column`: 컬럼 매핑
- `@OneToMany`, `@ManyToOne`: 연관관계

## ORM, JPA, Hibernate 장단점

### ORM
**장점**: 생산성, 유지보수성, 데이터베이스 독립성
**단점**: 성능, 복잡한 쿼리, 학습 비용

### JPA
**장점**: 표준화, 벤더 독립성, 풍부한 기능
**단점**: 복잡성, 성능 튜닝 어려움

### Hibernate
**장점**: 성숙한 기술, 풍부한 기능, 캐싱
**단점**: 무거운 프레임워크, 복잡한 설정

## Mybatis란?
SQL 매핑 프레임워크로, SQL을 XML이나 어노테이션으로 분리하여 관리할 수 있게 해주는 퍼시스턴스 프레임워크입니다.

**특징**:
- SQL 직접 작성
- 동적 SQL 지원
- 결과 매핑 자동화
- 저장 프로시저 지원

## Mybatis, JPA 차이

| 구분 | MyBatis | JPA |
|------|---------|-----|
| 접근 방식 | SQL 중심 | 객체 중심 |
| SQL 작성 | 직접 작성 | 자동 생성 |
| 학습 비용 | 낮음 | 높음 |
| 복잡한 쿼리 | 용이 | 어려움 |
| 성능 | 빠름 | 상대적으로 느림 |
| 데이터베이스 독립성 | 낮음 | 높음 |

## Spring JDBC
Spring에서 제공하는 JDBC 추상화 레이어입니다.

**주요 클래스**:
- **JdbcTemplate**: 반복 코드 제거
- **NamedParameterJdbcTemplate**: 이름 기반 파라미터
- **SimpleJdbcInsert**: 간단한 INSERT 작업

**장점**:
- 예외 처리 자동화
- 리소스 관리 자동화
- 반복 코드 제거

```java
@Repository
public class UserRepository {
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public List<User> findAll() {
        String sql = "SELECT * FROM users";
        return jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
    }
}
```

## DAO, DTO 란?

### DAO (Data Access Object)
데이터베이스 접근을 담당하는 객체입니다.

**특징**:
- 데이터베이스 연결 로직
- CRUD 메서드 제공
- 비즈니스 로직과 분리

```java
@Repository
public class UserDAO {
    public void save(User user) { }
    public User findById(Long id) { }
    public List<User> findAll() { }
    public void delete(Long id) { }
}
```

### DTO (Data Transfer Object)
계층 간 데이터 전송을 위한 객체입니다.

**특징**:
- 데이터 전송 목적
- Getter/Setter 메서드
- 비즈니스 로직 없음

```java
public class UserDTO {
    private String name;
    private String email;
    
    // getter, setter
}
```

**DAO vs DTO**:
- **DAO**: 데이터 접근 책임
- **DTO**: 데이터 전송 책임
