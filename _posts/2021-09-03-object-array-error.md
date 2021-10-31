---
layout: post
title: 🤞 CRA에서 babel이나 webpack 설정하는 방법
subtitle: Create react App를 craco 라이브러리로 조정해보자.
cover-img: /assets/img/path.jpg
share-img: /assets/img/path.jpg
tags: [javascript]
---

{: .box-error} Error: cannot assign to read only property '0' of object ' object array ' sort.

```javascript
export default function shuffle(obj: CardObj[]) {
  let array = obj.NearRecommendCards;
  let arrLength = obj.NearRecommendCards.length;
  if (obj.ShoppingRecommendCards) {
    array = obj.ShoppingRecommendCards.concat(array).slice(0, arrLength);
  }
  if (obj.FoodRecommendCards) {
    array = obj.FoodRecommendCards.concat(array).slice(0, arrLength);
  }
  return array.sort(() => Math.random() - 0.5);
}
```

Javascript 배열 오브젝트 셔플 함수를 만드는중 오류발생.

전역변수 또는 객체 프로퍼티가 읽기 전용으로 할당된 경우 발생한다고 한다.

파훼법을 찾던중 slice로 얕은 복사를 해줘서 프로퍼티를 건드리지않고 복사값을 넘겨줬다.

```javascript
return array.slice(0, arrLength).sort(() => Math.random() - 0.5);
```

그래프ql때 이런경우를 한번 겪어본적 있는데 
항상 객체는 참조타입임을 명심하고 개발해야겠다고 생각했다.
