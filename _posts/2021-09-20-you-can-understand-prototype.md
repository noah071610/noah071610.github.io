---
layout: post
title: 📥 Javascript의 프로토타입 찍먹하기 (prototype)
subtitle: Hey stranger? You can understand prototype!
tags: [javascript]
---

<p></p>

![야나두](https://mblogthumb-phinf.pstatic.net/MjAxNzA3MjVfMjgy/MDAxNTAwOTQ3ODU0NDYx.nr2KvglxcxWit7mSM1HNOzYtmvDNdY9g9u6v9abbKZ8g.eWCCgEdVLr4G7CiEOHdSgRqwyPwW_jZ5yOjJHvHVi-0g.JPEG.imok0328/%EC%95%BC%EB%82%98%EB%91%90_%EC%A1%B0%EC%A0%95%EC%A0%81_1.jpg?type=w800)
- 썸네일 출처 : 야나두 (https://www.yanadoo.co.kr/)

# 야 너두 프로토타입 어떤 느낌인지 알 수 있어.

오늘은 Javascript 의 Prototype 에 대해서 배워보겠습니다.

저또한 깊게 들어가면 모르는게 많은 개념이고 많은 개발경력이 있으신 분들도 이해하기 어렵고 난해한게 프로토타입 입니다.

깊게 들어가서 행여나 잘못된 정보를 전달하는것보단 간단하게 프로토타입에 대해서 찍먹하고 이해하는 시간을 가지고 

추후 개인적으로 공부를 하셔서 프로토타입을 완전하게 이해하시는걸 추천드립니다.

## 프로토타입이란?

우리가 보통 javascript 를 프로토타입기반 언어라고 부릅니다. 그만큼 프로토타입이 자바스크립트에게 미치는 영향권은 확실합니다.

그거 아나요? 자바스크립트에는 클래스가 없습니다.

대신 자바스크립트에는 프로토타입이 존재합니다. 이 프로토타입은 클래스의 속성인 상속 , 재사용성, 캡슐화등을 흉내냅니다.

ECMAscript6 에서는 Class 문법이 추가되었지만 Class를 흉내만 내는것이고 여전히 자바스크립트는 프로토타입기반 언어입니다.

그럼 프로토타입이 무엇일까요? 프로토타입은 원형이라는 의미이며 상속기능을 부연하는 장치입니다.

조금 난해하네요. 간단하게 말씀드리면 유전과 같은 느낌입니다.

우리는 부모에게 다양한 유전적 특성을 물려받습니다.

예를 들면 키가 크다던지, 탈모가 있다던지, 조금 마음아픈 일이지만 암도 유전적으로 영향권에 있습니다.

하지만 이런 특성은 나 라는 개인의 특성이지 인간 나아가서 포유류 동물 생명체 에 범위에서는 다릅니다. 그러므로

`인간` 에 있는 보편적 특성

`동물` 에 있는 보편적 특성 등이 프로토타입이 되겠네요.

예를들어 인간은 두발로 걷고 눈이 두개고 머리가 하나 하는것이 인간의 프로토타입이 되겠네요 (장애우에 대해서는 논외로 하겠습니다.)

프로토타입체인에 대해서 지금 짚고 넘어갑시다.

방금전 나는 인간 나아가서 포유류 동물 생명체 의 프로토타입을 가지고 있다고 말씀드렸습니다.

자바스크립트의 프로토타입도 이와같이 상위에 프로토타입을 공유하고 가져올 수 있습니다.

Array는 배열이죠? Array에 상위는 무엇일까요? 바로 Object (참조타입) 입니다.

즉 우리가 `const arr = [1,2,3,4,5]` 를 선언하면 `arr.sort()` 처럼 배열의 프로토타입 내장함수를 사용 할 수 있고 나아가서 `arr.toString()` 같은 Object의 프로토타입도 사용 가능합니다!

## 프로토타입의 특성

위에 arr이 예시로 이어서 설명해드리겠습니다.

우리가 `const arr = [1,2,3,4,5]` 로 선언을 하나 `Array.from({length:5},((v,i)=> i+1))` 처럼 생성자함수로 생성한 배열이든 내부 구조는 생성자 함수로 생성한것과 동일합니다.

즉 Array에 프로토타입을 참조합니다.

Array에 프로토타입은 뭘까요? concat , sort , forEach 처럼 우리가 자주사용하는 배열의 메소드 들입니다.

그럼 이 Array 의 prototype은 우리가 만든 인스턴스에 `[[Prototype]]` 이라는 명칭으로 이식됩니다.

그리고 인스턴스에서는 `변수명.__proto__` 또는 `Array.getPrototypeOf(변수명)` 키워드로 접근 가능합니다.

![proto](https://images.velog.io/images/noah071610/post/d3b62cc9-814c-4a41-af60-f3ceb9e6ce29/image.png)

노아월드 변수를 만들고 `__proto__` 키워드로 검색해보았습니다.

`__proto__` 는 Prototype Link 라고 부르며 객체가 생성될 때 부모였던 함수의 Prototype Object를 가리킵니다.

그러므로 우리가 Array 라는 부모에게 물려받은 내장함수들이 쫘르르 표시됩니다. 우리에게 친숙한 reduce map sort slice 등등이 보이네요.

여기서 `constructor` 라는 프로퍼티에 주목해주세요.

constructor는 Prototype Object와 같이 생성되었던 함수입니다. 즉 Array 생성자가 되겠네요.

constructor 의 prototype이 보이시나요? 많이 생략되어있지만 fill find를 보아하니 Array 생성자도

역시 Array의 프로토타입 (fill find 등은 내장함수)를 탑재하고 있습니다.

부모가 인간인데 자식이 늑대일 확률은 유전학 (프로토타입 관점) 에서는 존재하지 않습니다. 바로 그겁니다.

## 프로토타입 체이닝

![prototype-chaining](https://images.velog.io/images/noah071610/post/edbfbda3-85aa-4e0a-b820-66653d95c0cd/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85.png)

그림을 보시면 나는 인간의 보편적인 프로토타입과 증조부모의 잘생김 프로토타입과 조부모의 키큰 프로토타입 부모님의 똑똑한 프로토타입을 모두 물려받았습니다.

이것이 바로 프로토타입 체이닝 입니다.

프로토타입체이닝은 계층적으로 하위부터 상위까지 거슬러 올라가서 찾아갑니다.

예를들어 증조부모가 잘생김 프로토타입이 10이고 (10점만점에 10점) 있고 부모가 잘생김 프로토타입이 3 이라고 하게되면 프로토타입 체이닝은 하위부터 거슬러 올라가기 때문에

가장 근접해있는 부모의 잘생김 3을 물려받고 사용하게됩니다.

만약 부모의 잘생김 3 프로토타입을 없애게된다면 증조부모의 잘생김 10을 사용하게 되고 이 마저 없으면

인간(유전학으론 오스트랄로피테쿠스로 해야할까요?)의 잘생김 프로토타입을 사용하게 됩니다.

Array 예시도 마찬가지입니다. 우리가 인스턴스의 filter 프로토타입 프로퍼티를 지운다 해도 Array 생성자에 잇는 filter를 사용하게 되며 

Array 생성자의 filter 프로퍼티까지 지워주면 그땐 에러가 나게됩니다.

# 마치며

프로토타입은 충분히 더 복잡하고 특히 상속 관련해서 bridge라는 가상함수를 사용해서 class의 상속기능을 간접적으로 구현한 복잡한 기법이있으나,

이는 ECMA6 부터는 extends 키워드와 그 안에 super 키워드의 조합으로 간편하게 구현 가능하며 사실 제 실력상 설명하기엔 다소 어려운부분이 많아 제외했습니다.

이글은 간단하게 프로토타입을 찍먹한 글입니다. 어떤 개념인지 틀이 잡히셨다면 개인적으로 공부해서 여러분에 것으로 만들길 부탁드립니다.

읽어주셔서 감사합니다.
