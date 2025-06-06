<h1><center> Unit Testing Chapter 3 - TIL </center></h1>

###### tags: `💻 TIL`
###### date: `2024-06-0617:21:33.284Z`

> [color=#724cd1][name=데릭]

## Chapter 3 - 단위 테스트의 구조와 실용적인 지침들

### 1. 단위 테스트의 구조는 어떻게 짜야 할까?

> 기본은 AAA 패턴(Arrange, Act, Assert). 익숙해지면 테스트를 훨씬 쉽게 읽고 유지보수할 수 있음.

> Given-When-Then 패턴도 비슷한 구조인데, 비개발자에게 더 친숙한 표현이라는 차이가 있음.

> TDD에서는 오히려 Assert부터 쓰는 게 자연스럽다는 점도 인상 깊었음.

####❓ TDD에선 Assert부터 시작하는 게 좋다고 하는데, 현실에서 그렇게 작성하는 게 정말 실용적일까? 테스트 시나리오가 모호할 땐 오히려 Arrange부터 손이 가는 경우가 많은데.

### 2. 한 테스트엔 단 하나의 행동만 담아야 한다

> Arrange, Act, Assert 중 하나라도 두 번 이상 등장하면 단일 행동이 아님 → 테스트를 쪼개야 함.

> 특히 Act가 여러 개인 테스트는 사실상 통합 테스트가 되어버림.

> Assert가 여러 개라도 괜찮지만, 너무 많으면 리팩터링 신호.

####❓ 하나의 테스트가 여러 결과를 assert하는 건 괜찮다는데, 어느 정도까지 허용되는 걸까? 예: 객체의 여러 필드를 전부 검증해야 할 때는?

### 3. if문이나 조건문이 테스트에 있으면 거의 무조건 안티패턴
테스트는 단순한 순차 실행이어야 하며, 조건 분기는 테스트 분리로 해결해야 함.

테스트 안에서 로직이 존재하면 테스트 자체를 검증해야 하는 상황이 벌어짐.

####❓ 조건문을 피하라고 하는데, 가끔 파라미터화된 테스트에서 기대값이 달라질 때는 어떻게? (예: expected == true or false)

### 4. Act가 한 줄이 아니면 SUT의 API 설계에 문제가 있을 수 있다
행동(Act) 코드가 여러 줄이면 캡슐화가 깨졌다는 신호.

불변 조건(invariant)이 무너질 수 있는 구조를 방지하려면, public 메서드로 모든 작업을 캡슐화해야.

####❓ 이런 리팩터링은 언제 시도해야 할까? 제품 출시 직전에는 부담될 수 있음. 테스트에서 감지한 냄새만으로 설계를 바꾸는 게 항상 정답일까?

### 5. Test fixture는 생성자 말고 팩토리 메서드로 추출하자
생성자에 설정을 넣으면 테스트 간 결합도가 높아지고, 가독성이 떨어짐.

팩토리 메서드 패턴을 써서 각 테스트 내부에서 명시적으로 상태를 설정하는 게 더 낫다.

####❓ Test fixture가 진짜 모든 테스트에서 동일한 경우에도 생성자에 넣지 말아야 할까? 통합 테스트에선 공통 설정이 많아서 고민될 때가 많음.

### 6. 테스트 이름은 시스템의 행동을 설명하는 문장처럼 지어야 한다

> [Method]_[Condition]_[ExpectedResult] 같은 기계적인 네이밍은 오히려 유지보수에 해가 될 수 있음.

> 대신 자연어 문장처럼 읽히도록 지어야 함. 예: Delivery_with_a_past_date_is_invalid

> should나 Returns 같은 표현도 피하고, 결과를 사실처럼 진술해야 함.

####❓ 이런 네이밍이 코드 자동 생성 도구나 IDE 기능과 충돌하지는 않을까? 너무 긴 이름은 실무에서 불편할 수도 있는데, 이건 결국 트레이드오프?

### 7. 비슷한 테스트는 파라미터화해서 관리하자
> 비슷한 테스트를 줄일 수 있지만, 가독성 저하라는 단점도 있음.

> 특히 입력만 다르고 의미가 명확한 경우에만 사용하는 게 좋음.

> 너무 복잡한 경우에는 오히려 개별 테스트가 더 낫다.

####❓ 긍정 케이스와 부정 케이스를 같은 파라미터화 테스트에 넣는 건 좋은가? 헷갈릴 여지가 많아 보임.

### 8. Fluent Assertions처럼 읽기 쉬운 어설션 도구를 써라

- Assert.Equal(expected, actual)보다 result.Should().Be(30)이 더 자연스럽고 직관적임.

- 코드가 문장처럼 읽히는 것이 이상적.

- 부가적인 의존성이라는 단점은 있지만, 테스트에 한정되므로 큰 문제는 아님.

####❓ Fluent Assertions을 쓰면 익숙하지 않은 팀원들은 혼란스러워하지 않을까? 온보딩 비용이 생길 수 있는데 도입 시점을 어떻게 잡아야 할까?

**💬 정리하면서 든 생각들**

이 챕터는 단위 테스트를 어떻게 '작성'할지가 아니라, 어떻게 '의미 있게 구성'할지를 이야기하는 느낌이었다.

테스트도 결국 코드라는 사실을 잊지 말고, 가독성과 유지보수성을 항상 고려해야 한다는 점이 핵심임.

