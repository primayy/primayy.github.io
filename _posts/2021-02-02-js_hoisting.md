---
title: "[JS] 자바스크립트 변수 호이스팅"
date: 2021-02-02
categories: 
  - JS
toc: true
author_profile: true
toc_label: "목차" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
author_profile: true
---



## 자바스크립트 변수 호이스팅 - var, let, const

<모던 자바스크립트 Deep Dive - 자바스크립트의 기본 개념과 동작 원리, 이웅모 지음> (이하 '책'이라고 다루겠습니다.)을 읽으며 궁금한 내용을 조사해서 정리한 글입니다. 



### 변수 호이스팅 정의

자바스크립트에서 **변수 선언은 런타임이 아니라 그 이전 단계(소스 코드의 평가 과정)에서 실행된다고 한다.**

조금 더 명확하게 말하자면, **소스코드의 평가 과정에서 모든 선언문(변수 선언문, 함수 선언문 등)은 먼저 실행된다고 한다.**

이 과정에서 변수 선언이 코드의 선두로 끌어 올려진 것처럼 동작하는 것을 **변수 호이스팅**이라고 한다.



### var로 선언한 변수의 호이스팅

var 키워드로 선언한 변수의 호이스팅 과정을 보기위해

다음과 같이 코드를 작성해서 실행해보겠다.

```javascript
console.log(a);
var a;
```

![image](https://user-images.githubusercontent.com/37567802/106612026-fb02a680-65ab-11eb-9424-87a789c27609.png)

```javascript
var a;//이런 느낌으로 호이스팅 되는구나!
console.log(a);
```

변수 a의 선언이 코드의 선두로 끌어 올려진 것처럼 동작하여

Reference Error를 출력하는 것이 아니라 undefined를 출력한다.



### let과 const로 선언한 변수의 호이스팅

그런데 let과 const의 경우에는 var를 사용한 경우와 다른 결과를 보여준다.

```javascript
console.log(b);
let b;
```

```javascript
console.log(c);
const c=1;
```

단지 키워드를 var에서 let, const로 바꾼 것이기 때문에

```javascript
let b;
console.log(b);
```

```javascript
const c;
console.log(c);
const c=1;
```

처럼 작동해서 오류가 발생하지 않을거라고 생각했지만...

![image](https://user-images.githubusercontent.com/37567802/106612858-e7a40b00-65ac-11eb-8230-42db7d6883a4.png)

![image](https://user-images.githubusercontent.com/37567802/106613496-99dbd280-65ad-11eb-9801-e166a66a6880.png)

결과는 매정하게도... Reference Error를 출력한다.

let과 const로 선언한 변수들은 호이스팅이 안되는 걸까..? 

***모두 내게 거짓말을 했던걸까?***



그런데 잠깐..!

곰곰히 생각해보니 책의 앞 부분에서 **var의 경우, 선언과 초기화가 동시에** 이루어진다고 했다. (책 4장: 변수, 41p)

예를 들어  var a를 선언하면

1. 선언 단계: 변수 이름 a를 등록
2. 초기화 단계: a 변수에 암묵적으로 undefined를 할당하여 초기화

과정이 동시에 이루어지는 것이다.

그래서 런타임에 값 할당 전에 변수 값을 출력해도 Reference Error가 발생하지 않은 것이다.



2021/02/02 ~ 작성중



## Reference

- [Hoisting in Modern JavaScript - let, const and var](https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda?gi=1367f9d80c41)
- 모던 자바스크립트 Deep Dive - 자바스크립트의 기본 개념과 동작 원리, 이웅모 지음

