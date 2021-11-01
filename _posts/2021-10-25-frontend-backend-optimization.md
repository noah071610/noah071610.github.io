---
layout: post
title: 🏆 맨땅에 헤딩하면서 배운 프론트엔드&백엔드 성능 최적화
subtitle: Front-end & Back-end Optimization after building web site.
tags: [optimization, kiss, yagni, dry]
---

<p></p>

# 구현이 전부가 아니다.

프로젝트 3개 ( 노아월드 , [마이서울가이드](https://myseoulguide.site/) , [Fall IN Asia](https://fallinasia.com) ) 한땀한땀 기획부터 개발, 배포까지 해보며

구현 후에 불필요한 기능, 지저분한 코딩컨벤션, 로딩속도 , 네이밍 , 컴포넌트 비체계화 등 셀수없는 문제들과 맞닥뜨렸다.

구현은 예고편에 불과했고, 유지보수부터 본방인 것이였다.

나는 실제 프로젝트들의 이러한 문제들을 해결하고 프로젝트의 프론트엔드 , 백엔드 성능최적화 와 리팩토링 했던 내용들을 공유하고자한다.


## 1. YAGNI하게 불필요한 기능, 코드를 최대한 개선했다.

### 목적에 부합하지 않으면 과감히 삭제했다. (노아월드 블로그)

나는 일본어통역사 자격증을 가지고있다, 그래서 기술블로그(노아월드)에 일본어강의 페이지를 따로 두면 어떨까 하고 일본어 관련 페이지를 만들었었다.

그안에 일본어관련 퀴즈, 단어장들을 UI적으로 이쁘게 개발하며 꽤나 많은 시간을 쏟고 블로그를 배포했다.

하지만 곰곰히 생각해보니, 내 블로그는 기술블로그인데 겸사겸사 일본어를 배워주겠지 하는건 사실 욕심에 가까웠다.

또한 포트폴리오 페이지도 블로그에 같이 합쳐 배포했는데 이 둘다 유저경험(UX)이 기술블로그에 좋지 못했다.

![](https://images.velog.io/images/noah071610/post/150e4ed6-27b8-4b56-9787-7a55e0308813/20210909_215235.jpg)

- 우연히 개발유튜버에게 프로젝트리뷰를 받았고 이 유튜버님은 내가 직접 포트폴리오 위치를 가르쳐줄때까지 블로그내에 포트폴리오를 찾지못했다...

{: .box-note}
UX적으로 일본어,포트폴리오페이지는 기술블로그에 적합하지않다. **유저는 기술블로그에서 기술에대한 경험을 원한다.**

UX의 목적을 상실한 페이지와 기능들은 짐덩이가 되었고 눈물을 쏟아내며 페이지를 버렸다.

그리고 결국 노아월드 블로그는 아예 싹 갈아엎고 현재는 배포를 중단했다.

노아월드에 문제가 있는건 아니다. 단지 블로그는 정적인 페이지로도 충분히 가능하고 댓글 기능은 Disqus를 이용하여 충분히 구성 가능하기 때문이다.

**개발 전 계획,구성, 그리고 목적 등에 부합하는 기능,코드만 작성하는 것이 중요하다는 것을 정말 뼈저리게 배웠다.**

`YAGNI - You Ain't Gonna Need it` YAGNI 하게 개발하자.

### 선택과 집중을 위해 Fall IN Asia 프로젝트의 분야를 좁혔다.

[Fall IN Asia](https://fallinasia.com) 는 내 가장 최근 프로젝트이다.

나는 처음에 모든 여행자를 위한 커뮤니티를 기획하고 만들어갔다.

하지만 간과한게 있었다. 거의 모든 커뮤니티는 그 목적성이 뚜렷하고 구체화되있는것이다.

"모든 여행자를 위한 커뮤니티"는 구체적이지 못하고 너무 광범위했다.

그래서 개발 도중 범위를 아시아 여행 커뮤니티로 좁혀 프로젝트를 가볍게 가져갔고 속도와 성능을 개선했다.

### Fall IN Asia의 기능 , DB쿼리 , HTTP요청 최소화

[Fall IN Asia](https://fallinasia.com) 에서 인기국가를 불러와서 메인페이지에 넣어주는 기능이 있었다.

허나 DB로 Query가 굉장히 많이 들어가고 HTTP요청도 많아 성능이 많이 저하됬었고 개선이 필요했다.

두가지 선택지가 있었다.

1. 알고리즘 등을 개선
2. 쓰지말자

알고리즘을 개선시키는 방법은 알아냈다. DB에 Column을 하나 추가하면 끝났다.

근데 아무리 생각해도 Table에 Column을 추가시키면서 까지 필요한 기능인가를 곰곰히 생각했다.

또한 구지 쿼리갯수가 아니더라도 이 기능은 많은 연산이 필요하며 이게 메인페이지에 들어가야한다.

메인페이지는 유저가 처음보는 페이지 아닌가? 초기로딩속도 때문에 SSR , 다이나믹라우팅 , 이미지리사이징 등 다 했는데..? 검색엔진에도 별로 안중요하고..

`YAGNI` 의 모토, **정말 필요한가?** 를 생각하고 결국 기능을 삭제했다.

또한 같은 맥락으로 IP를 기반으로한 중복 조회수 방지 기능도삭제했다.
~~(하꼬사이트에서 조회수 어뷰징해주면 오히려 땡큐 아닌가..?)~~

프론트엔드쪽 개발을 하다보면 Network탭에서 불필요하거나 더딘 HTTP요청과 응답이 상당히 많이 보인다.

백엔드쪽 개발을 하다보면 수많은 DB쿼리와 요청들이 터미널로 쏟아져나온다.

**요청하나하나의 무거움을 인지하고 최대한 서버에 무리를 주지않는 쪽으로 개발해야한다.**

## 2. DRY한 코드와 구성으로 유지보수성을 끌어올리고있다.

### 가독성을 유지하며 코드의 중복을 제거했다. 

`DRY - Don't Repeat Yourself` DRY 하게 개발을 하기 위해 코드의 중복을 제거했다.

```javascript
  📁UserInfoAside/index.tsx

  // 함수들이 "클릭하면 유저의 정보를 변경한다 다만 본인이 아니면 반려한다" 는 맥락이 같습니다.
  // 가독성을 유지하며 중복을 최소화하기 위해 코드를 변경하겠습니다.
  
  const onClickConfirmRemoveIcon = useCallback(() => {
    if (!isOwner) {
      toastErrorMessage("본인프로필만 변경 가능합니다.");
      return;
    }
    dispatch(deleteUserIconAction());
  }, [isOwner]);

  const onClickChangeUserInfo = useCallback(() => {
    if (!isOwner) {
      toastErrorMessage("본인프로필만 변경 가능합니다.");
      return;
    }
    dispatch(changeUserInfoAction({ userName, introduce }));
    setUserInfoEdit(false);
  }, [userName, introduce, isOwner]);

  const onClickChangePassword = useCallback(() => {
    if (!isOwner) {
      toastErrorMessage("본인프로필만 변경 가능합니다.");
      return;
    }
    dispatch(changeUserPasswordAction({ prevPassword, newPassword }));
  }, [prevPassword, newPassword, isOwner]);

  const onClickWithdrawal = useCallback(() => {
    if (!isOwner) {
      toastErrorMessage("본인프로필만 변경 가능합니다.");
      return;
    }
    dispatch(mainSlice.actions.toggleWithdrawalModal());
  }, [isOwner]);


  //
  // 👊 중복최소화를 위해 코드를 이렇게 변경했습니다.
  //

  const onClickUserSetting = useCallback(
    (type: string) => {
      if (!isOwner) {
        toastErrorMessage("본인프로필만 변경 가능합니다.");
        return;
      }
      switch (type) {
        case "remove_icon":
          dispatch(deleteUserIconAction());
          break;
        case "user_info":
          dispatch(changeUserInfoAction({ userName, introduce }));
          setUserInfoEdit(false);
          break;
        case "password":
          dispatch(changeUserPasswordAction({ prevPassword, newPassword }));
          break;
        case "withdrawal":
          dispatch(mainSlice.actions.toggleWithdrawalModal());
          break;
        default:
          return;
      }
    },
    [isOwner, userName, introduce, prevPassword, newPassword]
  );

```

### 재사용되는 컴포넌트들을 체계적이게 분리했다.

이부분도 `YAGNI` 의 꼭 필요한가를 염두해두고 `DRY` 만들었다. 

![](https://images.velog.io/images/noah071610/post/278a9c5b-831c-43a0-bbed-9e1e2878509c/%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%93%A4.jpg)

### Custom CSS 및 tailwindCSS 등을 사용하며 가독성과 효율을 높였다.

SCSS를 제거했다. 왜냐하면 나의 프로젝트 특성상 인터랙티브한 사이트도 아니며 그때그때 스타일을 불러오는 CSS in JS가 최적화에 좋았다. 

그리고 어쩔수없이 자주쓰는 CSS 뭉텅이들은 Custom으로 만들어 불러왔고,

불필요하게 루즈하고 긴 스타일시트를 개선하기 위해 tailwindCSS를 썼다.

CSS in JS에 tailwindCSS을 넣어 사용한 이유는 className에 tailwindCSS넣으면 JSX와 style을 최대한 분리시키기로 정한 나의 코딩컨벤션에 부합하지 않기 때문이다.

```javascript
  📁Header/styles.tsx

  export const HeaderWrapper = (headerDownSize: boolean) => css`

    ${tw`w-full bg-white fixed`}
    ${headerDownSize ? tw`py-2 px-4` : tw`py-4 px-8`}
    z-index:60;
    transition: 0.3s all;
    ${BORDER_THIN("border-bottom")};
    ${FLEX_STYLE("space-between", "center")};

    .header-logo {
      ${tw`w-40 h-8 mr-6`}
      transition: 0.3s all;
      ${headerDownSize && `transform:scale(0.8);`}
      img {
        ${tw`w-full h-full`}
      }
    }

    ...

  `;

  📁config.ts

  export const FLEX_STYLE = (justify: string, align: string, flexStyle?: string) => `
    display:flex;
    justify-content:${justify};
    align-items:${align};
    flex-direction:${flexStyle};
  `;

  export const ELLIPSIS_STYLE = (lineHeight: number, clamp: number, height: string) => `
    line-height: ${lineHeight};
    -webkit-line-clamp: ${clamp};
    height: ${height};
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
    word-wrap: break-word;
    text-overflow: ellipsis;
  `;

  ...

```


### inline Style을 제거했다.

inline Style은 리액트 리렌더링을 발생시키므로 지양했다.

```javascript
<div style={{display:'flex' ....}}>Hello World</div>
//요런경우엔 style은 객체이며, {} === {} 는 자바스크립트에서 false이기 때문에
//값이 계속 다르다고 virtual DOM이 인식하고 계속 리렌더링이 된다.
```

### 그외

- 변수명,함수명은 직관적이게 `Keep It Simple, Stupid` #kiss 하게 적었다.

- "99999999" 같이 보기만해도 눈아픈 숫자들은 "9".repeat(8) 처럼 바꾸어주었다.

- `custom hooks` 를 만들어 사용했다.


## 여러가지 최적화 기술을 사용하는 중이다.

### React 리렌더링을 최소화했다.

`useCallback` 와 `useMemo` 를 사용해 함수 또는 상태값을 메모리제이션하고,

컴포넌트 말단 쯔음에 `React.memo` 를 사용해 props 값이 변경되었을 때 리렌더링하게했다.

```javascript
  const countryOptions = useMemo(
    () =>
      countries?.map((v, i) => {
        return { value: v.name, code: v.code };
      }),
    [countries]
  );

	...
   
  const onClickConfirmRemoveIcon = useCallback(() => {
    if (!isOwner) {
      toastErrorMessage("본인프로필만 변경 가능합니다.");
      return;
    }
    dispatch(deleteUserIconAction());
  }, [isOwner]);

	...

 const TopNavigation: FC<IProps> = ({ filter, list, onClickList }) => {
  return (
      <TopNavigationWrapper>
          ....
      </TopNavigationWrapper>
    );
  };

 export default memo(TopNavigation);
    
   
```

###  lazy로딩을 위한 Next의 dynamic import 사용했다.

위지글에디터와 Map 에디터의 용량이 정말 어마무시했는데 이 기법을 사용해서 로딩시간을 많이 단축시켰다.

특히 위지글에디터는 검색엔진이라든지, 초기 구동시 꼭 필요한 기능이 아니였기 때문에 lazy로딩으로 많은 이점을 가졌다.

### 이미지를 크기에 맞게 리사이징 했다.

AWS의 S3와 Lambda를 사용해 유저가 이미지를 올릴시 리사이징해서 다듬었다.

그리고 개인적으로 이미지를 사용할때도 크기를 알맞게 줄이고 압축하는 과정을 통해서 최적화했다.

이부분은 정말 당연하면서도 동시에 등한시해서 최적화 작업에서 큰 비중을 차지했다.

또한 이미지스프라이팅 기법은 현재상황에서 사용한다면 많은 수정작업이 걸리는 `오버엔지니어링` 의 우려가있어 그 개념만 숙지했다.

```javascript

const upload = multer({
    storage: multerS3({
      s3: new AWS.S3(),
      bucket: "noahworld",
      key(req: Request, file: Express.Multer.File, cb) {
        cb(null, 'original/Date.now()_path.basename(file.originalname)');
      },
    }),
    limits: { fileSize: 20 * 1024 * 1024 },
  });

router.post(
  "/icon",
  upload.single("image"),
  async (req: Request, res: Response, next: NextFunction) => {
    try {
      User.update(
        { icon: (req.file as Express.MulterS3.File).location.replace(//original//, "/thumb/") },
          { where: { id: req.body.id } }
        );
         res.json((req.file as Express.MulterS3.File).location.replace(//original//, "/thumb/"));
        } catch (error) {
        console.error(error);
        return next(error);
      }
    }
    );
```

### Next bundle analyzer 를 이용했다.

analyzer로 faker 라이브러리를 Tree shaking 했다. (현재는 아예 삭제했다.)

하지만 UI라이브러리 , Map라이브러리 , 위지글에디터 등은 Tree shaking이 거의 불가능해서 조금 아쉽다.

여러모로 분석하며 최적화하고 있는데 라이브러리 선정도 꽤나 중요한것같다.

### IntersectionObserver 로 infinite scroll을 구현했다. (+Throttle)

Scroll Event 보다 훨씬더 연산횟수가 적었고 만족스러웠다.

만약 Scroll Event 로 구현해야한다면 Throttle 기법을 사용한다. 

Debounce는 아직 사용해본적 없지만 개념은 알고있다.

### DB의 인덱싱과 ngram Full-text검색을 사용했다.

[Fall IN Asia](https://fallinasia.com)에 사용했는데, 이전 풀스캔 방식보다 검색속도가 월등히 향상됬다.

또한 자주 DB에서 찾는 Column들을 인덱싱으로 지정해주었다. 


### 간소하게나마 DB의 정규화와 역정규화를 진행했다.

[Fall IN Asia](https://fallinasia.com)에 DB 테이블을 계속 정규화해서 결국 많은 조인 연산으로 DB 성능이 감소했었다. 테이블 합치거나 분리하는데 기준을 정하고 최적화했다.


# 마치며

3개의 나의 프로젝트들 ( 노아월드 , [마이서울가이드](https://myseoulguide.site/) , [Fall IN Asia](https://fallinasia.com) ) 덕분에 

클론코딩에선 느끼지못할 뿌듯함과 애정을 느꼈고 유지보수에 대해서 알아가고 배워가게 해줘서 고맙다고 말하고싶다. 
