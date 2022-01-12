# collection

Collection : 가장 상위 인터페이스

ㄴ Set : 중복 허용하지 않는 집합을 처리하는 인터페이스

    ㄴ sortedSet : 오름차순을 갖는 Set 인터페이스
ㄴ List : 순서가 있는 집합을 처리하기 위한 인터페이스

ㄴ Queue : 여러개의 객체를 처리하기 전에 담아 처리할때 사용하기 위한 인터페이스

Map : 키와 값의 쌍으로 구성된 객체의 집합을 처리하기 위한 인터페이스. 중복허용안함

ㄴ SortedMap : 키를 오름차순으로 정렬하는 Map 인터페이스

--------

# Set 인터페이스

- HashSet : 데이터를 해쉬테이블에 담는 클래스. 순서없음.
- TreeSet : red-black이라는 트리에 데이터를 담음. 값에 따라 순서 정해짐. HashSet보다 성능상 느리다. 데이터를 담으면서 동시에 정렬하는 경우 유용
- LinkedHashSet : 해쉬테이블에 데이터를 담음. 저장된 순서에 따라 순서가 결정됨

cf) red-black 트리 : 이진트리구조의 데이터를 담는 형태. 성능이 좋지 않음.

----

# List 인터페이스

- Vector : 객체생성시 크기를 지정할 필요없는 배열클래스
- ArrayList : Vector와 비슷하지만 동기화처리 되어있지 않음
- LinkedList : ArrayList와 동일하지만 Queue를 구현했기 때문에 FIFO 큐 작업을 수행함

----

#Map 인터페이스

- Hashtable : 데이터를 해쉬 테이블에 담는 클래스. 내부에서 해쉬테이블 객체가 동기화되어있어 동기화가 필요하면 이 클래스를 쓰면됨
- HashMap : 데이터를 해쉬 테이블에 담는 클래스. null을 허용하고 동기화되어있지 않음
- TreeMap : red-black 트리에 데이터를 담음. TreeSet과 다른점은 키에의해 순서가 정해진다는 점
- LinkedHashMap : HashMap과 거의 동일. 이중연결리스트 방식을 사용한다는 점만 다르다

cf) 이중연결리스트 : 앞뒤 노드에 대한 링크 정보를 가진 형태. 링크값이 null이거나 비어있으면 가장 첫노드를 의미. 뒤면 가장 마지만 노드.

-----

#Queue 인터ㅓ페이스

- PriorityQueue : 큐에 추가된 순서와 상관없이 먼저 생성한 객체가 먼저나옴
- LinkedBlockingQueue : 선택적으로 저장할 데이터의 크기를 정할수도 있는 fifo기반 링크노드를 사용하는 블로킹큐
- ArrayBlockingQueue : 저장되는 데이터 크기가 정해져 있는 fifo기반 블로킹큐
- PriorityBlockingQueue : 저장되는 데이터 크기가 정해져있지 않고 객체 생성순서에 따라 순서가 저장되는 블로킹큐
- DelayQueue : 큐가 대기하는 시간을 지정해 처리하는 큐
- SynchronousQueue : put() 메소드를 호출하면 다른 스레드에서 take() 메소드가 호출될때까지 대기하느 ㄴ큐. 이 큐에는 저장되는 데이터가 없다.
-----

# 실행속도
Set : TreeSet 가장느림 < LinkedHashSet 가장빠름
List : Vector 가장빠름. List 가 Set 보다 빠르다
Map : TreeMap 가장느림 < HashMap, Hashtable, LinkedHashMap 비슷함

- 시간상 큰 차이들은 없다



