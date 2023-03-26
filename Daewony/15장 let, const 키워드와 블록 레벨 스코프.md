## var 키워드로 선언한 변수의 문제점

변수 중복 선언 허용 → 의도치 않게 변수 값 변경되는 부작용 생김

함수 레벨 스코프 → 함수 외의 코드 블록 내에서 선언해도 모두 전역 변수 → 잡 키워드

변수 호이스팅 → 변수 선언문 이전에 참조 가능해서 → undefined 반환 문제 → 원래 에러가 나와야지

## let 키워드

변수 중복 선언 금지

블록 레벨 스코프

변수 호이스팅 → 참조 에러가 잘 발생한다

funtion* 는 뭘까?
Generator 함수를 선언하는 키워드
Generator는 실행 중인 함수의 상태를 유지하면서, 호출자에게 값을 반환할 수 있는 함수이다

예시)
```jsx
function* foo() {
  yield 1;
  yield 2;
  yield 3;
}

const bar = foo();
console.log(bar.next()); // { value: 1, done: false }
console.log(bar.next()); // { value: 2, done: false }
console.log(bar.next()); // { value: 3, done: false }
console.log(bar.next()); // { value: undefined, done: true }


```


let, const 키워드로 선언한 전역변수는 window 프로퍼티가 아니다

## const 키워드

선언과 초기화 동시에 해야함 → 하지 않으면, 문법 에러 발생

재할당 금지

상수

언더스코어로 구분

const 키워드와 객체 → 값 변경 가능(공유니깐)

어지간하면 const로 다 할 수 있다고 한다

대부분 const 객체를 활용한다고 한다
