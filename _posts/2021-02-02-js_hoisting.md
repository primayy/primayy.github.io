---

title: "[JS] 자바스크립트 변수 호이스팅"
date: 2021-02-02
last_modified_at: 2021-02-07
categories: 
  - JS
toc: true
author_profile: true
toc_label: "목차" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
author_profile: true
---

# 자바스크립트 변수 호이스팅 - var, let, const

<모던 자바스크립트 Deep Dive - 자바스크립트의 기본 개념과 동작 원리, 이웅모 지음> (이하 '책')을 읽다가 **변수 호이스팅**에 대해서 이해가 안 되는 부분들이 있어서 정리해 봤습니다.



**목차**는 다음과 같습니다.

[1. 변수 선언의 3단계](#변수-선언의-3단계)

[2. 변수 호이스팅이란?](#변수-호이스팅이란?)

[3. var로 선언한 변수의 호이스팅](#var로-선언한-변수의-호이스팅)

[4. let과 const로 선언한 변수의 호이스팅](#let과-const로-선언한-변수의-호이스팅)

[5. let과 const 키워드는 호이스팅이 안되는 걸까?](#let과-const-키워드는-호이스팅이-안되는 걸까?)

[6. 정리](#정리)

궁금한 내용을 먼저 보셔도 좋지만, 글을 쓰다보니 생각의 흐름대로 작성되었습니다...

처음부터 읽는 것을 추천드립니다ㅎㅎ (머쓱 코 쓰윽)



## 변수 선언의 3단계

변수 호이스팅을 다루기 전에 자바스크립트 변수 선언의 3단계를 알고 시작하면 좋을 것 같아서 준비했습니다.



<center><img src="https://user-images.githubusercontent.com/37567802/107055535-bcbbf000-6814-11eb-880a-f73a34234a15.png" alt="image" style="zoom:80%;" /></center>

```
1. 선언: 변수를 스코프에 등록한다.
2. 초기화: 스코프 안에 있는 변수에 메모리를 할당하고 undefined로 초기화한다.
3. 할당: 초기화 된 변수에 값을 할당한다.
```



## 변수 호이스팅이란?

자바스크립트에서 **변수 선언은 런타임이 아니라 그 이전 단계(소스 코드의 평가 과정)에서 실행된다고 합니다.**

조금 더 명확하게 말하자면, **소스코드의 평가 과정에서 모든 선언문(변수 선언문, 함수 선언문 등)은 먼저 실행된다고 하죠**.

이때, 변수가 선언된 위치가 아니라 코드의 선두로 끌어 올려진 것처럼 동작하는 것을 **변수 호이스팅**이라고 합니다.



책에서는 호이스팅을 위 글처럼 설명하면서 var 키워드에 대한 호이스팅 결과만 보여줬습니다. (적어도 제가 읽은 부분까지는요...)

그러다 보니까 'let, const 키워드는 다르게 동작 하나?' 라는 궁금증이 생기더라고요.



<center><i>그래서 직접 해봤습니다!</i></center>



다음 항목부터는 var, let, const 키워드를 이용한 간단한 코드 실행 결과를 보면서 변수 호이스팅 과정을 보려고 합니다.



## var로 선언한 변수의 호이스팅

var 키워드로 선언한 변수의 호이스팅 과정을 보기 위해

다음과 같이 코드를 작성해서 실행해보겠습니다.

```javascript
console.log(a);
var a;
```

변수 a를 선언한 위치보다 a를 출력하는 코드가 먼저 실행되므로 reference error가 발생할 거라고 예상되지만...



![image](https://user-images.githubusercontent.com/37567802/106612026-fb02a680-65ab-11eb-9424-87a789c27609.png)

결과는 undefined를 출력합니다!



변수 a의 선언이 코드의 선두로 끌어 올려진 것처럼 동작한 거죠!

```javascript
var a;//이런 느낌으로 호이스팅 되는구나!
console.log(a);
```

역시 이건 책에서 보여준 결과와 같습니다!



## let과 const로 선언한 변수의 호이스팅

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



단지 키워드를 var에서 let, const로 바꿨기 때문에

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



이전 결과와 마찬가지로 오류가 발생하지 않을 거라고 생각했지만...

![image](https://user-images.githubusercontent.com/37567802/106612858-e7a40b00-65ac-11eb-8230-42db7d6883a4.png)

![image](https://user-images.githubusercontent.com/37567802/106613496-99dbd280-65ad-11eb-9801-e166a66a6880.png)

결과는 매정하게도... Reference Error를 출력합니다.



## let과 const 키워드는 호이스팅이 안되는 걸까?



<center>
    <p>
        변수는 호이스팅 된다고 했는데...
    </p>
     <P>
        let과 const로 선언한 변수들은 왜 호이스팅이 안되는 것처럼 보일까요..? 
    </P> 
    <i><b>제게 거짓말을 했던걸까요...?</b></i>
    <p>
        <b>긁어요!!! -></b>
        <span style="color:white;"><b>let, const도 호이스팅 된다!!!!!!!!!!!!</b></span>
        <b><- 긁어요!!!</b></p>
</center>




변수 선언의 3단계부터 천천히 생각해 보겠습니다.

var a = 3; 이라는 변수를 선언하고 할당하는 코드는 변수 선언의 3단계에 따르면 다음과 같이 작동합니다.

```
1. 선언: 스코프에 변수 이름 a를 등록
2. 초기화: a 변수에 메모리를 할당하고 undefined로 초기화
3. 할당: a 변수에 3을 할당
```



그런데 곰곰이 생각해 보니 책의 앞 부분에서 **var의 경우, 선언과 초기화가 동시에** 이루어진다고 했던 것 같습니다. (책 4장: 변수, 41p)

선언과 초기화가 동시에 이루어지면 다음과 같겠네요.

```
//바로 이렇게요!!
1. 선언 - 초기화: 변수 이름 a를 등록하고 a 변수에 메모리를 할당하고 undefined로 초기화
2. 할당: a 변수에 3을 할당
```



이 과정을 앞서 설명했던 var 키워드로 선언한 변수 호이스팅 코드에 적용한다면,

```javascript
console.log(a);
var a = 3;
```



위 코드는 런타임 이전 단계에서 다음과 같이 작동하겠습니다.

```javascript
//호이스팅 되면 아마 이럴 거야!
var a; // (1)~(2) 변수 이름을 등록하고 undefined로 초기화
console.log(a);// 오 값이 있네? undefined 출력!!!!!!!!!!!! 
a = 3;// (3) 값 할당
```

<center>
    <i>"아 ~ 그래서 런타임에서 값이 할당되는 순간보다 먼저 변수 값을 출력해도 Reference Error가 아니라 초기화 단계에서 할당된 값인 undefined가 나오는 거구나~"</i>
</center>


**그런데 let, const 키워드는 왜 결과가 다를까요? var 키워드랑 다르게 동작하는 걸까요?**

<center>
    <i>"네! 선언과 초기화가 동시에 이루어지지 않습니다!"</i>
</center>



예를 들어서 let b = 3; 이라는 변수를 선언하고 값을 할당하는 코드를 작성한다면

변수 선언의 3단계는 똑같이 거치지만, 각각의 단계가 **독립적으로 실행**됩니다!

```
1. 선언: 스코프에 변수 이름 b를 등록 -> 너 따로!
2. 초기화: b 변수에 메모리를 할당하고 undefined로 초기화 -> 그 다음 너 따로!
3. 할당: b 변수에 3을 할당 -> 마지막으로 너 따로!
```



예를 들자면

```javascript
//호이스팅 전에는 이렇게 작성된 코드가
console.log(b);
let b=3;
```

```javascript
//호이스팅 된다면 이렇게 되는거죠!
b; //(1)선언 단계를 통해 스코프에 이름만 등록하고

console.log(b);

let b;//(2)여기서 b 변수에 메모리가 할당되고 undefined로 초기화 될 거에요.
b=3;//(3) 그리고 3이라는 값이 할당 되겠죠.
```



그런데 호이스팅 결과를 자세히 보면 **메모리가 할당되는 초기화 단계 전에 변수에 접근**하고 있습니다.

```javascript
b; //(1)선언

//↓↓↓↓↓↓↓↓↓↓↓↓↓↓
console.log(b); //바로 여기서요!
//↑↑↑↑↑↑↑↑↑↑↑↑↑↑ 

let b;//(2)초기화
b=3;//(3)할당
```



엔진 입장에서는 변수가 참조하는 메모리가 없는 순간입니다.

<img src="https://user-images.githubusercontent.com/37567802/107121455-e852df00-68d5-11eb-8c75-e5e2ac109985.png" alt="image" style="zoom:80%;" />

![image](https://user-images.githubusercontent.com/37567802/106612858-e7a40b00-65ac-11eb-8230-42db7d6883a4.png)

그래서 Reference Error를 출력하는 거죠.



이렇게 변수가 선언되었을 때, 메모리가 할당되고 초기화되기 전에 변수에 접근하는 구간을 **TDZ(Temporal Dead Zone: 일시적 사각 지대)** 라고 부릅니다.

변수에 접근해도 참조할 수 없는 구간이기 때문에 ''일시적 사각 지대'' 라고 하는 거죠.



<center><i>아~ let, const는 TDZ 구간에서 변수를 참조하려니까 오류를 출력하는 거구나!</i></center>



그렇다면 **변수가 호이스팅 된다는 건 변수 선언의 3단계 중에서 '선언' 단계가 스코프 최상단으로 옮겨지듯 동작하는 것**을 말하겠네요.



마지막으로 그림을 참고하면 더욱 이해가 쉬울 것 같아서 준비해봤습니다.

<center>
    <table>
        <th style="text-align:center;">var 키워드 lifecycle</th>
        <th style="text-align:center;">let 키워드 lifecycle</th>
        <tr>
        <td><img src="https://user-images.githubusercontent.com/37567802/107122616-163b2200-68dc-11eb-9838-a004fa65fc7f.png" alt="image" style="zoom:80%;" /></td>
        <td><img src="https://user-images.githubusercontent.com/37567802/107056288-8894ff00-6815-11eb-830b-710f9d2a0c36.png" alt="image" style="zoom:80%;" /></td>
        </tr>
    </table>
</center>



## 정리

오늘 다룬 내용을 정리하면 다음과 같습니다.

- 변수는 선언, 초기화, 할당이라는 3개의 단계로 나뉜다.
- var는 선언과 초기화를 동시에 수행하며 let, const는 모두 따로 수행한다.
- 이 과정에서 let, const는 변수를 참조할 수 없는 TDZ라는 구간을 갖게 된다.
- 변수 호이스팅: **이 3개의 단계 중 ''선언''을 스코프 최상단으로 끌어 올린다.**





# 마무리

많이 부족하고 틀린 내용이 있을 수도 있습니다. (분명히)

수정할 내용, 보완할 내용 알려주시면 반영하도록 하겠습니다.

감사합니다!



<center><b>루키 8기 모두 열공해서 행코합시다~~~</b></center>



# Reference

- [Hoisting in Modern JavaScript - let, const and var](https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda?gi=1367f9d80c41)
- [JavaScript Variables Lifecycle: Why let Is Not Hoisted](https://dmitripavlutin.com/variables-lifecycle-and-why-let-is-not-hoisted/)
- 모던 자바스크립트 Deep Dive - 자바스크립트의 기본 개념과 동작 원리, 이웅모 지음

