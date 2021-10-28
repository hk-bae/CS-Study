# 템플릿 메소드 패턴


- 알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴
- 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
- 즉, 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화 할 때 유용하다.
- 예를 들어, 전체적인 알고리즘은 상위 클래스에서 구현하면서 다른 부분은 하위 클래스에서 구현할 수 있도록 함으로써 전체적인 알고리즘 코드를 재사용하는 데 유용하도록 한다.
- 행위(Behavioral) 패턴 중 하나 (객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴)
<br/>

## **템플릿 메소드 패턴 구조**

![https://blog.kakaocdn.net/dn/8iyLF/btqZpfdHFNT/EJCpNFiBBWNkcKlohpPU5k/img.png](https://blog.kakaocdn.net/dn/8iyLF/btqZpfdHFNT/EJCpNFiBBWNkcKlohpPU5k/img.png)

- AbstractClass
    - 템플릿 메서드를 정의하는 클래스
    - 하위 클래스가 공통으로 갖고있는 알고리즘을 정의하고 하위 클래스에서 구현될 기능을 primitive 메서드 또는 hook 메서드로 정의하는 클래스
- ConcreteClass
    - 물려받은 primitive 메서드 또는 hook 메서드를 구현하는 클래스
    - 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 primitive 메서드나 hook 메서드를 오버라이드하는 클래스
    
<br/>
## **템플릿 메소드 패턴 예제**

- 아이스 아메리카노, 아이스 라떼 만드는 법

[https://www.crocus.co.kr/1531](https://www.crocus.co.kr/1531)

- Wooden house, Glass house

[https://niceman.tistory.com/142](https://niceman.tistory.com/142)

동일한 부분은 Abstract Class에 정의한다.  조금씩 다른 부분 Abstract Class에서 추상화해 정의하여 Concrete Class에서 오버라이딩하여 변형한다.

<br/>

## **템플릿 메소드 패턴 장단점**

### **장점**

**1.** 중복코드를 줄일 수 있다.

**2.** 자식 클래스의 역할을 줄여 핵심 로직의 관리가 용이하다.

**3.** 좀더 코드를 객체지향적으로 구성할 수 있다.

### **단점**

**1.** 추상 메소드가 많아지면서 클래스 관리가 복잡해진다.

**2.** 클래스간의 관계와 코드가 꼬여버릴 염려가 있다.

<br/>
<br/>

## **출처**
- https://niceman.tistory.com/142
- https://www.crocus.co.kr/1531
- https://yaboong.github.io/design-pattern/2018/09/27/template-method-pattern/
