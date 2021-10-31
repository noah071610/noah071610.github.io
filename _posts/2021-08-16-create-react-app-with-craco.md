---
layout: post
title: 🤞 CRA에서 babel이나 webpack 설정하는 방법
subtitle: How to set babel and webpack by using Create react app.
cover-img: /assets/img/path.jpg
share-img: /assets/img/path.jpg
tags: [javascript, react]
---

# CRA는 Create react App 의 약자이다.

즉 React App 의 초기설정을 쉽게 해주자는 취지로 만든건데

단점도 있으니 커스터마이징이 굉장히 어렵다는것이다

본인은 이모션이라는 css 라이브러리를 사용하고있었는데, 여기서 애를 먹었다.

이모션은 css props를 추가한 자체 jsx를 사용하고있어서 문서 상단에 항상


```javascript
/** @jsxImportSource @emotion/react */ <- 이놈
 import { css } from "@emotion/react";
```

이런식으로 구문을 추가해야지 비로소 Emotion이 작동했다.

하지만 매번 새로운 컴포넌트에 이런식으로 구문을 추가하는건 뭔가 꺼림직하다.

그래서 중복제거를 목표로 바벨 프리셋을 설정하려고 했지만 CRA는 바벨설정이 불가능하게 셋팅되어있다.

eject를 사용하게 되면 설정이 가능한데 그러면 CRA의 본질 자체를 잃어버린다.

마치 컴퓨터를 소비자를 위해 깔끔하게 조립해서 배송해줬더니 그걸 다 뿌수고 새로 조립하는 아이러니한 상황이 연출되는거다. 
(그럴꺼면 다나와에서 알아서 사서 조립하지 왜 조립해달라고 했어?)

## eject 없이 carco를 사용해서 babel 및 webpack을 설정해주자

그렇기 때문에 eject는 지양하고 craco를 사용해서 babel preset을 오버라이딩 해줬다

```javascript
package.json
npm i @craco/craco
npm i -D @emotion/babel-preset-css-prop
craco.config.js
const emotionPresetOptions = {};
const emotionBabelPreset = require("@emotion/babel-preset-css-prop").default(
    undefined,
    emotionPresetOptions
);
```

```javascript
module.exports = {

  babel: {

  plugins: [...emotionBabelPreset.plugins],

  },

};
```

루트폴더에 craco.config.js 를 추가하고 안에 preset을 추가해 준다.

그리고

```
package.json

"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
  "eject": "craco eject"
},
```

스크립트 명령어를 craco 로 바꿔 주도록한다.

이렇게 그러면 eject 없이 CRA에 babel 설정을 override 가능하게 된다.

개인적으로 CRA는 양날의검이다. 프로젝트의 성향에 따라 사용할지 안할지를 결정해서 쓸데없는 시간낭비를 미연에 방지하도록 하자.
