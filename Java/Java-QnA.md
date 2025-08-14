# Java Q&A

## 객체지향프로그래밍이란?
프로그램을 객체들의 집합으로 보는 프로그래밍 패러다임입니다. 현실 세계의 개념을 객체로 모델링하여 프로그램을 구성합니다.

### 객체지향 프로그래밍 4가지 특징

1. **캡슐화(Encapsulation)**
   - 데이터와 메서드를 하나의 단위로 묶고 외부에서의 접근을 제한
   - 정보 은닉을 통한 보안성 향상

2. **상속(Inheritance)**
   - 부모 클래스의 특성을 자식 클래스가 물려받음
   - 코드 재사용성과 확장성 제공

3. **다형성(Polymorphism)**
   - 같은 인터페이스로 다양한 형태의 객체를 다룰 수 있음
   - 오버로딩, 오버라이딩으로 구현

4. **추상화(Abstraction)**
   - 복잡한 구현 내용을 숨기고 필요한 기능만 제공
   - 인터페이스와 추상 클래스로 구현

### 객체지향 설계 5대 원칙 (SOLID)

1. **SRP (Single Responsibility Principle)**: 단일 책임 원칙
2. **OCP (Open-Closed Principle)**: 개방-폐쇄 원칙
3. **LSP (Liskov Substitution Principle)**: 리스코프 치환 원칙
4. **ISP (Interface Segregation Principle)**: 인터페이스 분리 원칙
5. **DIP (Dependency Inversion Principle)**: 의존 역전 원칙

## 추상 클래스와 인터페이스의 차이

| 구분 | 추상 클래스 | 인터페이스 |
|------|-------------|------------|
| 키워드 | abstract class | interface |
| 상속 | extends (단일 상속) | implements (다중 구현) |
| 메서드 | 추상/구현 메서드 모두 가능 | 추상 메서드만 (Java 8+ default 메서드 가능) |
| 변수 | 일반 변수 가능 | public static final만 가능 |
| 생성자 | 생성자 가능 | 생성자 불가능 |
| 접근제어자 | 모든 접근제어자 사용 가능 | public만 가능 |

## 객체지향, 함수형의 차이

| 구분 | 객체지향 | 함수형 |
|------|----------|--------|
| 기본 단위 | 객체 | 함수 |
| 상태 관리 | 상태 변경 가능 | 불변성 추구 |
| 부작용 | 부작용 허용 | 부작용 최소화 |
| 데이터 처리 | 객체 메서드 | 고차 함수 |
| 예시 | Java, C++ | Haskell, Scala |

## 오버로딩 오버라이딩

### 오버로딩 (Overloading)
- **같은 이름의 메서드**를 **매개변수의 타입, 개수, 순서**를 다르게 하여 여러 개 정의
- **컴파일 타임**에 결정 (정적 바인딩)
- 반환 타입은 관계없음

```java
public void print(int x) { }
public void print(String x) { }
public void print(int x, int y) { }
```

### 오버라이딩 (Overriding)
- **상위 클래스의 메서드**를 **하위 클래스에서 재정의**
- **런타임**에 결정 (동적 바인딩)
- 메서드 시그니처가 동일해야 함

```java
class Parent {
    public void method() { }
}
class Child extends Parent {
    @Override
    public void method() { } // 오버라이딩
}
```

## 자바 애플리케이션, JVM 실행 과정

1. **소스 코드 작성**: .java 파일 생성
2. **컴파일**: javac로 .class 파일(바이트코드) 생성
3. **클래스 로딩**: ClassLoader가 .class 파일을 JVM 메모리에 로드
4. **바이트코드 검증**: Bytecode Verifier가 안전성 검사
5. **인터프리터/JIT 컴파일**: 바이트코드를 기계어로 변환
6. **실행**: main 메서드부터 프로그램 실행

## 자바 메모리 구조

### JVM 메모리 영역
1. **메서드 영역 (Method Area)**
   - 클래스 정보, 상수, static 변수 저장
   - 모든 스레드가 공유

2. **힙 영역 (Heap Area)**
   - 객체와 배열 저장
   - GC의 주요 대상
   - 모든 스레드가 공유

3. **스택 영역 (Stack Area)**
   - 지역변수, 매개변수, 메서드 호출 정보
   - 스레드별로 독립적

4. **PC 레지스터**
   - 현재 실행 중인 명령어 주소
   - 스레드별로 독립적

5. **네이티브 메서드 스택**
   - JNI를 통한 네이티브 메서드 호출 시 사용

### 클래스 로드 방법
1. **Bootstrap ClassLoader**: 핵심 Java API 로드
2. **Extension ClassLoader**: 확장 클래스 로드
3. **System ClassLoader**: 애플리케이션 클래스패스의 클래스 로드

### 클래스 로드 과정
1. **로딩(Loading)**: 클래스 파일을 메모리에 로드
2. **링킹(Linking)**
   - 검증: 바이트코드 유효성 검사
   - 준비: static 변수 메모리 할당
   - 해석: 심볼릭 레퍼런스를 직접 레퍼런스로 변환
3. **초기화(Initialization)**: static 블록 실행, static 변수 초기화

## 가비지 컬렉션이란?
JVM이 자동으로 사용하지 않는 메모리 객체를 해제하는 메커니즘입니다.

**장점**: 메모리 누수 방지, 개발자의 메모리 관리 부담 감소
**단점**: GC 실행 시 일시적 성능 저하

### 가비지 컬렉션 일어나는 과정

1. **Mark**: 사용 중인 객체 마킹
2. **Sweep**: 마킹되지 않은 객체 제거
3. **Compact**: 메모리 단편화 제거 (선택적)

**GC 영역**:
- **Young Generation**: Eden, Survivor 영역
- **Old Generation**: 오래된 객체 저장
- **Minor GC**: Young Generation에서 발생
- **Major GC**: Old Generation에서 발생

### GC 모니터링이란
GC 성능을 측정하고 분석하는 과정입니다.

**모니터링 도구**:
- jstat, jvisualvm, GCViewer
- GC 로그 분석

**주요 지표**:
- GC 빈도, GC 시간, 메모리 사용량

## 예외 처리
프로그램 실행 중 발생할 수 있는 오류 상황을 처리하는 메커니즘입니다.

**예외 계층구조**:
```
Throwable
├── Error (시스템 레벨 오류)
└── Exception
    ├── RuntimeException (Unchecked Exception)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── IllegalArgumentException
    └── Checked Exception
        ├── IOException
        ├── SQLException
        └── ClassNotFoundException
```

**Checked vs Unchecked Exception**:
- **Checked**: 컴파일 시점에 처리 강제, try-catch 또는 throws 필요
- **Unchecked**: 런타임에 발생, 처리 선택적

**예외 처리 문법**:
```java
try {
    // 예외 발생 가능한 코드
} catch (SpecificException e) {
    // 특정 예외 처리
} catch (Exception e) {
    // 일반 예외 처리
} finally {
    // 항상 실행되는 코드
}
```

**try-with-resources** (Java 7+):
```java
try (FileReader file = new FileReader("file.txt")) {
    // 파일 처리
} catch (IOException e) {
    // 예외 처리
} // 자동으로 close() 호출
```

## 멀티스레딩
하나의 프로세스 내에서 여러 스레드를 동시에 실행하는 프로그래밍 기법입니다.

**스레드 생성 방법**:
1. **Thread 클래스 상속**:
```java
class MyThread extends Thread {
    @Override
    public void run() {
        // 스레드 실행 코드
    }
}
```

2. **Runnable 인터페이스 구현** (권장):
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 스레드 실행 코드
    }
}
```

3. **람다식 사용** (Java 8+):
```java
Thread thread = new Thread(() -> {
    // 스레드 실행 코드
});
```

**스레드 상태**:
- **NEW**: 생성되었지만 시작되지 않음
- **RUNNABLE**: 실행 중 또는 실행 대기
- **BLOCKED**: 동기화 블록 대기
- **WAITING**: 다른 스레드 대기
- **TIMED_WAITING**: 일정 시간 대기
- **TERMINATED**: 실행 완료

**동기화 메커니즘**:
- **synchronized**: 메서드나 블록 동기화
- **volatile**: 변수의 가시성 보장
- **Lock**: 더 세밀한 제어 가능
- **Atomic 클래스**: 원자성 보장

**스레드 풀**:
```java
ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> {
    // 작업 내용
});
executor.shutdown();
```

## 컬렉션 프레임워크
Java에서 데이터 집합을 저장하고 조작하기 위한 통합된 아키텍처입니다.

**주요 인터페이스**:
- **List**: 순서 있는 중복 허용 (ArrayList, LinkedList, Vector)
- **Set**: 중복 불허용 (HashSet, TreeSet, LinkedHashSet)
- **Map**: 키-값 쌍 (HashMap, TreeMap, LinkedHashMap)
- **Queue**: FIFO 구조 (PriorityQueue, ArrayDeque)

## Vector와 ArrayList의 차이

| 구분 | Vector | ArrayList |
|------|--------|-----------|
| 동기화 | 동기화됨 (thread-safe) | 동기화 안됨 |
| 성능 | 상대적으로 느림 | 빠름 |
| 크기 증가 | 2배씩 증가 | 1.5배씩 증가 |
| 도입 시기 | Java 1.0 | Java 1.2 |

## HashSet, TreeSet, LinkedHashSet 차이

| 구분 | HashSet | TreeSet | LinkedHashSet |
|------|---------|---------|---------------|
| 정렬 | 순서 없음 | 자동 정렬 | 삽입 순서 유지 |
| 성능 | O(1) | O(log n) | O(1) |
| 내부 구조 | 해시 테이블 | Red-Black Tree | 해시 테이블 + 연결 리스트 |
| null 허용 | 1개 허용 | 불허용 | 1개 허용 |

## HashMap, LinkedHashMap, HashTable, TreeMap 차이

| 구분 | HashMap | LinkedHashMap | HashTable | TreeMap |
|------|---------|---------------|-----------|---------|
| 동기화 | 안됨 | 안됨 | 됨 | 안됨 |
| 순서 | 순서 없음 | 삽입 순서 유지 | 순서 없음 | 키 기준 정렬 |
| null | 허용 | 허용 | 불허용 | 값만 허용 |
| 성능 | O(1) | O(1) | O(1) | O(log n) |

## Servlet 개념, 동작순서, 생명주기

### 개념
웹 서버에서 실행되는 Java 프로그램으로, HTTP 요청을 처리하고 응답을 생성합니다.

### 동작순서
1. 클라이언트 HTTP 요청
2. 웹 서버가 서블릿 컨테이너에 요청 전달
3. 서블릿 컨테이너가 HttpServletRequest, HttpServletResponse 객체 생성
4. 해당 서블릿의 service() 메서드 호출
5. doGet() 또는 doPost() 메서드 실행
6. 응답 객체를 통해 클라이언트에 응답

### 생명주기
1. **init()**: 서블릿 초기화 (최초 한 번)
2. **service()**: 요청 처리 (요청마다 호출)
3. **destroy()**: 서블릿 종료 시 호출

## 정적변수와 전역변수의 차이
Java에는 전역변수가 없고, 정적변수가 전역변수와 유사한 역할을 합니다.

**정적변수 (static)**:
- 클래스 레벨에서 선언
- 모든 인스턴스가 공유
- 클래스 로딩 시 메모리에 할당
- 클래스명으로 접근 가능

## 접근제어자에 대해 설명

| 접근제어자 | 클래스 내부 | 패키지 내부 | 하위 클래스 | 패키지 외부 |
|------------|-------------|-------------|-------------|-------------|
| private | O | X | X | X |
| default | O | O | X | X |
| protected | O | O | O | X |
| public | O | O | O | O |

### 접근제어자 사용하는 이유
- **캡슐화**: 데이터 은닉을 통한 정보 호
- **보안성**: 외부에서의 무분별한 접근 차단
- **유지보수성**: 내부 구현 변경 시 외부 영향 최소화
- **인터페이스 명확화**: 외부에 제공할 기능만 공개

## 직렬화(Serialize)란?
객체를 바이트 스트림으로 변환하는 과정입니다.

**용도**:
- 네트워크 전송
- 파일 저장
- 메모리 간 데이터 교환

**구현 방법**:
```java
// Serializable 인터페이스 구현
public class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private int age;
}
```

**주의사항**:
- serialVersionUID 관리
- transient 키워드로 직렬화 제외
- static, transient 필드는 직렬화되지 않음

## Wrapper Class
기본 타입(primitive type)을 객체로 감싸는 클래스입니다.

| 기본 타입 | Wrapper Class |
|-----------|---------------|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

**필요성**:
- 컬렉션에 기본 타입 저장
- 제네릭 사용
- null 값 표현
- 유틸리티 메서드 제공

## String은 래퍼클래스인데 == 비교시 값 같게 나오는 이유
String은 엄밀히 말하면 래퍼 클래스가 아니라 참조 타입입니다.

**String Pool** 때문입니다:
```java
String s1 = "hello";  // String Pool에 저장
String s2 = "hello";  // 같은 참조 반환
System.out.println(s1 == s2);  // true

String s3 = new String("hello");  // 새로운 객체 생성
System.out.println(s1 == s3);  // false
```

- 리터럴로 생성한 String은 String Pool에서 관리
- 같은 값의 리터럴은 같은 참조를 가짐
- new로 생성하면 새로운 객체 생성

## 제네릭이란?
컴파일 시점에 타입을 명시하여 타입 안정성을 보장하는 기능입니다.

**장점**:
- 컴파일 시 타입 체크
- 형변환 불필요
- 코드 재사용성 향상

```java
// 제네릭 사용 전
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);  // 형변환 필요

// 제네릭 사용 후
List<String> list = new ArrayList<>();
list.add("hello");
String s = list.get(0);  // 형변환 불필요
```

**제네릭 타입**:
- `<T>`: Type
- `<E>`: Element
- `<K, V>`: Key, Value
- `<?>`: 와일드카드

## final이란?
변경할 수 없음을 나타내는 키워드입니다.

### 사용 위치별 의미
1. **변수**: 상수 (재할당 불가)
```java
final int x = 10;  // x 값 변경 불가
```

2. **메서드**: 오버라이딩 불가
```java
public final void method() { }  // 하위 클래스에서 재정의 불가
```

3. **클래스**: 상속 불가
```java
public final class FinalClass { }  // 상속 불가 (예: String, Integer)
```

## 자바 버전

| 버전 | 출시년도 | 주요 특징 |
|------|----------|-----------|
| Java 8 (LTS) | 2014 | Lambda, Stream API, Optional |
| Java 9 | 2017 | Module System (Jigsaw) |
| Java 10 | 2018 | var 키워드 |
| Java 11 (LTS) | 2018 | String 메서드 추가, HTTP Client |
| Java 17 (LTS) | 2021 | Sealed Classes, Pattern Matching |
| Java 21 (LTS) | 2023 | Virtual Threads, String Templates |

**LTS (Long Term Support)**: 장기 지원 버전
**릴리즈 주기**: 6개월마다 새 버전 출시
