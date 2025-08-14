# 기타 개발 Q&A

## Git
분산 버전 관리 시스템으로, 소스 코드의 변경 이력을 추적하고 관리하는 도구입니다.

**주요 개념**:
- **Repository**: 프로젝트가 저장되는 저장소
- **Commit**: 변경사항을 저장하는 단위
- **Branch**: 독립적인 개발 라인
- **Merge**: 브랜치를 합치는 작업
- **Pull Request**: 코드 리뷰 및 머지 요청

**기본 명령어**:
```bash
# 저장소 복제
git clone <repository_url>

# 변경사항 스테이징
git add <file>
git add .  # 모든 변경사항

# 커밋
git commit -m "커밋 메시지"

# 원격 저장소에 푸시
git push origin <branch_name>

# 원격 저장소에서 풀
git pull origin <branch_name>

# 브랜치 생성 및 전환
git checkout -b <new_branch>
git switch <branch_name>  # Git 2.23+

# 브랜치 병합
git merge <branch_name>

# 상태 확인
git status
git log
```

**Git Flow**:
- **main/master**: 배포 가능한 안정된 코드
- **develop**: 개발 중인 코드
- **feature**: 새로운 기능 개발
- **release**: 배포 준비
- **hotfix**: 긴급 수정

**Best Practices**:
- 의미 있는 커밋 메시지 작성
- 작은 단위로 자주 커밋
- 브랜치 전략 수립
- 코드 리뷰 프로세스 도입

## Docker
애플리케이션을 컨테이너화하여 배포하고 실행할 수 있게 해주는 플랫폼입니다.

**핵심 개념**:
- **이미지**: 컨테이너 실행에 필요한 파일과 설정값
- **컨테이너**: 이미지를 실행한 상태
- **Dockerfile**: 이미지 빌드 명령어들을 담은 텍스트 파일
- **Registry**: 이미지를 저장하고 공유하는 저장소

**주요 명령어**:
```bash
# 이미지 빌드
docker build -t <image_name> .

# 컨테이너 실행
docker run -d -p 8080:80 <image_name>

# 실행 중인 컨테이너 확인
docker ps

# 컨테이너 중지
docker stop <container_id>

# 이미지 목록 확인
docker images

# 컨테이너 로그 확인
docker logs <container_id>

# 컨테이너 내부 접속
docker exec -it <container_id> /bin/bash
```

**Dockerfile 예시**:
```dockerfile
FROM openjdk:11-jre-slim

WORKDIR /app

COPY target/app.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
```

**Docker Compose**:
여러 컨테이너를 정의하고 실행하는 도구
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
  
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

**Docker vs VM**:
| 구분 | Docker | VM |
|------|--------|----|
| 가상화 수준 | OS 수준 | 하드웨어 수준 |
| 리소스 사용 | 적음 | 많음 |
| 시작 속도 | 빠름 | 느림 |
| 격리 수준 | 프로세스 수준 | 완전 격리 |

## 테스트
소프트웨어의 품질을 보장하기 위해 코드가 예상대로 동작하는지 검증하는 과정입니다.

**테스트 레벨**:
1. **단위 테스트 (Unit Test)**: 개별 함수나 메서드 테스트
2. **통합 테스트 (Integration Test)**: 모듈 간 상호작용 테스트
3. **시스템 테스트 (System Test)**: 전체 시스템 테스트
4. **인수 테스트 (Acceptance Test)**: 사용자 요구사항 충족 테스트

**테스트 종류**:
- **기능 테스트**: 요구사항 만족 여부
- **성능 테스트**: 응답 시간, 처리량
- **보안 테스트**: 보안 취약점 검사
- **사용성 테스트**: 사용자 경험

**JUnit 예시** (Java):
```java
@Test
public void testCalculator() {
    // Given
    Calculator calculator = new Calculator();
    
    // When
    int result = calculator.add(2, 3);
    
    // Then
    assertEquals(5, result);
}

@Test
public void testDivideByZero() {
    Calculator calculator = new Calculator();
    
    assertThrows(ArithmeticException.class, () -> {
        calculator.divide(10, 0);
    });
}
```

**테스트 더블**:
- **Mock**: 호출되는 메서드와 반환값을 미리 정의
- **Stub**: 특정 입력에 대해 미리 준비된 답변 반환
- **Spy**: 실제 객체의 일부 메서드만 모킹
- **Fake**: 실제 구현체의 간단한 버전

**TDD (Test-Driven Development)**:
1. **Red**: 실패하는 테스트 작성
2. **Green**: 테스트를 통과하는 최소한의 코드 작성
3. **Refactor**: 코드 개선 및 중복 제거

## 알고리즘
문제를 해결하기 위한 명확하고 체계적인 절차나 방법입니다.

**시간 복잡도**:
알고리즘의 실행 시간이 입력 크기에 따라 어떻게 변하는지를 나타냅니다.
- **O(1)**: 상수 시간
- **O(log n)**: 로그 시간 (이진 탐색)
- **O(n)**: 선형 시간 (순차 탐색)
- **O(n log n)**: 선형 로그 시간 (효율적인 정렬)
- **O(n²)**: 제곱 시간 (중첩 반복문)
- **O(2ⁿ)**: 지수 시간 (피보나치 재귀)

**공간 복잡도**:
알고리즘이 사용하는 메모리 공간의 복잡도입니다.

**주요 알고리즘 분류**:

### 정렬 알고리즘
| 알고리즘 | 시간복잡도 (평균) | 시간복잡도 (최악) | 공간복잡도 | 안정성 |
|----------|-------------------|-------------------|------------|--------|
| 버블 정렬 | O(n²) | O(n²) | O(1) | 안정 |
| 선택 정렬 | O(n²) | O(n²) | O(1) | 불안정 |
| 삽입 정렬 | O(n²) | O(n²) | O(1) | 안정 |
| 퀵 정렬 | O(n log n) | O(n²) | O(log n) | 불안정 |
| 병합 정렬 | O(n log n) | O(n log n) | O(n) | 안정 |
| 힙 정렬 | O(n log n) | O(n log n) | O(1) | 불안정 |

### 탐색 알고리즘
- **순차 탐색**: O(n) - 정렬되지 않은 배열
- **이진 탐색**: O(log n) - 정렬된 배열
- **해시 탐색**: O(1) 평균 - 해시 테이블

### 그래프 알고리즘
- **DFS**: 깊이 우선 탐색 - O(V + E)
- **BFS**: 너비 우선 탐색 - O(V + E)
- **다익스트라**: 최단 경로 - O((V + E) log V)
- **플로이드-워셜**: 모든 쌍 최단 경로 - O(V³)

### 동적 프로그래밍 (DP)
복잡한 문제를 작은 하위 문제로 나누어 해결하는 기법입니다.

**특징**:
- **최적 부분 구조**: 부분 문제의 최적해가 전체 문제의 최적해에 포함
- **중복되는 부분 문제**: 같은 부분 문제가 반복적으로 계산됨

**구현 방법**:
- **Top-down (메모이제이션)**: 재귀 + 캐싱
- **Bottom-up (타뷸레이션)**: 반복문으로 작은 문제부터 해결

## 개발 방법론
소프트웨어 개발 과정을 체계적으로 관리하기 위한 방법과 절차입니다.

### 소프트웨어 개발 생명주기 (SDLC)
1. **요구사항 분석**: 사용자 요구사항 파악
2. **설계**: 시스템 아키텍처 및 상세 설계
3. **구현**: 실제 코드 작성
4. **테스트**: 버그 발견 및 수정
5. **배포**: 운영 환경에 시스템 설치
6. **유지보수**: 지속적인 개선 및 수정

### 개발 방법론 종류

#### 폭포수 모델 (Waterfall)
각 단계를 순차적으로 진행하는 전통적인 방법론입니다.

**장점**: 체계적, 문서화 잘됨, 관리 용이
**단점**: 유연성 부족, 늦은 피드백, 변경 어려움

#### 애자일 (Agile)
변화에 빠르게 대응하고 고객과의 협력을 중시하는 방법론입니다.

**애자일 선언문**:
- 개인과 상호작용 > 프로세스와 도구
- 작동하는 소프트웨어 > 포괄적인 문서
- 고객과의 협력 > 계약 협상
- 변화에 대응 > 계획을 따르기

**스크럼 (Scrum)**:
- **스프린트**: 1-4주의 개발 주기
- **일일 스탠드업**: 매일 15분 진행 상황 공유
- **스프린트 리뷰**: 스프린트 결과 리뷰
- **회고**: 개선점 도출

**칸반 (Kanban)**:
- 작업을 시각화하여 흐름 관리
- WIP(Work In Progress) 제한
- 지속적인 개선

#### DevOps
개발(Development)과 운영(Operations)의 협력을 강조하는 문화와 방법론입니다.

**핵심 원칙**:
- **CI/CD**: 지속적 통합/배포
- **인프라스트럭처 as 코드**: 코드로 인프라 관리
- **모니터링**: 실시간 시스템 상태 감시
- **자동화**: 반복 작업의 자동화

**CI/CD 파이프라인**:
1. **소스 코드 관리**: Git 등 버전 관리
2. **빌드**: 컴파일 및 패키징
3. **테스트**: 자동화된 테스트 실행
4. **배포**: 스테이징/프로덕션 환경 배포
5. **모니터링**: 애플리케이션 상태 감시

### 코드 품질 관리

**코딩 컨벤션**:
- 일관된 네이밍 규칙
- 들여쓰기 및 포맷팅
- 주석 작성 가이드
- 파일 및 디렉토리 구조

**코드 리뷰**:
- 버그 조기 발견
- 코드 품질 향상
- 지식 공유
- 팀 협업 강화

**정적 분석 도구**:
- **SonarQube**: 코드 품질 분석
- **ESLint**: JavaScript 코딩 스타일 검사
- **PMD**: Java 코드 분석
- **Checkstyle**: Java 코딩 컨벤션 검사

### 프로젝트 관리 도구
- **JIRA**: 이슈 트래킹 및 프로젝트 관리
- **Trello**: 칸반 보드 기반 작업 관리
- **Asana**: 팀 협업 및 작업 관리
- **Slack**: 팀 커뮤니케이션
- **Confluence**: 문서 협업
