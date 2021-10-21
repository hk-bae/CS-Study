# 싱글톤 패턴

- 애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당(static)하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴
- 즉, 정확히 하나의 인스턴스만 생성해 사용하는 패턴.
- 인스턴스 생성은 외부에서 이루어질 수 없고, 싱글톤 패턴으로 정의된 클래스의 내부에서만 생성되도록 제한한다.
- 싱글톤으로 구현한 인스턴스는 전역으로, 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능하다.

![image](https://user-images.githubusercontent.com/49391145/138267298-6f24d501-fa65-49b5-9bcc-655360beafad.png)
<br/>
## 싱글톤 패턴 사용 시 주의할 점

- 만약 싱글톤 인스턴스가 혼자 너무 많은 일을 하거나, 많은 데이터를 공유시키면 다른 클래스들 간의 결합도가 높아지게 되는데, 이때 객체 지향 설계 원칙 중 `개방-폐쇄 원칙`이 위배된다.
- 결합도가 높아지게 되면, 유지보수가 힘들고 테스트도 원활하게 진행할 수 없는 문제점이 발생한다.
- 또한, 멀티 스레드 환경에서 동기화 처리를 하지 않았을 때, 인스턴스가 2개가 생성되는 문제도 발생할 수 있다.

싱글톤 패턴을 구현할 땐 위의 사항들을 주의해야 하며 반드시 싱글톤이 필요한 상황이 아니면 지양하는 것이 좋다고 한다.

<br/>

## 싱글톤 패턴 구현 방법

1. **Lazy Initialization (초기화 지연)**
- synchronized 동기화를 활용해 스레드를 안전하게 만듦 (하지만, synchronized는 큰 성능저하를 발생시키므로 권장되지 않음)
2. **Lazy Initialization + Double-checked Locking**
- 1번의 성능 저하를 완화시키는 방법.
- 1번과 달리, 먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성하는 방법
3. **Initialization on demand holder idiom (holder에 의한 초기화)**
- 클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한 방법
- JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘기는 걸 활용함
<br/>

(자세한 예시 코드 [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design Pattern/Singleton Pattern.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/Singleton%20Pattern.md))

![image](https://user-images.githubusercontent.com/49391145/138267548-6f0d0fab-4105-4f74-9481-a49c89cb9dfa.png)
![image](https://user-images.githubusercontent.com/49391145/138267570-9525d92a-8a2c-4024-a480-803a0a774029.png)
<br/>

### 참고 자료

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design Pattern/Singleton Pattern.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/Singleton%20Pattern.md)
