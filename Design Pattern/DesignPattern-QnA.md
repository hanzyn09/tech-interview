# 디자인 패턴 Q&A

## 디자인 패턴
객체지향 설계에서 자주 발생하는 문제들을 해결하기 위한 재사용 가능한 해결책입니다.

**분류**:
1. **생성 패턴 (Creational)**: 객체 생성 관련
   - Singleton, Factory Method, Abstract Factory, Builder, Prototype

2. **구조 패턴 (Structural)**: 클래스나 객체의 구조 관련
   - Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy

3. **행위 패턴 (Behavioral)**: 객체 간의 상호작용 관련
   - Observer, Strategy, Command, State, Template Method, Visitor, Iterator

**장점**:
- 검증된 설계 방법
- 개발자 간 의사소통 도구
- 코드 재사용성
- 유지보수성 향상

**단점**:
- 복잡성 증가
- 성능 오버헤드 가능
- 과도한 설계 위험

## 싱글톤 패턴
클래스의 인스턴스가 하나만 생성되도록 보장하고, 전역적으로 접근할 수 있는 지점을 제공하는 패턴입니다.

**사용 사례**:
- 로거 (Logger)
- 데이터베이스 연결
- 캐시
- 스레드 풀
- 설정 관리자

**장점**:
- 메모리 절약
- 전역 접근 가능
- 초기화 제어

**단점**:
- 테스트 어려움
- 전역 상태로 인한 결합도 증가
- 멀티스레드 환경에서 동기화 이슈

### 싱글톤 패턴 종류

**1. Eager Initialization (이른 초기화)**
```java
public class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();
    
    private EagerSingleton() {}
    
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```
- 클래스 로딩 시점에 인스턴스 생성
- 스레드 안전
- 사용하지 않아도 메모리 차지

**2. Lazy Initialization (늦은 초기화)**
```java
public class LazySingleton {
    private static LazySingleton instance;
    
    private LazySingleton() {}
    
    public static LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```
- 필요할 때 인스턴스 생성
- 멀티스레드 환경에서 안전하지 않음

**3. Thread Safe Singleton (스레드 안전)**
```java
public class ThreadSafeSingleton {
    private static ThreadSafeSingleton instance;
    
    private ThreadSafeSingleton() {}
    
    public static synchronized ThreadSafeSingleton getInstance() {
        if (instance == null) {
            instance = new ThreadSafeSingleton();
        }
        return instance;
    }
}
```
- synchronized 키워드로 스레드 안전 보장
- 성능 저하 (매번 동기화)

**4. Double-Checked Locking**
```java
public class DoubleCheckedSingleton {
    private static volatile DoubleCheckedSingleton instance;
    
    private DoubleCheckedSingleton() {}
    
    public static DoubleCheckedSingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckedSingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckedSingleton();
                }
            }
        }
        return instance;
    }
}
```
- 성능과 스레드 안전성 모두 고려
- volatile 키워드 필수

**5. Bill Pugh Solution (권장)**
```java
public class BillPughSingleton {
    private BillPughSingleton() {}
    
    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }
    
    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```
- 내부 정적 클래스 활용
- Lazy Loading과 Thread Safety 모두 보장
- 성능 우수

**6. Enum Singleton**
```java
public enum EnumSingleton {
    INSTANCE;
    
    public void doSomething() {
        // 비즈니스 로직
    }
}
```
- 직렬화 안전
- 리플렉션 공격 방어
- 가장 안전한 방법

## Factory Method Pattern
객체 생성을 서브클래스에서 처리하도록 하는 패턴입니다. 구체적인 클래스를 지정하지 않고 객체를 생성할 수 있습니다.

**구조**:
```java
// Creator (추상 생성자)
abstract class Creator {
    public abstract Product factoryMethod();
    
    public void anOperation() {
        Product product = factoryMethod();
        // product 사용
    }
}

// ConcreteCreator (구체적 생성자)
class ConcreteCreator extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProduct();
    }
}

// Product (제품 인터페이스)
interface Product {
    void use();
}

// ConcreteProduct (구체적 제품)
class ConcreteProduct implements Product {
    @Override
    public void use() {
        System.out.println("ConcreteProduct 사용");
    }
}
```

**장점**:
- 객체 생성과 사용의 분리
- 새로운 제품 추가 용이 (OCP 준수)
- 코드 재사용성

**단점**:
- 클래스 수 증가
- 복잡성 증가

## 템플릿 메소드 패턴
알고리즘의 구조를 상위 클래스에서 정의하고, 세부 구현을 하위 클래스에서 처리하는 패턴입니다.

**구조**:
```java
abstract class AbstractClass {
    // 템플릿 메소드
    public final void templateMethod() {
        step1();
        step2();
        step3();
    }
    
    protected abstract void step1();
    protected abstract void step2();
    
    protected void step3() {
        // 기본 구현 (선택적 오버라이드)
        System.out.println("기본 step3 실행");
    }
}

class ConcreteClass extends AbstractClass {
    @Override
    protected void step1() {
        System.out.println("ConcreteClass의 step1");
    }
    
    @Override
    protected void step2() {
        System.out.println("ConcreteClass의 step2");
    }
}
```

**장점**:
- 코드 재사용성
- 알고리즘 구조 통제
- 공통 부분을 상위 클래스에서 관리

**단점**:
- 상속에 의한 강한 결합
- 리스코프 치환 원칙 위반 가능성

**사용 예시**:
- 정렬 알고리즘 (비교 방법만 다름)
- 데이터 처리 파이프라인
- 게임 AI 행동 패턴

## 어댑터 패턴
호환되지 않는 인터페이스를 가진 클래스들이 함께 동작할 수 있도록 하는 패턴입니다.

**구조**:
```java
// Target (클라이언트가 사용하는 인터페이스)
interface Target {
    void request();
}

// Adaptee (적응 대상 클래스)
class Adaptee {
    public void specificRequest() {
        System.out.println("특별한 요청 처리");
    }
}

// Adapter (어댑터 클래스)
class Adapter implements Target {
    private Adaptee adaptee;
    
    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }
    
    @Override
    public void request() {
        adaptee.specificRequest();
    }
}

// 사용
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request();
    }
}
```

**장점**:
- 기존 코드 수정 없이 호환성 제공
- 단일 책임 원칙 준수
- 개방-폐쇄 원칙 준수

**단점**:
- 코드 복잡성 증가
- 추가적인 추상화 레이어

**사용 예시**:
- 레거시 시스템 통합
- 서드파티 라이브러리 통합
- 데이터 형식 변환

## Observer Pattern
객체의 상태 변화를 관찰하는 관찰자들에게 변화를 알려주는 패턴입니다.

**구조**:
```java
// Subject (주제 인터페이스)
interface Subject {
    void registerObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Observer (관찰자 인터페이스)
interface Observer {
    void update(String message);
}

// ConcreteSubject (구체적 주제)
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;
    
    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(state);
        }
    }
    
    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }
}

// ConcreteObserver (구체적 관찰자)
class ConcreteObserver implements Observer {
    private String name;
    
    public ConcreteObserver(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        System.out.println(name + "이 업데이트 받음: " + message);
    }
}
```

**장점**:
- 느슨한 결합
- 개방-폐쇄 원칙 준수
- 런타임에 관찰자 추가/제거 가능

**단점**:
- 메모리 누수 위험 (관찰자 제거 누락)
- 순환 참조 가능성
- 예상치 못한 업데이트 순서

**사용 예시**:
- MVC 패턴
- 이벤트 처리 시스템
- 모델-뷰 바인딩

## Strategy Pattern
동일한 계열의 알고리즘을 정의하고, 각각을 캡슐화하여 상호 교환 가능하게 만드는 패턴입니다.

**구조**:
```java
// Strategy (전략 인터페이스)
interface Strategy {
    int execute(int a, int b);
}

// ConcreteStrategy (구체적 전략들)
class AddStrategy implements Strategy {
    @Override
    public int execute(int a, int b) {
        return a + b;
    }
}

class MultiplyStrategy implements Strategy {
    @Override
    public int execute(int a, int b) {
        return a * b;
    }
}

// Context (전략을 사용하는 클래스)
class Calculator {
    private Strategy strategy;
    
    public Calculator(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public int calculate(int a, int b) {
        return strategy.execute(a, b);
    }
}

// 사용
public class Client {
    public static void main(String[] args) {
        Calculator calculator = new Calculator(new AddStrategy());
        System.out.println(calculator.calculate(5, 3)); // 8
        
        calculator.setStrategy(new MultiplyStrategy());
        System.out.println(calculator.calculate(5, 3)); // 15
    }
}
```

**장점**:
- 알고리즘 교환 용이
- 개방-폐쇄 원칙 준수
- 조건문 제거 가능

**단점**:
- 클래스 수 증가
- 클라이언트가 전략을 알아야 함

**사용 예시**:
- 정렬 알고리즘 선택
- 결제 방법 처리
- 압축 알고리즘 선택

## State Pattern
객체의 내부 상태에 따라 행위를 변경할 수 있게 하는 패턴입니다. 객체가 마치 클래스를 바꾼 것처럼 보이게 합니다.

**구조**:
```java
// State (상태 인터페이스)
interface State {
    void handle(Context context);
}

// ConcreteState (구체적 상태들)
class ConcreteStateA implements State {
    @Override
    public void handle(Context context) {
        System.out.println("State A 처리");
        context.setState(new ConcreteStateB());
    }
}

class ConcreteStateB implements State {
    @Override
    public void handle(Context context) {
        System.out.println("State B 처리");
        context.setState(new ConcreteStateA());
    }
}

// Context (상태를 가지는 클래스)
class Context {
    private State state;
    
    public Context() {
        state = new ConcreteStateA();
    }
    
    public void setState(State state) {
        this.state = state;
    }
    
    public void request() {
        state.handle(this);
    }
}
```

**실제 예시 - 자판기**:
```java
interface VendingMachineState {
    void insertCoin(VendingMachine vm);
    void selectProduct(VendingMachine vm);
    void dispense(VendingMachine vm);
}

class NoCoinState implements VendingMachineState {
    @Override
    public void insertCoin(VendingMachine vm) {
        System.out.println("동전이 투입되었습니다.");
        vm.setState(new HasCoinState());
    }
    
    @Override
    public void selectProduct(VendingMachine vm) {
        System.out.println("먼저 동전을 투입하세요.");
    }
    
    @Override
    public void dispense(VendingMachine vm) {
        System.out.println("먼저 동전을 투입하세요.");
    }
}

class HasCoinState implements VendingMachineState {
    @Override
    public void insertCoin(VendingMachine vm) {
        System.out.println("이미 동전이 투입되어 있습니다.");
    }
    
    @Override
    public void selectProduct(VendingMachine vm) {
        System.out.println("상품이 선택되었습니다.");
        vm.setState(new SoldState());
    }
    
    @Override
    public void dispense(VendingMachine vm) {
        System.out.println("먼저 상품을 선택하세요.");
    }
}

class VendingMachine {
    private VendingMachineState state;
    
    public VendingMachine() {
        state = new NoCoinState();
    }
    
    public void setState(VendingMachineState state) {
        this.state = state;
    }
    
    public void insertCoin() { state.insertCoin(this); }
    public void selectProduct() { state.selectProduct(this); }
    public void dispense() { state.dispense(this); }
}
```

**장점**:
- 상태별 행동을 명확히 분리
- 새로운 상태 추가 용이
- 조건문 대신 다형성 활용

**단점**:
- 클래스 수 증가
- 상태 전환 복잡성

**사용 예시**:
- 게임 캐릭터 상태 (이동, 공격, 방어)
- 네트워크 연결 상태
- 주문 처리 상태 (주문접수, 결제완료, 배송중, 배송완료)

**State vs Strategy 패턴 차이**:
- **State**: 상태에 따라 행동이 완전히 다름, 상태 전환 존재
- **Strategy**: 같은 목적의 다른 알고리즘, 상호 교체 가능

## MVC 패턴
Model-View-Controller의 약자로, 애플리케이션을 세 가지 역할로 구분하여 개발하는 아키텍처 패턴입니다.

**구성 요소**:
- **Model**: 데이터와 비즈니스 로직 처리
- **View**: 사용자 인터페이스 (UI) 담당
- **Controller**: 사용자 입력 처리 및 Model과 View 연결

### MVC 동작 과정
1. **사용자 입력**: Controller가 사용자 요청 받음
2. **모델 호출**: Controller가 Model에서 데이터 처리
3. **뷰 선택**: Controller가 적절한 View 선택
4. **데이터 전달**: Model의 데이터를 View에 전달
5. **화면 출력**: View가 사용자에게 결과 표시

### 코드 예시
```java
// Model
public class User {
    private String name;
    private String email;
    
    // getter, setter, 비즈니스 로직
}

public class UserService {
    public User getUser(Long id) {
        // 데이터베이스에서 사용자 조회
        return new User();
    }
    
    public void saveUser(User user) {
        // 사용자 저장 로직
    }
}

// Controller
@Controller
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/users/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userService.getUser(id);
        model.addAttribute("user", user);
        return "userDetail"; // View 이름
    }
    
    @PostMapping("/users")
    public String saveUser(@ModelAttribute User user) {
        userService.saveUser(user);
        return "redirect:/users";
    }
}

// View (JSP 예시)
// userDetail.jsp
<html>
<body>
    <h1>사용자 정보</h1>
    <p>이름: ${user.name}</p>
    <p>이메일: ${user.email}</p>
</body>
</html>
```

### MVC의 장점
- **관심사 분리**: 각 구성 요소의 역할이 명확
- **재사용성**: Model과 View의 독립적 재사용 가능
- **유지보수성**: 수정 시 다른 부분에 영향 최소화
- **확장성**: 새로운 View나 Controller 추가 용이
- **테스트 용이성**: 각 계층별 단위 테스트 가능

### MVC의 단점
- **복잡성 증가**: 작은 애플리케이션에는 과도할 수 있음
- **학습 비용**: 패턴 이해와 구현에 시간 필요
- **성능 오버헤드**: 계층 간 호출로 인한 성능 저하 가능

### MVC 변형 패턴
**MVP (Model-View-Presenter)**:
- View와 Model이 직접 통신하지 않음
- Presenter가 모든 UI 로직 처리

**MVVM (Model-View-ViewModel)**:
- 데이터 바인딩을 통한 View와 ViewModel 연결
- 주로 WPF, Angular, Vue.js에서 사용

**활용 사례**:
- Spring MVC
- ASP.NET MVC
- Ruby on Rails
- Django (MTV 패턴이지만 MVC와 유사)
