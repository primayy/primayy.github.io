---
title: "[JS] 자바스크립트 변수 호이스팅"
date: 2021-02-02
last_modified_at: 2021-02-06
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

자바스크립트에서 **변수 선언은 런타임이 아니라 그 이전 단계(소스 코드의 평가 과정)에서 실행된다고 합니다.**

조금 더 명확하게 말하자면, **소스코드의 평가 과정에서 모든 선언문(변수 선언문, 함수 선언문 등)은 먼저 실행된다고 하죠**

이렇듯이 런타임에서 변수의 선언이 코드에서 존재하는 위치보다 코드의 선두로 끌어 올려진 것처럼 동작하는 것을 **변수 호이스팅**이라고 합니다.



### var로 선언한 변수의 호이스팅

var 키워드로 선언한 변수의 호이스팅 과정을 보기 위해

다음과 같이 코드를 작성해서 실행해보겠습니다.

```javascript
console.log(a);
var a;
```

변수 a를 선언한 위치보다 a를 출력하는 코드가 먼저 실행되므로 reference error가 발생할 것이라고 예상되지만...

![image](https://user-images.githubusercontent.com/37567802/106612026-fb02a680-65ab-11eb-9424-87a789c27609.png)

결과는 undefined를 출력합니다!

```javascript
var a;//이런 느낌으로 호이스팅 되는구나!
console.log(a);
```

변수 a의 선언이 코드의 선두로 끌어 올려진 것처럼 동작한거죠!



### let과 const로 선언한 변수의 호이스팅

그런데 let과 const의 경우에는 var 키워드를 사용한 경우와 다른 결과를 보여줍니다.

```javascript
//let의 경우
console.log(b);
let b;
```

```javascript
//const의 경우
console.log(c);
const c=1;
```

단지 키워드를 var에서 let, const로 바꾼 것이기 때문에

```javascript
//let 호이스팅은 이렇게 되겠지??
let b;
console.log(b);
```

```javascript
//const는 아마도 이렇게 되겠지..?
const c;
console.log(c);
const c=1;
```

처럼 작동해서 이전 결과와 마찬가지로 오류가 발생하지 않을 거라고 생각했지만...

![image](https://user-images.githubusercontent.com/37567802/106612858-e7a40b00-65ac-11eb-8230-42db7d6883a4.png)

![image](https://user-images.githubusercontent.com/37567802/106613496-99dbd280-65ad-11eb-9801-e166a66a6880.png)

결과는 매정하게도... Reference Error를 출력합니다.



#### let과 const 키워드는 호이스팅이 안되는 걸까?

<center>
    <p>
        변수는 호이스팅 된다고 했는데...
    </p>
     <P>
        let과 const로 선언한 변수들은 호이스팅이 안되는 걸까요..? 
    </P> 
    <i><b>제게 거짓말을 했던걸까요...?</b></i>
</center>



<div> 
    <b>답이 정말 궁금한 분들은 긁어요!!! -></b><span style="color:white;"><b>당연하지!!! let, const도 호이스팅 된다!!!!!!!!!!!!</b></span><b><- 긁어요!!!</b>
                                                                        </div>



#### 변수 생성의 3단계

<center><img src="https://user-images.githubusercontent.com/37567802/107055535-bcbbf000-6814-11eb-880a-f73a34234a15.png" alt="image" style="zoom:80%;" /></center>

자바스크립트 변수는 생성하면 위 사진처럼 3단계에 걸쳐서 생성된다고 해요.

그리고 각 단계별로 수행하는 역할은 다음과 같아요.

1. **선언**: 변수를 스코프에 등록한다.
2. **초기화**: 메모리를 할당하고 스코프 안에 있는 변수에 바인딩한다.
3. **할당**: 초기화 된 변수에 값을 할당한다.



곰곰이 생각해 보니 책의 앞 부분에서 **var의 경우, 선언과 초기화가 동시에** 이루어진다고 했던 것 같아요. (책 4장: 변수, 41p)

예를 들어  var a = 3;이라는 변수를 선언하고 할당하는 코드가 존재할 때,

```
1. 선언: 변수 이름 a를 등록
2. 초기화: a 변수에 암묵적으로 undefined를 할당하여 초기화
3. 할당: a 변수에 3을 할당
```



3 단계로 이루어진 과정에서 1,2 단계인 선언과 초기화가 동시에 이루어지는 거죠!

```
//바로 이렇게요!!
1. 선언 - 초기화: 변수 이름 a를 등록하고 a 변수에 암묵적으로 undefined를 할당하여 초기화
2. 할당: a 변수에 3을 할당
```



이 과정을 앞서 설명했던 var로 선언한 변수 호이스팅에 적용한다면

```javascript
console.log(a);
var a = 3;
```

이랬던 코드는 런타임 이전 단계에서 이렇게 작동하는 거겠죠?

```javascript
//호이스팅은 아마 이럴 거야!
var a; // (1)~(2) 변수 이름을 등록하고 undefined로 초기화
console.log(a);// undefined 출력!!!!!!!!!!!! 
a = 3;// (3) 값 할당
```

<center>
    <i>"아 ~ 그래서 런타임에서 값이 할당되는 순간보다 먼저 변수 값을 출력해도 Reference Error가 아니라 초기화 단계에서 할당된 값인 undefined가 나오는 거구나~"</i>
</center>



**그럼 실행 결과가 다르게 나오는 let, const 키워드는 var와는 다르게 동작하는 걸까요?**

<center>
    <i>"네! 다르게 동작합니다. 선언과 초기화가 동시에 이루어지지 않아요!"</i>
</center>

예를 들어서 let b = 3; 이라는 변수를 선언하고 값을 할당하는 코드를 작성한다면

```
1. 선언: 변수 이름 b를 등록
2. TDZ: ?????!!!!!!!
3. 초기화: b 변수에 undefined를 할당하여 초기화
4. 할당: b 변수에 3을 할당
```

이라는 과정을 거치는 거죠!



#### TDZ란?

TDZ의 정확한 명칭은 Temporal Dead Zone입니다.

<center><img src="https://user-images.githubusercontent.com/37567802/107056288-8894ff00-6815-11eb-830b-710f9d2a0c36.png" alt="image" style="zoom:80%;" /></center>



# 02-06~ 계속 작성중!!!!!!!!!





## 마무리

자바 스크립트를 정~말 모르는 상태에서 정리한 글이라 많이 부족하고 틀린 내용이 있을 수도 있습니다.

수정할 내용, 보완할 내용 알려주시면 참고해서 반영하도록 하겠습니다!

**우리 루키 8기 모두 행코합시다~~~**



## Reference

- [Hoisting in Modern JavaScript - let, const and var](https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda?gi=1367f9d80c41)
- [JavaScript Variables Lifecycle: Why let Is Not Hoisted](https://dmitripavlutin.com/variables-lifecycle-and-why-let-is-not-hoisted/)
- 모던 자바스크립트 Deep Dive - 자바스크립트의 기본 개념과 동작 원리, 이웅모 지음

