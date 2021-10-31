---
layout: post
title: 🔑 MySQL 비밀번호 정책
subtitle: Change policy of MySQL password.
cover-img: /assets/img/path.jpg
share-img: /assets/img/path.jpg
tags: [mysql, ubuntu]
---

# MySQL 비밀번호 강도를 변경해보자.

Mysql 의 MySQL_secure은 우리의 DB를 안전하게 지켜준다.

여러가지 보안정책중에 비밀번호에 강도를 지정하는 password_policy 가 있고 강도는

LOW 부터 MIDIUM , HIGH까지 있는데 문제가

**공인인증서이랑 견줄만하게 만들어야 하는게 MIDIUM 정도 된다.**

즉 MYSQL secure을 받고 password_policy 강도를 높게하면 기존에 비밀번호로는 높은 확률로 정책에 어긋나게되있다.

이때 password_policy를 다시 LOW로 만들어주거나 비밀번호를 변경해야되는데 안전제일이긴하지만 안쓰던 비밀번호 쓰다 까먹는게 더 위험하기때문에 본인 프로젝트에서는 LOW로 바꿔줬다.

```linux
mysql -uroot -p

1)비밀번호 정책변경
set global validate_password_policy=LOW;
or
SET GLOBAL validate_password.policy=LOW;
//위에 둘다 해보고 ok 되면 아래 명령어 실행

2)비밀번호 변경
show variables like 'validate_password%';
or
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '비밀번호'; <==따옴표 지우지마세요

3)비밀번호 정책부합 테스트
select password('테스트비밀번호'); <= 따옴표 지우지마세요

```



