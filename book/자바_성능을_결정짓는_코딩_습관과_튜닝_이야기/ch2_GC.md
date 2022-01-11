###자바가 GC를 이용해 메모리를 관리하기 때문에 개발자가 메모리를 정리하기 위한 로직을 만들거나/개입해선 안된다





# GC의 두가지 타입

- 마이너 GC : YOUNG 영역에서 발생하는 GC

- 메이저 GC : OLD 영역/Perm 영역에서 발생하는 GC


- GC가 발생하거나 객체가 각 영역에서 타 영역으로 이동시 어플리케이션 병목이 발생 > 성능에 영향


------------


# YOUNG 영역



## 구성 

- Eden 영역
- Survivor 영역(2개)

: Eden  영역 1개 + Survivor 영역 2개를 더해서 총 3개영역이라고 자주 언급됨



## 처리순서

- 새로 생성한 대부분의 객체는 Eden 영역에 위치

- Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역 중 하나로 이동

- Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓임

- 하나의 Survivor 영역이 가득 차게 되면 그 중에서 살아남은 객체를 다른 Survivor 영역으로 이동. 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태.

- 이 과정을 반복하고 살아남아 있는 객체는 Old 영역으로 이동.



cf)  HotSpot VM에서 빠른 메모리 할당을 위해 사용하는 기술 2가지

- bump-the-pointer : 

Eden 영역에 할당된 마지막 객체를 추적한다. 마지막 객체는 Eden 영역의 맨 위(top)에 있다. 그리고 그 다음에 생성되는 객체가 있으면, 해당 객체의 크기가 Eden 영역에 넣기 적당한지만 확인한다. 만약 해당 객체의 크기가 적당하다고 판정되면 Eden 영역에 넣게 되고, 새로 생성된 객체가 맨 위에 있게 된다. 따라서, 새로운 객체를 생성할 때 마지막에 추가된 객체만 점검하면 되므로 매우 빠르게 메모리 할당이 이루어진다.

 그러나 멀티 스레드 환경을 고려하면 이야기가 달라진다. Thread-Safe하기 위해서 만약 여러 스레드에서 사용하는 객체를 Eden 영역에 저장하려면 락(lock)이 발생할 수 밖에 없고, lock-contention 때문에 성능은 매우 떨어지게 될 것이다. HotSpot VM에서 이를 해결한 것이 TLABs.



- TLABs(Thread-Local Allocation Buffers) : 

각각의 스레드가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가질 수 있도록 하는 것. 각 쓰레드에는 자기가 갖고 있는 TLAB에만 접근할 수 있기 때문에, bump-the-pointer라는 기술을 사용하더라도 아무런 락이 없이 메모리 할당이 가능

-------------


# OLD 영역



## GC방식

- Serial GC 시리얼 콜렉터

- Parallel GC 병렬 콜렉터

- Parallel Old GC(Parallel Compacting GC) 

- Concurrent Mark & Sweep GC(이하 CMS)

- G1(Garbage First) GC



## Serial GC (-XX:+UseSerialGC)

운영 서버에서 절대 사용하면 안 되는 방식. 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식이다. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어진다.

적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식



- Mark-sweep-compact 콜렉션 알고리즘을 따름 : 

표시단계 : old 영역으로 이동한 객체들중 살아있는 객체를 식별

스윕단계 : old영역의 객체들을 훑는 작업을 수행, 쓰레기 객체를 식별

컴팩션단계 : 필요없는 객체들을 지우고 살아있는객체들을 모은다





## Parallel GC (-XX:+UseParallelGC)

Parallel GC는 Serial GC와 기본적인 알고리즘은 같다. 그러나 Serial GC는 GC를 처리하는 스레드가 하나인 것에 비해, Parallel GC는 GC를 처리하는 쓰레드가 여러 개이다. 그렇기 때문에 Serial GC보다 빠른게 객체를 처리할 수 있다. Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 유리하다. Parallel GC는 Throughput GC라고도 부른다.

다수의 CPU를 사용하기 때문에  GC 의 부하를 줄이고 어플리케이션의 처리량을 증가시킬 수 있다.





## Parallel Old GC(-XX:+UseParallelOldGC)

Parallel Old GC는 JDK 5 update 6부터 제공한 GC 방식이다. 앞서 설명한 Parallel GC와 비교하여 Old 영역의 GC 알고리즘만 다르다. 이 방식은 Mark-Summary-Compaction 단계를 거친다. Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르며, 약간 더 복잡한 단계를 거친다.

여러개의 CPU를 상뇽하는 서버에 적합하다



- Mark-Summary-Compaction 알고리즘

표시단계 : 살아있는 객체를 식별

종합단계 : 이전에 GC를 수행해 컴팩션된 영역에 살아있는 객체의 위치를 조사하는 단계

컴팩션단계 : 필요없는 객체들을 지우고 살아있는객체들을 모은다. 수행이후에는 컴팩션 영역과 빈 영역으로 나뉨





## CMS GC (-XX:+UseConcMarkSweepGC)

stop-the-world 시간이 매우 짧다. 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용하며, Low Latency GC라고도 부른다.

CMS GC는 stop-the-world 시간이 짧다는 장점에 반해 다음과 같은 단점이 존재한다.

다른 GC 방식보다 메모리와 CPU를 더 많이 사용한다.

Compaction 단계가 기본적으로 제공되지 않는다.

따라서, CMS GC를 사용할 때에는 신중히 검토한 후에 사용해야 한다. 그리고 조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 stop-the-world 시간이 더 길기 때문에 Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인해야 한다.

2개 이상의 프로세서를 사용하는 서버에 적합하다  EX) 웹서버



- Mark-Sweep-Compaction 알고리즘

초기표시단계 :  매우 짧은 대기시간으로 살아있는 객체를 식별

컨커런트 표시 단계 : 서버 수행과 동시에 살아있는 객체에 표시를 해놓는 단계

재표시 단계 : 컨커런트 표시 단계에서 표시하는동안 변경된 객체에 대해 다시 표시하는 단계

컨커런트 스윕단계 : 표시되어 있는 쓰레기를 정리하는 단계





## G1 GC

G1 GC는 바둑판의 각 영역에 객체를 할당하고 GC를 실행한다. 그러다가, 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행한다. 즉, 지금까지 설명한 Young의 세가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식이라고 이해하면 된다. G1 GC는 장기적으로 말도 많고 탈도 많은 CMS GC를 대체하기 위해서 만들어 졌다.

G1 GC의 가장 큰 장점은 성능이다. 지금까지 설명한 어떤 GC 방식보다도 빠르다. 하지만, JDK 6에서는 G1 GC를 early access라고 부르며 그냥 시험삼아 사용할 수만 있도록 한다. 그리고 JDK 7에서 정식으로 G1 GC를 포함하여 제공한다.





### 참고 자료

링크 :
https://d2.naver.com/helloworld/1329