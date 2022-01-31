## Hoisting
: hoist(=끌어올리다). js에서 끌어올려지는 것은 변수. 호이스트란 변수의 정의가 그 범위에 따라 *선언*과 *할당*으로 분리되는 것을 의미. 
-> 변수가 함수 내에서 정의되었을 경우 선언이 함수의 최상위로, 함수밖에서 정의되었을 경우 전역 컨텍스트의 최상위로 변경된다.

### Declaration 선언 부가 호이스팅된다: 선언문이 자바스크립트 엔진 구동시 가장 최우선으로 해석되기 때문
### Assignment 할당부는 런타임 과정에서 일어나기 때문에 호이스팅되지 않음

```
function getX() {
  console.log(x); // undefined  -> 이 타이밍에서 에러나지 않음. undefined 라고 출력하고 넘어감
  var x = 100;
  console.log(x); // 100
}
getX();
```

```
test( );
function test( ){
  console.log(‘hello’);
};
// console> hello   -> 선언부. test함수에 대한 선언이 호이스팅되어 global 객체에 등록되기 때문에 출력된다
```

```
test( );
var test = function( ) {
  console.log(‘hello’);
};
// console> Uncaught TypeError: foo is not a function   -> 할당부. test변수에 함수를 할당하는 구조기 때문에 호이스팅되지 않음. 런타임시 에러가 발생.
```

------
## Closure 클로저
: 두개의 함수로 만들어진 환경으로 이루어진 특별한 객체. 함수와 그 함수가 선언됐을 때의 렉시컬 환경(Lexical environment)과의 조합.

### 클로저 생성 조건
1. 내부함수가 익명함수가 되어 외부함수의 반환값으로 사용된다
2. 내부함수는 외부함수의 실행환경에서 실행된다.
3. 내부함수에서 사용되는 변수x는 외부함수의 변수 스코프에 있다.
```
var name = `Warning`;
function outer() {
  var name = `closure`;
  return function inner() {
    console.log(name);
  };
}

var callFunc = outer();
callFunc();
// console> closure
```
위 코드예제에서 callFunc를 클로저라고 함. outer함수의 context를 참조한다.
외부함수 호출이 종료되더라도 외부함수의 지역변수 및 변수 스코프객체의 체인관계를 유지할수 있는 구조를 클로저라고 한다. 외부함수에 의해 반환되는 내부함수.

-----
## Promise 프로미스
: 콜백함수의 중첩을 피하기 위해 만들어진 패턴. 비동기 작업들을 순차적으로 진행/병렬로 진행하는 등 컨트롤이 가능해진 구조. 예외처리에 대한 구조가 존재하기 때문에 좀더 가시적으로 관리할 수 있게 됨

-> 프로미스 패턴은 ECMAScript6 스펙에 정식 포함되었다.
