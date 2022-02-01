## Priority Queue 우선순위 큐
: 가장 가중치가 낮은 순서로 데이터를 정렬(Min Heap)시켜놓고 데이터를 출력하는 자료구조.
poll(), peek()을 사용할 수 있다.

- poll() : queue의 가장 작은 데이터를 뽑음 = queue에서는 값이 삭제됨
- peek() : queue의 가장 앞의 데이터를 확인

```
PriorityQueue<Integer> heap = new PriorityQueue<>();
int[] intArr = {12, 1, 2, 3, 8, 10};

for(int i : intArr){
    heap.offer(i); //add() or offer(). add는 큐의 크기가 다 찬경우 예외(IllegalStateException)가 발생할 수 있음
}

heap.peek(); //1
heap.poll(); //1
heap.poll(); //2
heap.peek(); //3
heap.peek(); //3
heep.poll(); //3
```
- add() : 값추가 성공시 true 반환, 큐에 여유공간 없는 경우 IllegalStateException 발생
- offer() : 값 추가 성공시 true 반환, 값 추가 실패시 false 반환
- remove() : 큐가 비어있을 때 삭제하면 예외발생
- clear() : 큐 비우기

