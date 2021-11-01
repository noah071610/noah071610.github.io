---
layout: post
title: 💁‍♂️ 3. 브라우저 렌더링 방식, 브라우저는 어떻게 화면을 보여줄까?  | 웹모르는 웹개발자
subtitle: How browser render our contents
tags: [cs, 웹모르는웹개발자]
---

<p></p>

# 브라우저는 어떻게 화면을 보여줄까?

우리는 저번글에서 인터넷 네트워크의 전송방식에 대해서 배웠습니다.

이젠 우리가 받은 데이터와 리소스를 브라우저가 렌딩하는 방식에 대해서 배워보겠습니다.

## 브라우저의 기본 구조

저번글과 같이 데이터는 TCP/IP 4계층을 통해 인코딩 또는 디코딩되며 데이터를 전송합니다.

서버는 클라이언트에 HTTP요청을 읽고 그에 맞는 값을 반환 (Response) 해줍니다.

다시 머나먼 여정을 끝내고 드디어 데이터와 리소스들이 클라이언트 서버에 도착했습니다.

여기서 브라우저의 기본 구조를 소개해드리겠습니다.

![image](https://user-images.githubusercontent.com/74864925/139631447-66bbe63e-0d13-4d52-926c-0b626c5d798f.png)

- 출처 : https://feel5ny.github.io/

1. 사용자 인터페이스 <br/><br/>
주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등.

2. 브라우저 엔진 <br/><br/>
사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어.

3. 렌더링 엔진 <br/><br/>
요청한 콘텐츠를 표시. 예를 들어 HTML을 요청하면 HTML과 CSS를 파싱하여 화면에 표시함.

4. 통신 <br/><br/>
HTTP 요청과 같은 네트워크 호출에 사용됨.

5. UI 백엔드 <br/><br/>
콤보 박스와 창 같은 기본적인 장치를 그림. (콤보박스 : 드롭다운 목록과 텍스트 상자와 조합한 것)

6. 자바스크립트 해석기 <br/><br/>
자바스크립트 코드를 해석하고 실행.

7. 자료 저장소 <br/><br/>
이 부분은 자료를 저장하는 계층

- 출처 : https://monny.tistory.com/185

우리가 주목해야할것은 3번과 6번입니다.

나머지는 따로 공부하셔도 무방합니다.

## 브라우저 렌더링 자세히보기

![rendering-flow](https://blog.kakaocdn.net/dn/bKz1JU/btqJrw2fQJ7/eLL2cnMhL9CZOixSbnrRtK/img.png)

- 자 이제 시작이야 렌더링

<br/>

요청받은 HTML , XML , 이미지를 브라우저에 표시합니다.

파이어폭스 게코 , 사파리 웹킷 , 크롬의 블링크등 브라우저에 따라 사용하는 엔진도 다릅니다. (마이크로소프트 엣지는 크로미움의 블링크를 채용했다고하네요.)

### DOM TREE 생성

![dom-tree-image](https://poiemaweb.com/img/HTMLElement.png)

- 출처 : https://poiemaweb.com/js-dom

<br/>

첫째로 렌더링 엔진은 HTML 파서를 통해 HTML문서의 최상단부터 읽어내려가기 시작하며 DOM TREE를 생성합니다.

{: .box-note}
**DOM TREE란?** : DOM은 문서객체모델에 약자이며, DOM TREE란 웹 문서를 브라우저가 이해할 수 있는 구조로 구성하여 메모리에 적재한 결과를 트리구조로 표시한 것입니다.

### 야생의 script태그를 만나다.

HTML 파서가 최상단부터 쭉 읽어내려가는 도중 script태그를 만날때가 있습니다.

이때 HTML 파서는 모든 권한을 임시적으로 6번 자바스크립트 해석기에게 부여하고 javascript 엔진은 javscript 파일을 다운후 실행합니다.


> 주의점

만약 head쪽에 script를 넣게되면 HTML 파서가 javascript가 실행될때까지 파싱을 멈추기때문에 

다운 및 실행시간이 길다면 빈화면 또는 로딩화면만 보게되어 유저경험이 안 좋아지게됩니다.

![vscode-script](https://images.velog.io/images/noah071610/post/3d010f9f-8dee-4648-851a-78fff4e12843/image.png)

DOM Tree가 보여질때 반드시 필요한 js파일이거나, 용량이 작은 특수한 경우에만 사용하는게 바람직합니다.

그런 경우가 아니라면 defer나 async 키워드를 사용하거나 (일부 브라우저에서는 지원하지 않습니다.) body 내부의 아래쪽으로 script태그를 넣어주는게 합리적입니다.

![async-defer](https://kimlog.me/static/7b56046cd820d53017f5fa7124ba2255/44a54/script_load.png)

`async` 키워드를 사용하게되면 다운로드중에도 파싱을 계속하고,

`defer` 키워드를 사용하면 다운로드중에도 파싱을 계속하고 파싱이 끝나면 실행합니다.

### CSS 파싱

CSS는 어떻게 파싱 할까요? Internal, Inline, External 세 가지 방법이 있습니다.

Internal과 Inline방법은 style태그 또는 HTML태그속 style을 쓰는 방법이며 HTMLstyleElement 변환 후 DOMTREE에 추가됩니다.

External방법은 link 태그로 연결하는 방법이며 HTMLlinkElement 변환되어 파일을 받아 온 후에 컴파일이됩니다.

### 야생의 CSS는 왜 head태그 안쪽 아래에 둘까?

왜 script는 아래고 css는 위일까요?

기본적으로 DOM TREE를 만들어도 스타일 규칙이 없으면 렌더링 할 수가 없습니다.

즉, 최대한 빨리 스타일 규칙을 알아야 렌더링트리가 완전히 만들어지므로 스타일시트 파일을 모두 다운로드시키기 위해 head태그 사이에 놓는 것이다!

어찌됬든 저찌됬든 이러한 모종의 규칙으로 style sheets는 CSS Parser를 통해 CSSOM 이라는 스타일을 정의한 TREE를 작성합니다.

### 렌더트리 생성

이제 만들어진 DOMTREE 와 CSSOM 을 합쳐서 Render Tree를 만들게 됩니다. 실제 스타일 정보가 포함되어있는 정말 화면에 나오는 녀석들이죠.

참고로 visibility hidden 이나 opacity 0 같은 경우에는 Render Tree에 포함됩니다. 실제로 값은 있지만, 보이지 않을 뿐인거죠.

하지만 display none 같은 경우에는 Render Tree에 포함되지 않습니다.

### 레이아웃

이제 레이아웃 파트입니다. 이때 각노드들이 어디에 위치할지 어떠한 크기인지를 계산합니다. 리렌더링될때 레이아웃이 변경되면 reflow 를 하게됩니다.

### 페인트 및 컴포지션

자 이제 레이아웃이 끝난 요소들을 실제로 그리게됩니다!

와우 정말 긴 과정이였습니다.

### Reflow (레이아웃) 과 Repaint (페인트)

이벤트가 발생됨에 따라서 레이아웃 위치나 크기를 재정비해야될때가 있습니다. 이 과정을 Reflow라고 하는데요, 대표적으로 많이 Reflow되는 경우가

- margin
- padding
- width / height
- top / right / bottom / left
- 노드 추가 / 노드 제거

...

등등이 이겠습니다.

reflow는 레이아웃의 재배치가 일어남으로 연산과정이 많습니다. 가능한한 지양하고 repaint를 노리는게 좋습니다!

repaint 의 대표적인 경우에는

- transform
- background color
- visibility

...

등등이 있습니다.

여기서 style의 변경에 따른 reflow와 repaint여부를 알 수 있습니다!

[https://csstriggers.com/](https://csstriggers.com/).

> 저는 저의 프로젝트에서 이렇게 응용했습니다.

absolute의 값에 위치를 변경할때 left 나 right 값의 사용을 지양하고, transform의 translateX 를 사용해서 위치를 변경해 주었습니다.

최적화 이야기로 넘어가면 이미지스프라이팅기법, 레이지로딩 등등 여러가지가 있는데 일단 여기선 reflow 최소화 정도로 넘어갑시다.

## 아직 한발 남았다.

우리는 렌더링된 페이지를 드디어 우리의 웹 화면에서 볼 수 있게됬지만 아직 브라우저는 한발이 남았습니다.

그건 렌더링된 값을 캐싱하는 과정입니다.

정적인 파일들은 브라우저에 의해 캐싱이 되서 나중에 해당 페이지를 방문할 때 다시 서버로부터 불러와지지 않도록 합니다.

근데 React같은 CSR같은 경우엔 캐싱이 되지 않습니다. (Next.js같은 프레임워크를 쓰면 가능하지만요)

당연히 빈html에 js가 돌아야 그제서 페이지를 생성하니 그렇습니다.

CSR이나 SSR에 대해선 다음 포스팅에 다시 적겠습니다!

## 마치며

우리는 인터넷 네트워크 부터 시작 데이터가 전송 방법, 브라우저 렌딩방법까지 알아보았습니다. 이제 웹이라는 거대한 산이 보이기 시작했습니다.

다음 웹모르는웹개발자 에서는 동적인 웹을 만들어주는 자바스크립트 엔진 원리에 대해서 알아봅시다. 

요즘 모든 웹사이트는 동적웹이라고 생각해도 과언이 아니기때문에 상당히 중요한 원리일겁니다.

읽어주셔서 감사합니다.
