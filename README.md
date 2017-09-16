PostgreSQL Installation Guide
===
 Installation environment
---
 Ubuntu Version : Xenial (16.04 LTS) <br>
 Apt Version : apt 1.2.24 (amd64) <br>
 PostgreSQL Version : 9.6

## Index
1. Overview
2. About PostgreSQL
3. PostgreSQL Installation
  - Ubuntu add PPA
  + postgreSQL Install
4. PostgreSQL Tutorial
  - postgreSQL 원격 설정
  - DBMS 접속
  - 계정 생성
  - 데이터베이스 생성
  - 데이터베이스에 계정 연동
  - 데이터베이스 접속
 
## Overview
PPA에 대해서 모르신다면 아래의 글을 읽고나서 설치를 진행하시기 바랍니다.

**Personal Packge Archive (PPA)** <br>
리눅스는 업데이트 방식을 각자 프로그램이 직접 하는 게 아닌 패키지 저장소를 이용해 업테이트를 합니다. <br>
이 패키지 저장소가 있어서 사람들은 일일히 검색하면서 홈페이지에 들어가고, 소프트웨어 설치 파일을 다운로드해서 설치하지 않아도 됩니다. <br>
하지만 모든 프로그램이 패키지 저장소에 저장할 수 없기 때문에 PPA가 필요합니다. <br><br>
PPA는 개인 패키지 저장소입니다. <br> 
PPA는 런치패드에서 제공하는 우분투의 공식 패키지 저장소에 없는 서드 파티 소프트웨어를 위한 개인용 소프트웨어 패키지 저장소입니다. <br>
이곳으로부터 사용자는 프로그램을 받아서 사용할 수 있기때문에 우분투 공식 패키지에 아직 올라오지 않은 최신버전의 프로그램등과 같은 것들을 사용할 수 있습니다.

Launchpad : [ <https://www.launchpad.net> ]

## About PostgreSQL

PostgreSQL은 객체 - 관계형 데이터베이스 시스템(ORDBMS)으로, 엔터프라이즈급. DBMS의 기능과 차세대 DBMS에서나 볼 수 있을 법한 많은 기능을 제공하는 오픈소스 DBMS입니다.
실제 기능적인 면에서는 Oracle과 유사한 것이 많아, Oracle 사용자들이 가장 쉽게 적응할 수 있습니다. <br>
 
자세한 정보는 링크를 통해 확인해보길 바랍니다. <br>
PostgreSQL : [ <https://www.postgresql.org> ]

## PostgreSQL Installation
#### Ubuntu add PPA
```
~$ cd /etc/apt/sources.list.d
~$ sudo vi pgdg.list
```

다음과 같이 작성한 후 저장합니다.

```
deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main
```

저장 후 다음 명령어를 실행합니다. 

```
~$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \ sudo apt-key add -
~$ sudo apt-get update
```

#### Install PostgreSQL
```
sudo apt-get install postgresql-9.6
sudo psql --version

```
정상적으로 설치가 완료된다면 다음과 같이 출력됩니다.

```
psql (PostgreSQL) 9.6.5
```

## PostgreSQL Tutorial
#### postgreSQL 원격 설정
```
~$ sudo vi /etc/postgresql/9.6/main/postgresql.conf
```

postgresql.conf 파일에 연 다음 명령어를 추가 후 저장
```
listen_addresses = '*'
```

```
~$ sudo vi /etc/postgresql/9.6/main/pg_hba.conf
```
pg_hba.conf 파일 열고 마지막 라인에 다음 명령어를 추가 후 저장

```
host all all 203.230.0.0/16 md5
```

방화벽 설정

```
~$ sudo ufw status
~$ sudo ufw allow 5432
~$ sudo ufw status
~$ sudo service postgresql restart
```
방화벽 확인 후 5432 포트가 없을 경우 추가합니다.

추가 후 서비스를 재시작합니다.

#### DBMS 접속
```
sudo -u username psql
```

#### 계정 생성
우선, postgres 계정으로 접속합니다.

```
sudo -u postgres psql
```

접속 후 해당 명령어를 실행합니다.

```
postgres=# CREATE USER scott WITH PASSWORD 'tiger';
```

명령어를 실행하면 'tiger'라는 비밀번호를 가진 scott이라는 유저가 생성됩니다.

#### 데이터베이스 생성

```
postgres=# CREATE DATABASE testdb;
```

#### 데이터베이스에 계정 연동

```
postgres=# GRANT ALL PRIVILEGES ON DATABASE testdb TO scott;
```

#### 데이터베이스 접속
```
~$ psql -h host -U scott -d testdb
```
host : 데이터베이스 IP
