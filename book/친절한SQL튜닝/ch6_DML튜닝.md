# DML 성능에 영향을 미치는 요소
- 인덱스
- 무결성 제약
- 조건절
- 서브쿼리
- Redo로깅
- Undo로깅
- LOCK
- 커밋

## 데이터 무결성 규칙
- 개체 무결성
- 참조 무결성
- 도메인 무결성
- 사용자 정의 무결성(OR 업무제약조건)

## Redo 로그
### 사용되는 세가지 목적
1. Database Recovery
2. Cache Recovery
3. Fast Commit

: 서버프로세스가 변경한 버퍼블록들을 디스크에 기록하지 않았더라도 커밋 시점에 redo로그를 디스크에 안전하게 기록했다면 그순간부터 트랜젝션의 영속성은 보장된다

## Undo 로그
### 사용되는 세가지 목적
1. Transaction Rollback
2. Transaction Recovery
3. Read Consistency

### SQL이 실행되는 3단계
- Parse Call :sql 파싱과 최적화 수행단계. sql과 실행계획을 라이브러리에서 찾으면 최적화단계는 생략할 수 있다.
- Execute Call : 말그대로 sql 실행단계. dml은 이 단계에서 끝나지만 select 문은 fetch 단계를 거친다
- Fetch Call : 데이터를 읽어 사용자에게 결과집합을 전송하는 과정. 전송할 데이터가 많은 경우 fetch call 이 여러번 일어날 수 있다.

#### sql 실행단계는 call의 발생위치에 따라 두가지로 나뉘기도 한다.

- User call : 네트워크를 경유해 DBMS 외부에서 인입되는 Call. 3-tier 아키텍쳐에서는 was(또는 ap서버)에서 발생하는 call이다
- Recursive call : DBMS 내부에서 발생하는 Call. 사용자정의 함수/프로시저/트리거 내장 sql등이 해당한다.

