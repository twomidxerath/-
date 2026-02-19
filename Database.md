# 데이터베이스

## 용어 정리

Relation : 데이터가 저장되는 2차원 테이블 전체를 말한다.

Attribute : 속성. 표의 열을 의미하고, 데이터의 특징을 나타낸다. 학번, 이름, 학과 등이 이곳에 속한다.

Tuple : 표의 행으로, 실제 데이터 한 건을 의미한다. 위에서 나온 Relation이 이 Tuple의 집합으로, Tuple 사이의 순서는 그다지 중요하지 않다.

Domain : 하나의 Attribute가 가질 수 있는 값의 범위. 학년 속성의 도메인은 1~4가 될 것이다.

Schema : 데이터베이스의 설계도. 자주 변경되지 않는다.

Instance : 특정 시점에 저장된 실제 데이터를 의미한다. 수시로 데이터가 업데이트되며 변화한다.


## 관계형 데이터베이스를 사용하는 이유

가장 큰 이유는 데이터 독립성 때문이다. 기존의 파일 시스템은, 데이터에 새로운 컬럼을 추가하거나 구조가 바뀌면 코드를 전부 뜯어고쳐야만 한다.하지만, DBMS(Database Management System)를 사용하면, 데이터의 구조와 응용 프로그램이 철저하게 분리되어 데이터의 구조가 변화해도 프로그램은 영향받지 않는다.


## 관계 대수

릴레이션에서, 원하는 데이터를 어떻게 골라낼 것인지를 정의한 연산 방법. SQL의 바탕이 된다.

<img width="940" height="149" alt="image" src="https://github.com/user-attachments/assets/034ee70d-45ff-4523-8a6f-eb7a37ec77a8" />

<img width="940" height="130" alt="image" src="https://github.com/user-attachments/assets/c172b22c-cbd3-433d-bb16-36c12425c169" />

<img width="940" height="121" alt="image" src="https://github.com/user-attachments/assets/2ea9f1b7-ca30-48d7-bd65-334b41aa1bea" />

<img width="940" height="139" alt="image" src="https://github.com/user-attachments/assets/a7d8c53b-3dc9-4611-920c-0dfbf417aaf1" />

카티션곱의 경우, 열의 개수는 두 Relation의 열 개수를 더한 만큼이 된다. 즉, 위의 예시에서는 7개의 열이 생성된다. 일반적인 행렬의 연산을 생각하면 안 된다. 구조가 완전히 다르다. 이러한 특성은 이후 등장할 조인에서도 똑같이 적용된다.

<img width="940" height="329" alt="image" src="https://github.com/user-attachments/assets/1e638be1-fa0d-4b4b-9166-a620930291d8" />

<img width="940" height="159" alt="image" src="https://github.com/user-attachments/assets/a8a98ed7-fc15-4f72-bb06-a3810286d020" />

<img width="940" height="211" alt="image" src="https://github.com/user-attachments/assets/5d73d770-cb41-4f1f-be1d-3a645836a0ba" />

<img width="940" height="165" alt="image" src="https://github.com/user-attachments/assets/dea51299-921c-4710-8b90-098add79714d" />

<img width="940" height="207" alt="image" src="https://github.com/user-attachments/assets/fe11add7-289c-4d55-8d0a-750aea8fbce9" />

<img width="940" height="416" alt="image" src="https://github.com/user-attachments/assets/35a66e8f-24b2-4463-9ed0-1c41709b8420" />

## SQL

SQL은 기존에 배웠던 프로그래밍 언어와는 구조적으로 큰 차이가 있다. 기존의 프로그래밍 언어들은 별도의 라이브러리 없이는 데이터를 조회하기 위해선 반복문과 조건문을 사용해 직접 데이터를 순회하며 검색해야 한다. 하지만, SQL은 DBMS와 응용 프로그램이 철저하게 분리되어있기 때문에 이것이 불가능하다. 대신, 내가 어떤 데이터를 찾는지 조건만 데이터베이스에 입력하면 알아서 DBMS가 데이터를 가져온다.

SQL 명령어는 목적에 따라 크게 3가지로 나뉜다.

### 1. DDL(데이터 정의어, Data Definition Language)

schema 자체를 설계하고 변경할 때 사용한다. 다른 프로그래밍 언어로 치면 선언부에 속한다.

CREATE : 테이블을 새로 만든다.

ALTER : 테이블 구조를 변경한다.

DROP : 테이블 구조 자체를 완전히 날려버린다.

### 2. DML(데이터 조작어, Data Manipulation Language)

실제 instance 값을 변경할 때 사용한다. 제일 많이 사용해야 하는 코드다.

INSERT : 테이블에 새로운 튜플 추가.

SELECT : 원하는 데이터를 검색.

UPDATE, DELETE : 데이터를 수정하고 삭제.

### 3. DCL(데이터 제어어, Data Control Language)

데이터베이스 접근 권한을 관리한다.

GRANT, REVOKE : 각각 사용자에게 권한을 부여, 회수.



## DDL

### CREATE

표를 생성하는 명령어로, C의 구조체 선언과 상당히 유사한 구조를 띈다.

```sql

CREATE TABLE 테이블이름 (
    컬럼명1 데이터타입 [제약조건],
    컬럼명2 데이터타입 [제약조건],
    ...
);
```

테이블 이름과 컬럼명은 사용자가 직접 설정하는 것이고, 우리가 생각해야 할 것은 딱 두 가지이다. 데이터타입과 제약조건.


1. 데이터타입

다른 프로그래밍 언어의 데이터 타입과 겹치거나 비슷한 부분이 있다. 자주 사용되는 표준 타입들은 다음과 같다.

INT : 정수형 데이터

VARCHAR(n) : 최대 n의 길이를 가지는, 가변길이 문자열.

CHAR(n) : 무조건 n바이트의 공간을 차지하는 고정 길이 문자열. 학번/주민등록번호같이 반드시 길이가 일정해야 하는 데이터에 사용된다.

DATE/DATETIME : 날짜와 시간.


2. 제약조건

테이블에 쓰레기 데이터가 들어오는 것을 방지하기 위해 컬럼별로 다른 제약조건을 거는 것.

PRIMARY KEY : 제일 중요한 제약조건이다. 테이블에서 각 튜플을 유일하게 식별할 수 있는 대표 컬럼이라고 볼 수 있다. 중복된 값도 들어올 수 없고, 값을 비워두는 것도 불가능하다. 무조건 고유한 값이 이 안에 들어와있어야 한다. 학번, 주민번호 등 식별자에 이러한 조건을 걸어두는 편이다.

NOT NULL : 값을 비워두는 것만 금지한다.

UNIQUE : 중복되는 값이 들어오는 것만 금지한다. 값을 비워두는 것은 허용한다.


### ALTER

이미 만들어진 테이블을 수정할 때 사용한다. 크게 세 가지 기능을 가진다.

1. 새로운 열 추가하기 (ADD)

```sql
ALTER TABLE Student ADD 전화번호 VARCHAR(20);
```

2. 기존 열의 데이터 타입 변경하기 (MODIFY)

```sql
ALTER TABLE Course MODIFY 과목코드 VARCHAR(10);
```

이때 바꾸기 전의 데이터 타입은 중요하지 않으니, 바꿀 데이터 타입만 명시하면 된다. DBMS 종류에 따라 MODIFY 대신 ALTER COLUMN을 사용하기도 한다.

3. 기존 열 삭제하기 (DROP COLUMN)

```sql
ALTER TABLE Student DROP COLUMN 전화번호;
```

이때 전화번호 열에 들어있던 데이터가 전부 지워진다. 다른 데이터는 건드리지 않는다.


### DROP

테이블의 schema와 그 안에 든 모든 데이터를 통째로 삭제하는 명령어이다. 복구하기 까다로우니 신중하게 사용해야만 한다.

```sql
DROP TABLE Course;
```



## DML

백엔드 코드를 짤 때 제일 많이 작성해야 하는 부분이다. 이를 CRUD (Create, Read, Update, Delete) 작업 이라고 부르기도 한다.

### INSERT

테이블에 새로운 튜플을 추가할 때 사용한다.

```sql
INSERT INTO Student (학번, 이름, 전공)
VALUES (101, '홍길동', '소프트웨어학과');
```

데이터를 추가할 때는, PRIMARY KEY나 NOT NULL로 지정되어 반드시 값을 넣어야 하는 속성이 아니라면, 값을 넣지 않아도 된다. 자동으로 NULL 값이 들어갈 것이다.

참고로, SQL은 줄바꿈과 띄어쓰기를 전혀 신경쓰지 않는 방식을 채용했다. 위의 코드는 2줄이지만, 1줄로 쭉 늘여쓴 것과 완전히 동일한 효과를 낸다.


### SELECT

SQL에서 가장 중요한 문법이라고 해도 과언이 아닌 문법이다. 이 명령어를 어떻게 다루는 지에 따라 데이터베이스의 최적화 정도가 천차만별로 갈린다. 관계 대수의 셀렉션과 프로젝션이 합쳐진 문법이다.

```sql
SELECT 이름 FROM Student WHERE 전공 = '소프트웨어학과';
-- 학생 테이블 전체에서 전공이 소프트웨어학과인 모든 데이터를 조회한다.

SELECT * FROM Student;
-- 이렇게 별다른 조건을 걸지 않고 학생 테이블 전체를 조회하는 것도 가능하다.
```


### UPDATE

이미 들어간 데이터를 다른 값으로 변경할 때 사용한다. 

```sql
UPDATE Student SET 전공 = '인공지능' WHERE 학번 = 102;
-- 학번이 102인 학생을 찾아 전공을 인공지능으로 바꾸는 코드.
```


### DELETE

테이블에서 특정 튜플을 아예 지워버릴 때 사용한다.

```sql
DELETE FROM Student WHERE 학번 = 101;
-- 학번이 101인 학생을 테이블에서 삭제.
```


### ORDER BY

가져온 데이터를 오름차순이나 내림차순으로 정렬할 때 사용. 별다른 정렬 알고리즘을 짤 필요는 X

```sql
SELECT * FROM Student ORDER BY 학번 DESC;
-- DESC는 내림차순, ASC는 오름차순으로 정렬하라는 뜻.
```





SQL의 모든 문법에서 적용되는 주의사항인데, WHERE은 반드시 주의해서 사용해야만 한다. SQL은 일반 프로그래밍 언어와 달리 디스크에 영구적으로 저장된 데이터베이스에 접근하기 때문에 이러한 실수가 훨씬 치명적으로 다가오기 때문이다.












