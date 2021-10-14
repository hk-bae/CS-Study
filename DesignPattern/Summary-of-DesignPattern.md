# 디자인 패턴 개요

## 디자인 패턴이란

소프트웨어를 설계할 때 특정 상황에서 자주 발생하는 문제들을 해결하는 해결책.

패턴은 공통의 언어를 만들어주며 팀원 사이의 의사 소통을 원활하게 해주는 아주 중요한 역할을 한다.

<br>

## 객체지향 프로그래밍 설계 원칙 (SOLID)

디자인 패턴들은 이 SOLID 원칙을 기반으로 한다. 이 5원칙들을 잘 알아두면 디자인 패턴이 왜 그렇게 만들어 졌는지 좀 더 잘 이해할 수 있다.

1. **SRP: Single Responsibility Principle 단일 책임의 원칙**
    
    하나의 클래스는 하나의 책임만(역할만) 가진다
    
    ex) UserSetting 클래스에 chnageUserName(), changeUserAge() 같은 함수는 허용하나 verifyAgeUpTo19 같은 나이 인증 함수를 포함하는 것은 SRP 원칙에 위배된다.
    
    ([https://black-jin0427.tistory.com/192](https://black-jin0427.tistory.com/192))

<br>
    
2. **OCP: Open - Close Principle 다형성 설계**
변경에는 닫혀있고 확장에는 열려있어야 한다.
코드는 변경하지 않아야하고, 확장은 쉽게 할 수 있어야 한다.
    
    *ex) 학생 증명서 발급, 캐릭터 움직임*
    
    [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ljh0326s&logNo=221113248565](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ljh0326s&logNo=221113248565)
    
    **OCP가 위반되는 주요 경우**
    
    (1) 다운 캐스팅이 필요한 경우
    
    (2) 비슷한 if-else 블록이 존재하는 경우
    
    *ex) 동물 클래스 울음 소리 [https://black-jin0427.tistory.com/192](https://black-jin0427.tistory.com/192)*
    

<br>
    
3. **LSP: Liskov Substitution Principle 리스코프 치환법칙**
    
    부모 클래스는 언제든 자식 클래스로 교체될 수 있어야 한다.
    
    ```
     Tiger s = new Tiger();
     Animal a = (Animal) s;
    ```
    
    명시된 명세에서 벗어나면 LSP를 위반했다고 볼 수 있다. 사전에 약속한대로 구현해야하고, 상속 시 부모에서 구현한 원칙을 따라야 한다.
    
    LSP가 제대로 지켜지지 않으면 OCP역시 지켜지지 않는다.
    
    *ex)  사각형 클래스를 상속받는 정사각형 클래스*
    
    사각형이 정해놓은 명세를 정사각형은 지킬 수 없다. 업캐스트 후 사각형인 줄 알고 메소드를 호출했다간 예상과 다른 결과가 나올 수 있다.
    
    [https://wkdtjsgur100.github.io/solid-principle/](https://wkdtjsgur100.github.io/solid-principle/)
    
    [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ljh0326s&logNo=221113248565](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ljh0326s&logNo=221113248565)

<br>

    
4. **ISP: Interface Segregation Principle 인터페이스 분리 원칙**
    
    클라이언트가 자신이 사용하지 않는 인터페이스에는 영향을 받지 말아야 한다.
    
    범용 인터페이스 하나 보다, 특정 클라이언트를 위한 인터페이스 여러 개로 분리하는 것이 좋다.
    
    *ex) 복합기 [https://bottom-to-top.tistory.com/27](https://bottom-to-top.tistory.com/27)*

<br>

    
5. **DIP: Dependency Inversion Property 의존관계 역전 원칙**
    
    추상화에 의존해야지, 구체화에 의존하면 안된다.
    
    거의 변화가 없는 것에 의존하는 것이 좋다.
    
    상위클래스는 하위클래스에 의존해서는 안된다.
    
    인터페이스나 추상 클래스와 의존 관계를 맺도록 설계해야 한다.
    
    *ex) 아이와 장난감, logger
    [https://black-jin0427.tistory.com/192](https://black-jin0427.tistory.com/192)*
    
<br>

## 디자인 패턴의 종류

### GoF 디자인 패턴

- GoF 디자인 패턴이란
    
    1995년 GoF(Gang of Four)라고 불리는 Erich Gamma, Richard Helm, Ralph Johnson, John Vissides가 처음으로 디자인 패턴을 구체화하였다. GoF의 디자인 패턴은 소프트웨어 공학에서 가장 많이 사용되는 디자인 패턴이다.
    
- GoF 디자인 패턴의 분류
    
    ![https://user-images.githubusercontent.com/49391145/137137179-7eff433e-bd4d-423a-84c2-d199eaf09079.png](https://user-images.githubusercontent.com/49391145/137137179-7eff433e-bd4d-423a-84c2-d199eaf09079.png)
    
    - 생성 패턴 (Creational patterns) : 객체의 생성
        
        객체의 생성과 관련된 패턴. 객체의 생성과 참조 과정을 캡슐화하여 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 받지 않도록 하여 프로그램에 유연성을 더해준다.
        
        *ex) DBConnection을 관리하는 Instance를 하나만 만들 수 있도록 제한하여, 불필요한 연결을 막음.* 
    
<br>
        
    - 구조 패턴 (Structural patterns) : 객체나 오브젝트 간의 관계
        
        클래스나 객체를 조합해 더 큰 구조를 만드는 패턴.
        
        *ex) 2개의 인터페이스가 서로 호환이 되지 않을 때, 둘을 연결해주기 위해서 새로운 클래스를 만들어서 연결시킬 수 있도록 함.* 

<br>
        
    - 행동 패턴 (Behavioral patterns) : 객체나 오브젝트 사이의 행위
        
        객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴.
        
        클래스나 객체들이 서로 상호작용하는 방법이나 어떤 작업을 어떤 클래스로 어떻게 할당할지 정의하는 패턴이다. 행위 패턴은 하나의 객체로 수행할 수 없는 작업을 여러 객체로 분배하면서 그들 간의 결합도를 최소화 할 수 있도록 도와준다.
        
        *ex) 하위 클래스에서 구현해야 하는 함수 및 알고리즘들을 미리 선언하여, 상속시 이를 필수로 구현하도록 함.*
<br>
https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/%5BDesign%20Pattern%5D%20Overview.md
https://4z7l.github.io/2020/12/25/design_pattern_GoF.html
https://github.com/WeareSoft/tech-interview/blob/master/contents/designpattern.md#%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%A2%85%EB%A5%98

