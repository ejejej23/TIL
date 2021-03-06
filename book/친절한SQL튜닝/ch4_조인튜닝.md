#서브쿼리 조인

### 서브쿼리 조인이 필요한 이유
: 최근 옵티마이저는 비용cost를 평가하고 실행계힉을 생성하기 앞서 사용자로부터 전달받은 sql을 최적화에 유리한 형태로 변환하는 작업, 즉 쿼리변환부터 수행한다.

쿼리변환Query Transformation : 옵티마이저가 sql을 분석해 의미적으로 동일하면서도 더 나은 성능의 형태로 재작성하는것

서브쿼리 : 하나의 sql문 안에 괄호로 묶은 별도의 쿼리블록

-------

### 인라인뷰Inline view 
: from 절에 사용한 서브쿼리
### 중첩된 서브쿼리Nested Subquery 
: where 절에 사용한 서브쿼리. 특히 서브쿼리가 메인쿼리 컬럼을 참조하는 형태를 '상관관계있는 서브쿼리Correlated' 라고함
### 스칼라 서브쿼리Scalar Subquery
: 한 레코드당 정확히 하나의 값을 반환하는 서브쿼리

- 옵티마이저는 쿼리블록 단위로 최적화를 수행

-----

## 필터 오퍼레이션
: no_unnest 힌트 사용. 기본적으로 nl 조인과 처리 루틴이 같다. 
차이점은 아래와 같다
- 필터는 메인쿼리의 한 로우가 서브쿼리의 한 로우와 조인에 성공하는 순간 진행을 멈추고, 메인쿼리의 다음 로우를 계속 처리한다
- 필터는 캐싱기능이 있다
- 필터 서브쿼리는 일반 nl조인과 달리 메인쿼리에 종속되므로 조인순서가 고정된다. 메인쿼리가 항상 드라이빙 집합이다.

## 서브쿼리 Unnesting
: 옵티마이저가 대개 Unnesting 을 선택한다. 서브쿼리를 풀어낸다는 뜻.
- 서브쿼리를 그대로 두면 필터방식을 사용할수밖에 없지만, Unnesting하면 일반 조인문처럼 다양한 최적화 기법을 사용할 수 있다.

cf)nl 세미조인
: 기본적으로 nl 조인과 같은 프로세스. 다른점은 조인에 성공하는 순간 진행을 멈추고 메인쿼리의 다음 로우를 처리한다는 점.
오라클 10g부터는 nl세미조인이 캐싱기능도 갖게 되었으므로 사실상 필터 오퍼레이션과 큰 차이는 없다.

-----

### EXISTS는 매칭되는 데이터 존재여부를 확인하는 연산자. 조건절을 만족하는 레코드를 만나는 순간 멈추는 기능을 가지고있다.

### 서브쿼리에 rownum을 쓰면 옵티마이징 기능을 사용할 수 없도록 막힌다. 
: nl 세미조인이 동작하려면 서브쿼리가 Unnesting 되어야 하는데 rownum조건이 그것을 막기 때문

-----

##서브쿼리 Pushing
: Unnesting되지 않은 서브쿼리는 항상 필터방식으로 처리됨. + 대개 실행계획상에서 맨 마지막 단계에서 처리된다.
서브쿼리 필터링을 가능한한 앞단계에서 처리하도록 강제하는 기능. 
- 이 기능은 Unnesting되지 않은 서브쿼리에서만 동작한다
- Unnesting되면 다양한 조인방식으로 실행된다. 이때 푸싱힌트는 무용지물이 된다.
- push_subq 힌트는 항상 no_unnest 힌트와 같이 사용하는것이 올바른 사용법
- 힌트는 push_subq/no_push_subq


## 뷰와 조인
: 최적화 단위는 쿼리 블록이므로 옵티마이저가 뷰 쿼리를 변환하지 않으면 뷰 쿼리 블록을 독립적으로 최적화한다.
- 단점 : 부분범위 처리가 불가능

##조인조건Pushdown
: 11g 이후로 조인조건 pushdown이라는 쿼리변환기능이 작동. 메인쿼리를 실행하면서 조인 조건절 값을 건건이 뷰 안으로 밀어넣는 기능이다.
- 힌트 : push_pred
- 옵티마이저가 뷰를 머징하면 힌트가 작동하지 않으니 no_merge힌트를 함께 사용하는것이 올바른 방법

-----

# 스칼라 서브쿼리
: 사용자함수를 select절에 사용하는 경우 메인쿼리 건수만큼 재귀적으로 반복실행한다. 하지만 스칼라 쿼리를 사용하면 캐싱기능이 있다!

## 스칼라쿼리 캐싱효과
: 스칼라 서브쿼리 캐싱은 필터 서브쿼리 캐싱과 같은 기능. 캐싱은 쿼리단위로 이루어진다.
- 제약 : 두개이상의 값을 반환할 수 없다.

부작용
: 캐싱공간은 항상 부족함. 스칼라 쿼리의 캐싱효과는 입력값의 종류가 소수여서 해시 충돌 가능성이 작을 때 효과가 있다. 아닌 경우 cpu사용률만 높아질 수 있다.
메인쿼리 집합이 매우 작은 경우에도 도움이 안됨.

## 스칼라 서브쿼리 Unnesting
: 스칼라 쿼리도 nl방식 조인을 하므로 캐싱효과가 크지 않으면 랜덤 io 부담이 있다. 대량 데이터를 처리하는 병렬쿼리는 해시조인으로 처리해야 효과적이기 때문
