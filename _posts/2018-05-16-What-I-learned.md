---
layout: post
title: What I learned today, 2018-05-16
---

## Upgrading from MySQL to MariaDB

대략 MySQL 5.x 는 MariaDB 5.x 와 바이너리 호환, 즉 데이터 파일을 그대로 두고 MySQL을 지우고 MariaDB를 설치해서 구동하면 데이터 그대로 액세스 할 수 있다. MySQL 8.0 인 경우 `mysqldump` 사용해야 한다. 
`mysqldump`는 database를 복제(?)할 수 있는 sql statements 를 결과물로 내어놓는다. 

References:
* [Upgrading from MySQL to MariaDB, MariaDB KB](https://mariadb.com/kb/en/library/upgrading-from-mysql-to-mariadb/)
