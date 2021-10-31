
---
layout: post
title: 😤 Typescript Multer Error : Property 'location' does not exist on type 'File'.
subtitle: Let's right type AWS, MulterS3 with Typescript.
tags: [typescript, AWS]
---

<p></p>

## Typescript Multer Error

Multer 라이브러리 사용하는 프로젝트에 AWS의 multerS3 를 이용한다면 마주칠 #typescript 에러다.

찾는 방법은 타입스크립트가 어떻게 정의됬는지 찾아보면 된다.

```javascript

const upload = multer({
  storage: multerS3({
    s3: new AWS.S3(),
    bucket: "project",
    key(req, file, cb) {
      cb(null, `original/${Date.now()}_${path.basename(file.originalname)}`);
    },
  }),
  limits: { fileSize: 20 * 1024 * 1024 },
});

router.post("/icon", upload.single("image"), async (req, res, next) => {
  User.update(
    { icon: req.file.location.replace(/\/original\//, "/thumb/") },
    // 이부분에서 에러가 나게되는데 file의 타입이 어떻게 정의되었는지 확인해보자
    { where: { id: req.body.id } }
  );
  res.json(req.file.location.replace(/\/original\//, "/thumb/"));
});

```

ctrl + 클릭해서 타입정의를 보러가자

![](https://images.velog.io/images/noah071610/post/117b85fd-2865-4a61-8e34-153f3313c715/image.png)

Request는 Express의 타입 정의이고 그안에 file 인자는 Multer로 정의되어있다 (IDE가 비쥬얼스튜디오라면 갖다 대기만해도 추론이 가능하다.)

나는 upload 부분에서 분명히 Express.Multer를 multerS3로 확장했는데 타입스크립트는 그부분까지 추론해주지 못한것이다.

## 해결방법

```javascript

router.post("/icon", upload.single("image"), async (req, res, next) => {
  User.update(
    { icon: (req.file as Express.MulterS3.File).location.replace(/\/original\//, "/thumb/") },
    // req의 file 타입을 MulterS3.File로 정의해주자.
    { where: { id: req.body.id } }
  );
  res.json((req.file as Express.MulterS3.File).location.replace(/\/original\//, "/thumb/"));
});

```

비단 Multer뿐만 아닌 에러라 항상 라이브러리의 type 파일을 깊게 들여다보며 

타입정의를 파악하고 필요하다면 수정해 타입 불일치 오류를 미연에 방지하는 습관이 중요하다 


