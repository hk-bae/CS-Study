# 어댑터 패턴

- 호환되지 않는 인터페이스 때문에 함께 동작할 수 없는 클래스들 사이에서 어댑터를 넣어 동작할 수 있도록 만드는 패턴.
- 기존 시스템에 서드파티 라이브러리를 사용할 때나 오래된 인터페이스를 새로운 시스템에 맞출 때 주로 사용되며 코드의 재사용성을 높일 수 있다.

## 어댑터 패턴의 구조

![image](https://user-images.githubusercontent.com/49391145/138266867-f0011908-91a0-4091-828e-0edac2399b94.png)

- **Client** 써드파티 라이브러리나 외부시스템을 사용하려는 쪽이다.
- **Adaptee** 써드파티 라이브러리나 외부시스템
- **Target Interface** Adapter 가 구현(implements) 하는 인터페이스이다. 클라이언트는 Target Interface 를 통해 Adaptee 인 써드파티 라이브러리를 사용하게 된다.
- **Adapter** Client 와 Adaptee 중간에서 호환성이 없는 둘을 연결시켜주는 역할을 담당한다. Target Interface 를 구현하며, 클라이언트는 Target Interface 를 통해 어댑터에 요청을 보낸다. 어댑터는 클라이언트의 요청을 Adaptee 가 이해할 수 있는 방법으로 전달하고, 처리는 Adaptee 에서 이루어진다.

## 어댑터 패턴 호출 과정

![image](https://user-images.githubusercontent.com/49391145/138266902-d8c2e13e-cb09-43dc-a4a9-c38c852d80f2.png)

클라이언트에서는 Target Interface 를 호출하는 것 처럼 보인다. 하지만 클라이언트의 요청을 전달받은 (Target Interface 를 구현한) Adapter 는 자신이 감싸고 있는 Adaptee 에게 실질적인 처리를 위임한다. Adapter 가 Adaptee 를 감싸고 있는 것 때문에 Wrapper 패턴이라고도 불린다.

## 어댑터 패턴 사용예제

- AccountingAdapter

![image](https://user-images.githubusercontent.com/49391145/138266948-df5feb4a-83eb-4b7f-9134-190bee3e476b.png)
![image](https://user-images.githubusercontent.com/49391145/138266974-f472a2ad-a2cf-4054-b326-ea60b5f61c54.png)
- WebRequester
[https://yaboong.github.io/design-pattern/2018/10/15/adapter-pattern/](https://yaboong.github.io/design-pattern/2018/10/15/adapter-pattern/)

### 참고 자료

- [https://yaboong.github.io/design-pattern/2018/10/15/adapter-pattern/](https://yaboong.github.io/design-pattern/2018/10/15/adapter-pattern/)
- [http://vincehuston.org/dp/adapter.html](http://vincehuston.org/dp/adapter.html)
- [https://kim6394.tistory.com/172](https://kim6394.tistory.com/172)
