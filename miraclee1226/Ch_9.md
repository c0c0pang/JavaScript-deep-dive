# 09장 타입 변환과 단축 평가

## 1. 타입 변환이란?

> - 명시적 타입 변환(타입 캐스팅): 개발자가 의도적으로 값의 타입을 변환하는 것.
> 
> - 암묵적 타입 변환(타입 강제 변환): 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것.
>
> - 타입 변환: 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것.

가장 중요한 것은 코드를 예측할 수 있어야 한다는 것이다.

## 2. 암묵적 타입 변환

### 1. 문자열 타입으로 변환

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환한다.

- `숫자` + `문자열` = `문자열`
- `불리언` + `문자열` = `문자열`('true', 'false')
- `null` + `문자열` = `문자열`
- `undefined` + `문자열` = `문자열`
- `Symbol타입` + `문자열` = TypeError
- `객체 타입` + `문자열` = `문자열`


예제
```javascript
1 + '2' // "12"
```

`+` 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

### 2. 숫자 타입으로 변환

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

- `+문자열(숫자)` = `숫자`
- `+문자열(문자)` = `NaN`
- `+불리언` = true -> `1` , false -> `0`
- `+null` = `0`
- `+undefined` = `NaN`
- `+Symbol()` = TypeError
- `+객체` = `NaN`
    - 예외) 빈 문자열('')과 빈 배열([])은 0으로 치환

예제
```javascript
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
'1' > 0 // true
```

피연산자를 숫자 타입으로 변환할 수 없는 경우 NaN를 반환한다.

3. 불리언 타입으로 변환

**자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsey 값(거짓으로 평가되는 값)으로 구분한다.**

> Falsy값
> - false
> - undefined
> - null
> - 0, -0
> - NaN
> - ''(빈 문자열)

## 3. 명시적 타입 변환

### 1. 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
    - `String('Infinity')` -> "Infinity"  
2. Object.prototype.toString 메서드를 사용하는 방법
    - `(Infinity).toString()` -> "Infinity" 
3. 문자열 연결 연산자를 이용하는 방법
    - `Infinity + ''` -> "Infinity"

### 2. 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
    - `Number('0')` -> 0
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
    - `parseInt('0')` -> 0
3. `+`단항 산술 연산자를 이용하는 방법
    - `+'0'` -> 0
    - `+true` -> 1
4. * 산술 연산자를 이용하는 방법
    - `'0'*1` -> 0
    - `true*1` -> 1

### 3. 불리언 타입으로 변환

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
    - Boolean('x') -> true

2. ! 부정 논리 연산자를 두 번 사용하는 방법
    - !!'x' -> true

## 4. 단축 평가

### 1. 논리 연산자를 사용한 단축 평가

논리합(||) 또는 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 어느 한 쪽으로 평가된다.

이 때 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 이를 **단축평가**라고 한다.
- **단축평가**: 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것

|단축 평가 표현식 | 평가 결과 |
|-----|----|
|`true \|\| anything` | true |
|`false \|\| anything` | anything |
|`true && anything` | anything |
|`false && anything` | false |

#### 단축평가가 유용한 상황

1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```javascript
let item = null
let value = item && item.value -> null
```

2. 함수 매개변수에 기본값을 설정할 때

```javascript
function getStringLength(str) {
    str = str || ''; 
    return str.length;
}

getStringLength(); -> 0 // 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.
getStringLength('hi') -> 2
```

### 2. 옵셔널 체이닝 연산자

좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 유용하다.

```javascript
let elem = null;
let value = elem?.value;
console.log(value); // undefined
```

옵셔널 체이닝 연산자가 도입되기 이전에는 논리 연산자 &&를 사용해 변수가 null 또는 undefined인지 확인했다.

```javascript
let elem = null;
let value = elem && elem.value;
console.log(value); // null
```

논리 연산자 &&는 좌항 피연산자가 Falsy 값이면 좌항 피연산자를 그대로 반환한다.

하지만 옵셔널 체이닝 연산자는 좌항 피연산자가 Falsy 값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```javascript
let str =''
let len1 = str && str.length
let len2 = str?.length

console.log(len1) // ''
console.log(len2) // 0
```

### 3. null 병합 연산자

좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

null 병합 연산자 도입 이전에는 논리 연산자 ||를 사용한 단축 평가를 통해 변수에 기본값을 설정했다.

하지만 논리 연산자의 경우 모든 피연산자 Falsy 값을 걸러내기 때문에 의도적으로 Falsy 값을 넣는 경우 (0이나 '') 예기치 않은 동작이 발생할 수 있다.










