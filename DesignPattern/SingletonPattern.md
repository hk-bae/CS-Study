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
1. **Eager Initialization (Early Loaing)**

``` 
public class EagerSingleton {
    private static EagerSingleton instance = new EagerSingleton();
    
    // private constructor
    private EagerSingleton() {
    }
    
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```
- 클래스 초기화시 instance 생성
- **단점: 클라이언트에서 사용하지 않아도 instance를 항상 생성**

<br/>
<br/>


2. **Lazy Initialization**


``` 
public class LazyInitializedSingleton {
    private static LazyInitializedSingleton instance;
    
    private LazyInitializedSingleton() {}
    
    public static LazyInitializedSingleton getInstance(){
        if(Objects.isNull(instance)) {
            instance = new LazyInitializedSingleton();
        }
        return instance;
    }
}
```
- 1의 Eager Initialization의 단점을 보완한 방법. 
- 객체 생성을 getInstance 메소드를 이용해서만 가능하게 하여, 클라이언트가 사용하지 않으면 생성하지 않도록 만듦.

- **하지만 Thread-Safe하지 않음.**
  - 여러 개의 thread가 동시에 getInstance()를 호출하는 상황에서 각자 instance가 null이라 판단하게 되고, 여러 개의 인스턴스가 생성되게 된다. 

</br>


2-1. **Thread-Safe Singleton**
- getInstance()앞에 synchronized를 붙여 Thread-Safe하게 만듦

- **단점: synchronized 사용시 큰 성능저하 발생**


<br/>

2-2. **Thread-Safe + Double Checked Locking**

``` 
public class ThreadSafe_Lazy_Initialization{
    private volatile static ThreadSafe_Lazy_Initialization instance;

    private ThreadSafe_Lazy_Initialization(){}

    public static ThreadSafe_Lazy_Initialization getInstance(){
    	if(instance == null) {
        	synchronized (ThreadSafe_Lazy_Initialization.class){
                if(instance == null){
                    instance = new ThreadSafe_Lazy_Initialization();
                }
            }
        }
        return instance;
    }
}
```
- 1번과 달리, 먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성하는 방법

- 인스턴스를 레퍼런스하는 변수에 volatile 을 사용해줘야 한다. (jdk5 이상에서만 유효)
- 스레드의 working memory와 메인메모리의 데이터 이동과정 때문에 동기화가 진행되는 동안 빈틈이 생기게 되므로 volatile을 이용한다.
- volatile 로 선언된 변수는 아래와 같은 기능을 한다.
  - 각 스레드가 해당 변수의 값을 메인 메모리에서 직접 읽어온다.
  - volatile 변수에 대한 각 write 는 즉시 메인 메모리로 플러시 된다.
  - 스레드가 변수를 캐시하기로 결정하면 각 read/write 시 메인 메모리와 동기화 된다.

- **단점: 2-1보다 성능저하를 완화 시켰으나 완벽한 방법은 아니다.**

<br/>
<br/>

3. **Initialization on demand holder idiom (holder에 의한 초기화)**

``` 
public class Something {
    private Something() {
    }
 
    private static class LazyHolder {
        public static final Something INSTANCE = new Something();
    }
 
    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

- 클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한 방법
- JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘김
- JVM에게 싱글톤의 초기화 문제에 대한 책임을 넘겨 synchronized를 사용하지 않아도 되고, 1의 단점을 해결함

<br/>
<br/>

### 출처

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design Pattern/Singleton Pattern.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/Singleton%20Pattern.md)
- https://yaboong.github.io/design-pattern/2018/09/28/thread-safe-singleton-patterns/
- https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom
