# Fixed Partion & Variable Partition

* 프로그램이 실행되기 위해서는 Code와 Data가 메인메모리에 올라와야 한다. 

* 각 프로세스는 자신만의 주소공간이 필요하다.

* 하지만 메모리를 계속해서 추가하는 것은 비용의 문제가 발생하기 때문에 우리는 메모리를 관리하기 위한 여러 기법들을 사용한다.

* 메모리에 대한 **Abstraction**, 즉 **Virtual Memory**를 어떻게 구성하고 관리할 것인가에 대한 문제

* 메모리 관리란 프로세스에 메모리를 할당하는 것,  분리된 주소공간에 프로세스의 메모리를 할당하는 것, 프로세스들이 메모리 공간 내에서 재배치될 수 있도록 하는 것, 물리적인 연속적인 주소공간 제공, Fragmentaion, Caching 등의 것들이 해당된다.



## 주소 공간의 표현

### Physical Address(PA) vs Logical Address(LA)

<img width="690" alt="memory1" src="https://user-images.githubusercontent.com/68215452/138045906-36329223-269e-4e99-80f9-5e44c6c0d2bb.png">

* PA는 **실제 메모리에 저장되는 위치**입니다.

* LA는 CPU의 MMU(Memory Management Unit)에 의해서 생성되는 주소이며 프로세스와 CPU는 LA를 가지고 동작합니다. MMU가 LA를 PA로 맵핑시키는 역할을 담당합니다. 이 과정에서 우리는 실제 메모리에 접근하기 위해 LA를 PA로 변환시키기 위한 메커니즘과 자료구조가 필요합니다.

  


## Fixed Partitions (고정 분할 기법)

* 물리적인 메모리를 **같은 크기의 고정 파티션으로 분할**하는 방법입니다.

  * **PA = Base Address + virtual address(LA)**
  
  * 파티션을 넘는 범위로 접근할 수 없습니다. (유효성 검사가 필요하다.)
  
  * 고정된 크기로 파티션을 분할하므로 파티션의 개수는 멀티프로그래밍의 정와 같습니다. 
    * 파티션의 개수가 많을 수록 프로세스를 메모리에 많이 올릴 수 있다.
    
* MMU에 **Base Register**를 두고 프로세스의 Context Switching이 일어날 때 Base Register의 값을 활용하여 물리적인 메모리 공간의 주소를 계산합니다. 이때 LA의 크기가 파티션의 크기보다 커지게 되는지 체크함으로 파티션을 넘는 범위로의 접근을 제한합니다.

* **Base Register 내부의 값은 실제 물리 주소에서 파티션의 시작 위치입니다.**



<img width="721" alt="memory2" src="https://user-images.githubusercontent.com/68215452/138045968-4c5fd3fa-4f8f-43c5-9e32-54e20a5158be.png">

* 프로세스 P2와 P6는 0x0214라는 동일한 논리 주소를 가지며 실행중입니다. 

  

<img width="723" alt="memory3" src="https://user-images.githubusercontent.com/68215452/138045958-24a40144-cc45-47f0-839c-575304f1c90f.png">

* P2에서 메모리에 대한 접근이 일어났고 MMU의 Base Register에서 P2의 Base Address 값인 0x1000을 더하여 PA를 구합니다.

  

<img width="716" alt="memory4" src="https://user-images.githubusercontent.com/68215452/138045949-53291ae3-6edc-4653-993c-03e0684ee553.png">

* 같은 방법으로 P6에 대한 접근도 이루어집니다.



* 장점 
  *  구현하기가 매우 쉽고 단순하게 LA의 값이 파티션 하나의 크기보다 큰지만 체크하면 되기 때문에 접근의 유효성을 검사하는 것이 간단합니다.
  * 컨텍스트 스위칭이 발생할 때 단순하게 Base Register를 교체하면 되기 때문에 빠르게 동작한다.
  
* 단점
  * 하나의 크기로 파티션을 고정하는 것은 모든 프로세스에게 적합하지 않습니다.
  * **내부 단편화**문제



### Internal Fragment (내부 단편화)

* **메모리를 할당할 때 프로세스가 필요한 양보다 더 큰 메모리가 할당되어서 프로세스에서 사용하는 메모리 공간이 낭비되는 현상**

* 리소스(메모리)가 동일한 크기로 나뉘어 졌을 때 각 파티션의 남는 부분이 재사용되지 못하는 문제입니다. 이로 인해 남는 공간이 충분하더라도 새로운 프로세스를 할당하지 못하게 됩니다. 

* 이는 주로 고정된 파티션에서 발생하는 문제입니다.



## Variable Partitions (동적 분할 기법)

* **물리적인 메모리 주소공간을 가변 크기의 파티션으로 분할**하는 방법입니다.

* 프로세스가 사용할 메모리의 크기를 O.S가 미리 알고 있다는 가정이 필요합니다.

* 비어있는 연속된 물리 메모리 공간이 있다면 프로세스를 이에 할당하여 내부 단편화를 해결할 수 있습니다.

* 추가적인 Limit Register를 통해 할당된 메모리의 끝 지점을 저장하고 접근의 유효성을 검사하는데 활용해야 합니다.



<img width="743" alt="memory5" src="https://user-images.githubusercontent.com/68215452/138045936-73cfc7b8-7e80-487b-95af-22f78497219d.png">

1. P1의 LA는 Base Register의 값을 통해 PA가 계산되어집니다.

2. Limit Register의 값을 통해 유효한 값인지 검증합니다.



* 내부 단편화는 해결했지만 동적분할기법은 **외부 단편화** 문제가 발생합니다.



### External Fragmentation (외부 단편화)

<img width="671" alt="memory6" src="https://user-images.githubusercontent.com/68215452/138045932-f2ae2d4d-0352-44d4-ae39-604a9bd90255.png">

* 메모리를 할당되고 해체되는 작업이 반복될 때 **작은 메모리가 중간중간에 존재**하게 됩니다. 이 때 중간중간에 생긴 사용하지 않는 메모리가 많이 존재해서 총 여유 공간은 충분하지만 실제로 할당할 수 없는 문제입니다.

* 주로 파티션의 분할을 가변적으로 적용할 때 발생합니다.



**해결방법**

* 메모리 할당이 **Contiguous**한 경우 : `Compaction(압축)`

  <img width="282" alt="memory7" src="https://user-images.githubusercontent.com/68215452/138045916-1113bfd4-6907-495b-b551-b5fe4dcda4f9.png">

  메모리의 빈 공간을 제거하고 합쳐줍니다. 작업효율이 좋지 않습니다.

* 메모리 할당이 Non-Contiuous한 경우 : `Paging` , `Segmentation`

  <img width="341" alt="memory8" src="https://user-images.githubusercontent.com/68215452/138045921-53786961-537b-4d93-ba14-26c63b31b9d0.png">

  메모리를 더 작은 단위로 나누는 전략



### Variable Partitions 에서 메모리 할당 전략

<img width="703" alt="스크린샷 2021-10-20 오후 4 20 18" src="https://user-images.githubusercontent.com/68215452/138046244-9e2760a3-c53c-40db-88e3-af2093edfb34.png">


* 메모리의 효율적인 사용을 위해 위와 같은 상황에서 Process X를 어느 위치에 삽입할 것인지에 대한 전략이 필요합니다.



1. First fit : 할당 가능한 최초의 위치에 할당 
   * first fit 전략을 사용하게 되면 외부 단편화에 의해 50%의 메모리를 사용하지 못하게 됩니다.
   
2. Best fit : 할당 가능한 공간 중 가장 작은 공간에 할당

3. Worst fit : 할당 가능한 공간 중 가장 큰 공간에 할당



* 어떤 방법이 가장 좋은가 시뮬레이션을 돌려본 결과 First Fit과 Best Fit이 Worst Fit보다는 우수하다고 합니다. First fit과 Best fit은 성능면에서 크게 차이가 없습니다. 

* 그럼에도 First-fit을 기준으로 한 통계에서 50%의 메모리를 사용하지 못하게 됩니다. 




## 참고자료

* 아주대학교 오상은 교수님 강의자료



