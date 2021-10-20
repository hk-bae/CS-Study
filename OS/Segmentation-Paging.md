# Segmentaion & Paging

* 고정 분할 기법, 동적 분할 기법은 많은 문제점을 가지고 있습니다.

  * 단편화 문제
  * 메모리의 배치와 관리가 어렵다.
  * 프로세스에게 메모리 할당을 늘리거나 줄이기 어렵다.
  * 메모리가 보호받기 어렵다


## Segmentation

* 세그멘테이션은 동적 분할 기법을 확장한 방법으로 **주소 공간을 서로 다른 크기를 가진 논리적 블록(segments)으로 나누어 각 세그먼트를 PA의 연속적인 공간에 배치**하는 것입니다.

* 프로세스와 CPU는 <segment #, offset> 의 형태로 메모리 공간을 바라보게 됩니다.

* 논리 주소는 explicit하게 또는 implicit하게 표현됩니다.
  * explicit : <segment id, offset> ex) <0x01, 0x2a31>
  * implicit : 0x012a31

<img width="637" alt="seg1" src="https://user-images.githubusercontent.com/68215452/138043321-73e854ae-034e-4753-b9c2-72a124830923.png">

* 위 그림은 세그먼트를 Code, Data, Heap,Stack를 기준으로 나누었습니다.

* `세그먼트 테이블`은 세그먼트 번호인 **Seg#**, 세그먼트 길이인 **Limit**, PA의 시작주소인 **Base**, 저장의 진행 방향인 Dir, 가능한 권한인 Prot를 가지고 있습니다. 

* **세그먼트 테이블**은 각각의 프로세스마다 갖게 됩니다. 이러한 세그먼트 테이블들은 CPU의 **segment-table base register(STBR)**에 저장됩니다. 또한 이 정보들은 컨텍스트 스위칭 시에 교체됩니다.

  

* 가상 메모리는 <Seg#, offset> 으로 이루어져 있습니다. 

  

* Data 영역에 접근을 한다면 <1, 0x0362> 를 읽게 됩니다. 세그먼트 테이블에서 정보를 읽어 세그먼트 크기보다 작은지 우선 유효성 검사를 진행합니다. 작다면 Base의 값을 더하여 PA 주소로 변환하여 접근합니다.



* 장점 

  * 세그먼트별로  다양한 보호 비트를 통해 서로 다른 Protection을 설정할 수 있습니다. 

  * 내부 단편화가 발생하지 않습니다. 

    

* 단점

  * 여전히 세그먼트 자체는 연속적인 공간에 할당됩니다. 서로 다른 크기의 세그먼트들이 메모리에 적재되고 제거되는 일이 반복되다 보면, 작은 여유 공간들이 생기기 마련이고 이에 따라 **외부 단편화 문제**가 발생할 수 있습니다.
  
  * 지원하는 세그먼트 숫자의 개수가 많아질 수록 세그먼트 테이블의 크기가 커져 오버헤드가 발생합니다.



## Paging

<img width="660" alt="paging1" src="https://user-images.githubusercontent.com/68215452/138043083-3311d406-7292-412f-b8ab-fb70eca29372.png">

* 프로세스의 물리 주소공간을 비연속적으로 사용할 수 있습니다. 하나의 프로세스가 사용하는 메모리 공간이 연속적이어야 한다는 제약을 없애는 메모리 관리 방법입니다. 

* 물리적인 메모리 공간을 `page frames` 라고 불리는 고정된 크기로 분할합니다.

  * **Physical Address = <Page Frame Number(PFN), Offset>**

* 논리적인 주소 공간을 **frame과 동일한 크기로**  `pages` 라고 불리는 블록으로 분할합니다.

  * **Virtual Address = <Virtual Page Number(VPN), Offset>**
  * 보통 페이지 크기(= 프레임 크기)는 2의 거듭제곱입니다. 일반적으로 512B-8KB의 작은 크기이지만 2MB,1GB같은 큰 페이지로 나누는 경우도 있습니다.

* 하나의 프로세스가 사용하는 공간은 여러 개의 페이지로 나뉘어서 논리적인 메모리 공간에서 관리되고, 개별 페이지는 **순서에 상관없이** 물리적인 메모리 공간에 있는 Frame에 맵핑되어 저장됩니다.

* n 사이즈의 프로그램을 돌리기 위해서 n개의 여유 프레임을 찾아서 프로그램을 로드해야 합니다.

  

* Paging 기법은 page를 고정된 크기로 분할하기 때문에 **내부 단편화** 문제가 존재합니다. 각프로세스는 평균적으로 페이지 크기의 0.5배만큼의 메모리를 사용하지 못합니다.
  * 예를 들어 페이지 크기가 1024B이고 프로세스 A가 3172B의 메모리를 요구한다면 3개의 페이지 프레임을 하고도 100B가 남게됩니다. 따라서 페이지를 하나 더 할당해야 되고 (1024 - 100)인 924B에 대하여 내부 단편화 문제가 발생합니다.
* 그렇다고해서 더 작은 크기의 프레임으로 나누는 것이 항상 좋은 것은 아닙니다. 더 작은 프레임으로 나누게 되면 페이지 테이블에서의 더 많은 맵핑 정보가 필요하게 됩니다.



### Page Tables

* 각 프로세스는 자신의 페이지 테이블을 갖게 됩니다.

* CPU의 **PTBR(Page Table Base Register)**가 페이지 테이블의 base address를 갖고 있습니다. 

  * Context Swiftcging마다 PTBR이 교체되게 되고 따라서 서로 다른 프로세스의 주소공간에 접근할 방법이 없습니다.

* **Page Table은 OS에 의해 관리되고 MMU에 의해 접근됩니다.**

* 각 페이지 당 PTE(Page Table Entry)를 하나씩 갖게 됩니다.

  <img width="544" alt="paging3" src="https://user-images.githubusercontent.com/68215452/138043074-77b9c379-49a5-4778-bdf2-44becbe88caa.png">

  `V` : PTE를 사용할 수 있는지 없는지 Valid 여부, Invalid인 경우 할당되지 않은 주소공간임.

  `R` : 페이지가 접근되었는지 Reference, read/write가 발생할 때 설정됨. 페이징 교체 알고리즘에서 사용된다.

  `M` : page가 수정되었는지 여부. 페이지 교체가 이뤄질 때 수정된 페이지는 swap file에서도 수정이 이뤄져야 하기 때문에 더 많은 비용이 생기게 됩니다. M비트도 R비트와 함께 페이징 교체 알고리즘에 사용됩니다.

  `Prot` : 허용되는 권한 (R,W,X)

  `PFN` : Physical Address의 Frame을 경정하는 PFN

* **VPN(Virtual Page Number)**를 **PFN(Page Frame Number)**로 맵핑 시켜 줍니다.

  <img width="697" alt="paging2" src="https://user-images.githubusercontent.com/68215452/138043078-5255ee87-c566-4aca-8df0-b234749bc134.png">

  1. LA의 V를 통해 Page Table에서 해당 PTE를 찾는다.
  2. 이를 PFN으로 변환하고 offset과 함께 Physical Address에 접근한다.





### Linear Page Table

<img width="655" alt="paging4" src="https://user-images.githubusercontent.com/68215452/138043069-fe63220f-7492-4692-b5f4-51734462db2b.png">

* 모든 virtual Adrress의 Page를 순서대로 다 담아 놓은 page table이다. 즉 vpn 0부터 vpn N까지 쭉 늘려 놓은 것인데 이는 페이지 테이블에 많은 메모리 공간을 요구하기 때문에 잘 사용되지 않는다. 64bit 주소 공간, 8KB 페이지, 8bytes/PTE 기준으로 16PB의 엄청난 메모리 공간을 필요로 하기 때문이다 



* 그렇기 때문에 페이지 테이블의 크기를 줄일 수 있는 설계가 필요하다. 실제 프로세스에서 주소 공간의 일부분만 접근되고 대부분의 공간은 접근조차 하지 않게 됩니다. 따라서 다음과 같은 방안들을 생각해 볼 수 있습니다.

  * 전체 페이지 테이블을 작은 페이지 테이블로 분할한다. 이 때 분할된 페이지 테이블은 하나의 페이지 크기에 들어갈 수 있도록 맞춘다.
  
  * 주소 공간에 실제로 사용되는 부분만 맵핑시키도록 한다.



### Hierarchical page Tables

<img width="601" alt="스크린샷 2021-10-20 오후 4 02 35" src="https://user-images.githubusercontent.com/68215452/138043891-c58e74c1-5c90-45bd-81a3-e749587d7c51.png">

* 위 그림은 Two-level로 페이지를 나누어 둔 것이다. 

* **Outer Page 테이블**과 page table을 나누어, Outer Page table에서 사용되는 페이지만 하위의 page table을 할당해 주는 방식이다.

* 사용하지 않는 page의 경우, page table을 만들지 않아도 되고, outer page table을 통해 손쉽게 관리할 수 있다는 장점이 있다.

* Virtual Addresses = **<outer page#, page#, page offset>** 

  <img width="590" alt="paging5" src="https://user-images.githubusercontent.com/68215452/138043066-d3a4516c-b6aa-4071-9eee-8f5317d6c30d.png">


  * Virtual Address의 outer page#을 통해 Outer Page Table에서 Page Table의 Base Address를 가져온다. 이와 Page #을 조합하여 Page Table내에서 PFN이 들어있는 곳을 찾는다. 해당 PFN과 page offset을 이용하여 physical address를 구할 수 있다.

    <img width="673" alt="paging6" src="https://user-images.githubusercontent.com/68215452/138043058-50ed03d8-c3bf-409c-8f38-b96f03746382.png">

    



* 장점 
  * 작은 단위(page 크기)로 Page Table을 분할함으로써 메모리 관리가 용이히다.
    * Linear Page Table의 경우 그 크기가 매우 크다. 이를 메모리 공간에 연속적으로 저장할 수 있는 공간을 찾는 것도 쉽지 않다.
  * invalid한 페이지를 할당하지 않으므로 메모리를 절약할 수있다.
* 단점
  * page-table lookup 비용이 증가한다.



### Hashed Page Table

<img width="575" alt="paging9" src="https://user-images.githubusercontent.com/68215452/138043024-e6613df7-45bd-45bb-896b-c1d71f550455.png">

* VPN을 PFN으로 변경해주는 Hash Table을 사용하는 방법.
* 해시 테이블의 문제점 : 충돌 문제에 따른 비용 증가



### Inverted Page Table

<img width="456" alt="paging10" src="https://user-images.githubusercontent.com/68215452/138043013-966705a0-53c1-4fbb-8a25-9b319e3eec08.png">

* PFN -> <PID,VPN>을 찾아내는 방법이다. 즉 PA를 보고 VA를 찾아내는 방식이다.
* 페이지 테이블을 Frame의 관점에서 작성한 것이다. 페이지 테이블에서 LA의 (pid,p)를 조회하여 이를 프레임 주소로 변환하는 방식
* 프로세스마다 페이지 테이블을 만들지 않아도 된다는 장점이 있다.
* 단점 : 테이블을 찾는 시간 증가



### TLB(Translation Look-aside Buffer)

* TLB는 Page Table을 통한 주소 변환 과정에 많은 시간이 소요되는 단점을 보완하기 위한 하드웨어 버퍼입니다.
* TLB 엔트리**는 <페이지 번호, 프레임 번호>로** 이루어져 있다.
* TLB에 페이지 넘버가 존재할 경우 별도의 페이지 테이블 조회 없이 프레임 넘버를 얻을 수있다.
* 없을 경우에는 직접 페이지 테이블에서 확인해야 한다.



<img width="695" alt="paging7" src="https://user-images.githubusercontent.com/68215452/138043035-ae43f6dd-8d10-4655-baae-00efaa5f5af0.png">

* TLB miss시 MMU는 페이지 테이블을 확인하고 변환 결과를 TLB에 저장합니다.
* TLB Hit시 페이지 테이블을 거치치 않고 VA를 PA로 변환할 수 있습니다.
* Context Switching 시 TLB를 flush해야 합니다.
  * 각각의 프로세스는 고유의 VA-PA 맵핑이 있기 때문입니다. 따라서 프로세스간 TLB 항목을 공유할 수 없습니다.





### Demad Paging

* 메모리에서 디스크로 전체 프로세스를  **Swap Out**하는 시간은 너무 오래 걸립니다. 따라서 한번에 모든 페이지를 Swap-in 하는 것이 아닌 필요한 페이지만 가져오는 것이 효율적일 수 있습니다.

* **프로그램 실행 시작시에 프로그램 전체를 디스크에서 물리 메모리에 적재하는 대신, 초기에 필요한 것들만 적재하는 전략**입니다. 이는 가상메모리 시스템에서 많이 사용됩니다. 그리고 가상메모리는 대개 페이지로 관리됩니다. 
* Demand Paging을 사용하는 가상메모리에서 실행과정에서 필요해질 때 페이지들이 적재됩니다. **한 번도 접근되지 않은 페이지는 물리 메모리에 적재되지 않습니다.**
* 프로세스 내의 개별 페이지들은 `페이저`에 의해 관리됩니다. 페이저는 프로세스 실행에 실제 필요한 페이지들만 메모리로 읽어옴으로써, **사용되지 않을 페이지를 가져오는 시간 낭비와 메모리 낭비를 줄일 수 있습니다.**



* 처음에 페이지는 물리 메모리 프레임에서 할당됩니다.
* 물리적 메모리가 가득 차면 페이지 할당을 위해 다른 **희생 페이지**를 선정하고 이를 제거해야 합니다.
* 삭제된 페이지는 OS가 디스크의 스왑 파일로 이동시킵니다.



ex.  `P0`, `P1`, `P2` 세 개의 프로세스가 있고 현재 Physical Address가 가득 찬 상황

Main Memory [`p0`,`p0`,`p0`,`p1`,`p2`] 

1. `P2`에서 추가 페이지를 필요로 합니다.

   Main Memory : [`p0`,`p0`,**`p0`**,`p1`,`p2`]

   * OS가 vicitim page를 페이지 교체 알고리즘에 따라 결정합니다.
   * `P0`의 페이지가 선정되었고 이 페이지를 디스크로 swap out 합니다.
   * `P0`의 페이지가 Swap Out 되었음을 해당 PTE에 업데이트 합니다.
     * invalid 설정 및 PFN을 swap entry로 표시
   * `P2`의 페이지를 할당합니다. 
   * Main Memory : [`p0`,`p0`,**`p2`**,`p1`,`p2`]

2. `P0`가 Swap Out된 페이지에 접근합니다. 

   Main Memory : [`p0`,`p0`,`p2`,`p1`,`p2`]

   * PTE에서 page가 swap out 되었다는 것을 확인합니다. -> **page fault**
   * OS가 vicitim page를 페이지 교체 알고리즘에 따라 결정합니다.
     * Main Memory : [`p0`,`p0`,`p2`,**`p1`**,`p2`]
     * 이후 swap out 과정은 1번 과정과 같음
   * OS가 swap out된 페이지를 swap file로 부터 다시 load 합니다.
     * Main Memory : [`p0`,`p0`,`p2`,**`p0`**,`p2`]
   * PTE를 재 수정합니다.
     * valid 설정 및 PFN 수정
   * 해당 페이지로의 접근을 재개합니다.



**Page Fault** **처리 과정**

<img width="623" alt="paging8" src="https://user-images.githubusercontent.com/68215452/138043025-0cacb383-e730-4fcf-9733-91ea4c2fa88b.png">

1. page table에서 페이지 조회
2. PTE의 Valid bit가 Invalid이고 PFN이 swap entry로 되어있다면 Page Fault(**Trap**) 발생
   * PFN이 Null인 경우는 잘못된 페이지 접근
3. O.S가 디스크에 있는 page 확인
4. physical memory에 해당 페이지를 load
5. 페이지 테이블 수정
   * Valid bit를 invalid로 수정
   * PFN을 swap file에 있음으로 표시
6. 명령 재개



**Virtual Memory와 Demand Paging의 장점**

* logical memory를 physical memory로부터 분리하여 생각할 수 있도록 합니다. 메인 메모리를 크고 연속적인 스토리지 배열로 추상화 시켜줍니다. 프로그래머들이 메모리 스토리지 제한에 대한 우려에서 벗어나게 합니다.
* 전체 메모리의 일부만 메인메모리에 올려놓고 프로세스를 실행할 수 있습니다. Logical한 주소 공간은 physical한 주소 공간보다 훨씬 더 클 수 있습니다.
* 여러 프로세스에서 주소 공간을 공유할 수 있습니다.
* 더 많은 프로그램이 동시에 실행될 수 있습니다.
* 프로세스의 load, swap에 대한 I/O가 감소합니다.



### 페이징 교체 알고리즘

Demand Paging에서 언급된대로 프로그램 실행 시에 모든 항목이 물리 메모리에 올라오지 않기 때문에, 프로세스의 동작에 필요한 페이지를 요청하는 과정에서 page fault가 발생하게 되면, 원하는 페이지를 디스크에서 가져오게 된다. 하지만 만약 물리 메모리가 모두 사용중인 상황이라면, 페이지 교체가 이뤄져야 한다.



페이징 교체 알고리즘의 최대 목표는 **page fault rate을 최소화하는 것**입니다. 



**캐시의 지역성 원리**

캐시 메모리는 속도가 빠른 장치와 느린 장치 간의 속도 차에 따른 병목 현상을 줄이기 위한 범용 메모리 입니다. 이러한 역할을 수행하기 위해서는 CPU가 어떤 데이터를 원할 것인가를 어느 정도 예측할 수 있어야 합니다. 캐시의 성능은 작은 용량의 캐시 메모리에 CPU가 이후에 **참조할 쓸모 있는 정보가 어느정도 들어있느냐에 따라 좌우되기 때문**입니다.



이때 적중률을 극대화시키기 위해 데이터의 **지역성 원리**를 사용합니다. 지역성의 전제조건으로 프로그램은 모든 코드나 데이터를 균등하게 Access하지 않는다는 특성을 기본으로 한다. 즉 **Locality**란 기억 장치 내의 정보를 균일하게 Access하는 것이 아닌 어느 한 순간에 특정 부분을 집중적으로 참조하는 특성인 것이다.

이 데이터의 지역성은 대표적으로 **시간 지역성**과 **공간 지역성으로** 나뉜다.

* 시간 지역성 : 최근에 참조된 주소의 내용은 곧 다음에 다시 참조되는 특성
* 공간 지역성 : 대부분의 실제 프로그램이 참조된 주소와 인접한 주소의 내용이 다시 참조되는 특성



* **OPT(Optimal) Page Replacement**

이 알고리즘의 핵심은 **앞으로 가장 오랫동안 사용하지 않을 페이지를 찾아 교체**하는 것이다. 

* 장점 : 알고리즘 중 가장 낮은 페이지 부재율을 보장합니다.
* 단점 : 모든 프로세스의 메모리 참조 계획을 미리 파악하는 것은 불가능합니다. 따라서 이 알고리즘은 다른 알고리즘과의 비교 지표로 많이 사용됩니다.



* **FIFO Page Replacement**

  가장 간단한 페이지 교체 알고리즘으로 FIFO의 흐름을 갖는다. 즉 먼저 물리 메모리에 들어온 페이지 순서대로 페이지 교체 시점에 먼저 나가게 된다.

  * 장점 : 이해하기 쉽고 , 프로그래밍도 쉽다
  * 단점
    * 오래된 페이지가 항상 불필요하지 않은 정보를 포함하지 않을 수 도 있다. (초기 변수 등)
    * 처음부터 활발하게 사용되는 페이지를 교체해서 Page Fault를 높이는 부작용을 초래할 수 있다.
    * `Belady's Anomaly` : 페이지를 저장할 수 있는 페이지 프레임의 갯수를 늘려도 되려 페이지 부재가 더 많이 발생하는 모순이 존재한다.



* **LRU (Least Recently Used) Page Replacement**

  **가장 오랫동안 사용되지 않은 페이지를** 선택하여 교체합니다. 과거의 정보를 이용하여 미래를 예측하는 방법입니다.

  * 대체적으로 `FIFO`보다 우수하고 `OPT`보다는 좋지 않습니다.

  * Belady's Anomlay가 발생하지 않습니다. n frame에 있는 모든 페이지들이 n+1 frame에도 포함되기 때문입니다.

  * `구현1) Clock` 

    메모리 참조 시마다 1씩 증가하는 `clock` 또는 `counter`를 활용하여 구현할 수 있습니다. 모든 페이지 테이블 엔트리는 `time-of-use`를 갖게 합니다. MMU는 특정 페이지 프레임에 접근하게 되면  `time-of-use`를 현재 `clock` 값으로 수정합니다. **Vicitm Page 선정 시 `time-of-use`가 가장 작은 페이지를 선정합니다.** 

    이 방법은 메모리 참조에는 빠르지만 프레임 테이블의 크기가 클 수록 페이지 교체가 느립니다.

  * `구현2) Doubly Linked List or stack`

    페이지 프레임들로 구성된 `이중 연결 리스트`를 활용하는 방법입니다. 메모리 접근 발생 시 해당 페이지 프레임을 연결 리스트의 head로 이동 시킵니다. **Victim Page는 리스트의 tail 부분으로 선정합니다.**

    이 방법은 페이지 교체는 빠르지만 메모리 참조 시 연결 리스트에서 해당 프레임을 탐색하는데 오랜 시간이 걸립니다.



* **LRU Approxiamtion Algorithms**

  LRU의 구현을 완벽하게 할 수 있는 하드웨어는 거의 없습니다. 따라서 완벽한 LRU는 아니지만 이를 근사한 알고리즘을 선택하는 것도 하나의 대안이 될 수 있습니다.

  

  **`Additional Reference-Bits Algorithm`**

  추가적인 `Reference Bit` 를 사용하는 알고리즘입니다. OS는 각 page frame을 위한 n-bits를 저장하는  **in-memory table**을 관리합니다. **일정시간마다 Access 되었던 페이지들의 비트들을 오른쪽으로 shift하는 연산을 수행합니다.** 페이지 프레임 테이블에서 R bit를 가장 높은 차수의 비트로 이동 시키고 가장 낮은 차수의 비트를 버립니다. (ex: R=1, v = 0b 1011010**1** -> v = 0b **1**1011010).  Victim Page를 선정할 때 in-memory table의 v값이 가장 작은 페이지로 선정합니다.

  

  `Second Chance Algorithm`

  이는 clock 알고리즘으로도 알려져 있습니다. 페이지 프레임들을 원형으로 세워 놓고 `clock hand point` 를 통해서 Victim Page를 선정합니다. 마찬가지로 `Reference bit`를 사용하고 **R이 0인 경우 해당 페이지를 희생 페이지를 선정하고 R이 1인 경우 bit를 0으로 초기화 시키고 다음 페이지를 탐색**하는 방식입니다. R이 1일 때 한번의 기회를 더 준다고 하여 second chance 알고리즘이라고 불립니다. 

  

  `Enhanced Second Chance Algorithm`

  추가적으로 Reference bit와 **M(Modify or dirty) bit**를 사용하게 됩니다. 이는 메모리가 수정되었을 때 1이 되게 됩니다. Swap Out을 할 때 메모리 값에 수정이 일어났다면 Swap file에서 수정을 해야 하므로 더 많은 비용이 소요됩니다. **Enhanced Second Chance Algorithm은 R bit와 M bit를 함께 고려하여 희생 페이지를 결정하는 방식**입니다. (R=0,M=0), (R=0,M=1), (R=0,M=1), (R=1,M=1) 순서로 선택되게 됩니다. 

  

  

* **LFU(Least Frequently Used) Page Replacement**

  **참조 횟수가 가장 적은 페이지를 교체**하는 방법입니다. 활발하게 사용되는 페이지는 참조 횟수가 많아질 것이라는 가정에서 만들어진 알고리즘입니다.

  * 어떤 프로세스가 특정 페이지를 집중적으로 사용하다가 다른 기능을 사용하게 되면 더 이상 사용하지 않아도 메모리에 머물게 되어 초기 가정에 어긋나는 시점이 발생할 수 있습니다.

  * OPT를 제대로 근사하지 못하기 때문에, 잘 쓰이지 않습니다.

    

* **MFU(Most Frequently Used)Page Replacement**

  가장 많이 사용된 페이지가 앞으로는 사용되지 않을 것이다.

  * OPT를 제대로 근사하지 못하기 때문에, 잘 쓰이지 않습니다.

## 참고자료

아주대학교 오상은 교수님 강의자료
https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
https://github.com/gyoogle/tech-interview-for-developer




