---
layout: post
title: mysql 로 데이터 관리하기 - 설치부터 사용법까지
comments: true
categories: [tech,programming]
---

최근에 mysql data를 사용할 일이 있어서, 공부를 한번 해봤습니다.
생각보다 배워야 할 내용들이 많아서, 아래처럼 정리를 일단 해보았습니다.

---
### 설치는 어떻게 하나요?
맥에는 homebrew 라는 패키지 관리 앱이 거의 표준으로 쓰이고 있다고 합니다.
제 컴퓨터는 Mac 인데요, 오늘은 이 homebrew를 사용해서 설치해보도록 하겠습니다.
명령어는 다음 블로그에 무척 자세하게 정리되어 있어서 참고했습니다: [블로그 링크](https://www.44bits.io/ko/keyword/homebrew)
아래에는 관련 명령어들을 모두 요약해보았는데요, 이중에서 필요한 부분만 사용해 mysql 을 설치했습니다.
(brew 설치, 업데이트, mysql 검색 및 설치)

<pre><code>sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" # brew 설치
brew update             # brew 앱 업데이트
brew search mysql       # mysql 관련 패키지 검색
brew install mysql      # mysql 패키지 설치
brew install mysql@5.5  # mysql 을 특정 버전으로 설치
brew list               # 설치된 패키지 목록 보기
brew list mysql         # 설치된 mysql 관련 패키지 목록 보기
brew info mysql         # 설치된 mysql 패키지의 정보 보기
brew upgrade mysql      # mysql 버전 업그레이드
brew upgrade            # 모든 패키지의 버전 업그레이드
brew remove mysql       # mysql 패키지 삭제
brew uninstall mysql    # mysql 패키지 삭제 - 위와 동일한 결과
brew cleanup mysql      # mysql 의 최신버전을 제외한 outdated 버전 삭제
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" # brew 삭제 </code></pre>
---
### mysql 설치 후 설정하고 시작하기
mysql 을 설치하신 뒤에는, 몇가지 설정을 해야 mysql 을 사용할 수 있게 됩니다.
mysql 설정 또한 무척 잘 설명된 블로그가 있으니 한번 참고해보세요! [블로그 링크](https://whitepaek.tistory.com/16)

<pre><code>mysql.server start          # mysql 서버 실행하기
mysql_sequre_installation   # mysql 서버 설정하기 (아래 참조)
mysql -u root -p            # root 게정으로 mysql 접속, 비밀번호 필요
exit                        # mysql 로그아웃
quit                        # mysql 로그아웃
mysql.server stop           # mysql 서버 중지</code></pre>
mysql_sequre_installation 명령어를 입력하면, 몇가지 질문에 대해 yes/no 선택 혹은 설정이 필요합니다.
아래를 참고해서 각자 원하시는 방법대로 설정해보세요!
1. Would you like to setup VALIDATE PASSWORD component?: yes 인 경우 비밀번호를 어렵게 (숫자문자 조합) 만들어야 함
1. Please set the password for root here. : 비밀번호를 만들어주세요
1. Remove anonymous users? : yes 인 경우 접속 시 -u 옵션으로 유저를 지정해야 함
1. Disallow root login remotely? : yes 인 경우 원격 접속 불가능
1. Remove test database and access to it?: yes 인 경우 테스트로 미리 만들어져 있는 데이터베이스를 삭제함
1. Reload privilege tables now?: yes 인 경우 위의 설정을 테이블에 적용.

설정을 다 하고나면 mysql 접속이 가능해집니다!

---
### mysql 에 접속해 데이터베이스 생성 및 삭제하기

아래에 기본적인 명령어들을 모아보았습니다.
mysql 접속한 뒤에는 ```mysql> ``` 와 같은 콘솔로 바뀝니다.
명령어는 대/소문자를 구분하지 않는 것 같고, semi-colon (;) 으로 끝내야 합니다.

<pre><code>mysql -u root -p                    # root 게정으로 mysql 접속, 비밀번호 필요
show databases;                     # 어떤 databases 들이 있는지 확인
create database example_db;         # example_db 라는 데이터베이스 생성
remove database example_db;         # example_db 라는 데이터베이스 삭제
use example_db;                     # example_db 라는 데이터베이스 사용하기
show tables;                        # example_db 에 있는 테이블 목록 보기
desc example_table;                 # example_table 테이블의 구조 확인
show columns from example_table;    # example_table 테이블의 구조 확인</code></pre>

---
### mysql 테이블 만들기
테이블을 만들 때는 기본적으로 다음 형식으로 작성합니다.
테이블 이름 다음에는 괄호 안에 필요한 컬럼 이름과 형태를 적어줘야 합니다.
comma(,) 로 구분하고, 마지막엔 comma 가 없습니다.

<pre><code>create table example_table (
    name char,
    weight tinyint,
    height int,
    birthday date
);</code></pre>

컬럼에 대한 형태 및 설정은 다음과 같이 하면 됩니다. [링크 참조](https://www.mysqltutorial.org/mysql-create-table/):
<pre><code>column_name data_type(length) [NOT NULL] [DEFAULT value] [AUTO_INCREMENT] column_constraint;</code></pre>

1. 데이터 타입은 numeric, date and time, string, spatial, JSON 등 다양합니다. 다음 링크에서 확인할 수 있습니다: [링크](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
1. 데이터의 길이 등을 괄호 안에 지정해줄 수 있는데요, 예를 들어 char(3) 은 최대 3글자 까지 허용해준다는 뜻입니다. [링크](https://www.w3schools.com/sql/sql_datatypes.asp)
1. NOT NULL 이라고 쓰면, 해당 컬럼에는 NULL 값이 들어갈 수 없게 됩니다.
1. DEFAULT value 부분에는 입력안했을 때의 기본값을 설정할 수 있습니다.
1. AUTO_INCREMENT 는 새로운 row가 추가될 때마다 자동으로 1씩 증가한다는 뜻입니다. 각 테이블에는 최대 1개의 AUTO_INCREMENT 컬럼을 만들 수 있습니다. 또한 이 값은 항상 key로 설정해 줘야 합니다.
1. column constraint 에는 UNIQUE, CHECK, primary key 등을 추가할 수 있습니다. 예를 들어, UNIQUE 는 해당 컬럼 값들이 서로 달라야 한다는 뜻입니다.

위 항목을 참고하면 다음과 같이 테이블을 만들 수 있습니다:

<pre><code>create table example_table (
    name char(12),
    sex char(6),
    id char(20) not null unique,
    number int AUTO_INCREMENT,
    weight tinyint,
    height int,
    birthday date,
    primary key(number)
);</code></pre>

---
### mysql table 을 써보자!
이제 테이블을 만들었으니 사용을 해볼까요?
다음 명령어들을 통해 테이블 내용을 수정, 확인할 수 있습니다.

<pre><code>show tables;                                              # example_db 에 있는 테이블 목록 조회
desc example_table;                                       # example_table 테이블의 구조 확인
show columns from example_table;                          # example_table 테이블의 구조 확인
select name from example_table;                           # example_table 에서 'name' 컬럼 내용 조회
select name, birthday from example_table;                 # example_table 에서 'name,birthday' 컬럼 내용 조회
select * from example_table;                              # example_table 에서 모든 컬럼 내용 조회
select * from example_table limit 5;                      # 모든 컬럼 내용 중 '5개'만 조회
select name from example_table where weight < 60;         # weight 가 60 보다 작은 항목의 name 조회
select height from example_table where name = 'Joey';     # name 이 'Joey' 인 항목의 height 조회
select sex from example_table group by sex;               # 'sex' 컬럼의 내용을 sex 별로 조회하기
select sex, avg(height) from example_table group by sex;  # 'sex 와 'height'의 평균값을 sex 별로 조회하기
select * from example_table order by weight asc;          # 모든 컬럼을 'weight' 오름차순으로 나열해 조회하기
select * from example_table order by weight desc;         # 모든 컬럼을 'weight' 내림차순으로 나열해 조회하기</code></pre>

또한, 다음 명령어들을 사용해서 컬럼의 추가/삭제가 가능합니다.
insert 의 경우, NOT NULL 인 컬럼을 제외하고는 모든 값을 추가해줘야 합니다.
Update 의 경우 set 명령어 뒤에 바꿀 내용을 작성해주는데요, where 뒤의 조건에 맞는 모든 row에 대해 수정을 진행합니다.
<pre><code>insert into example_table(name,id) values('Ross','RG123');   # name, id 컬럼이 각각 Ross, RG123 이라는 row 추가하기
update example_table set id = 'RG1004' where name = 'Ross';  # name 이 'Ross'인 row의 id값을 RG1004로 교체
delete from example_table where name = 'Ross';               # name 이 'Ross'인 row 삭제</code></pre>

일단 기본적인 명령어들을 모아서 정리해보았습니다.
생각보다 공부해야 할 내용들이 많은 것 같습니다.
다음엔 좀 더 재밌는 내용을 찾아봐야겠군요.

그럼 다음에 만나요!