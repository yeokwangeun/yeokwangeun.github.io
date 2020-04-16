### Database란

- 구조화된 데이터. 구조화됐기 때문에 쉽게 관리할 수 있다. 쿼리를 이용해서 데이터를 추가, 삭제, 수정, 조회를 할 수 있다.

### Database의 구조

- table: 우리가 아는 row와 column으로 구성된 그 표
- database: table을 카테고리에 맞게 묶어놓은 것
- database server: database들이 저장된 곳

### mysql - database

- sql: structured query language

```mysql
CREATE DATABASE 'database_name' CHARACTER SET utf8 COLLATE utf8_unicode_ci;
SHOW database_name;
USE database_name;
DROP DATABASE database_name;
```

### mysql-table

- schema: 테이블에 들어갈 데이터의 구조와 형식을 결정하는 것. 데이터베이스는 마구잡이 데이터 저장소가 아니다!

```mysql
CREATE TABLE table_name (
	col_name_1 data_type,
  col_name_2 data_type,
);

CREATE TABLE `student` (
    `id`  tinyint NOT NULL ,
    `name`  char(4) NOT NULL ,
    `sex`  enum('남자','여자') NOT NULL ,
    `address`  varchar(50) NOT NULL ,
    `birthday`  datetime NOT NULL ,
    PRIMARY KEY (`id`)
);
```

### data type

- CHAR(고정문자. 메모리 고정), VARCHAR(가변문자), TEXT(65535 길이까지)
- TINYINT(1B), SMALLINT(2B), INT(4B)
- FLOAT, DOUBLE
- DATE(yyyy-mm-dd), DATETIME(yyyy-mm-dd hh:mm:ss)
- ENUM: 정해진 값을 강요. 선택지가 있는 데이터에서 활용.

### Create: INSERT

```mysql
DESC topic -> describe topic

INSERT INTO 
topic (title, description, created, author, profile) 
VALUES('MySQL', 'MySQL is...', NOW(), 'egoing', 'developer')
```

### Read: SELECT

- SELECT column FROM table WHERE 조건 ORDER BY 정렬기준column (DESC) LIMIT 100

```mysql
SELECT title, created FROM topic WHERE author='egoing' ORDER BY ID DESC
```

### Update: UPDATE

- UPDATE table SET 바꾸고싶은내용 WHERE 바꾸고싶은row
- WHERE을 안쓰면 큰일난다

```mysql
UPDATE topic SET description='Oracle is...' WHERE id=2;
```

### Delete: DELETE

- DELETE FROM table WHERE 지우고싶은row
- WHERE 안쓰면 큰일난다. 인생이 바뀔 수 있다.

```mysql
DELETE FROM topic WHERE id=5
```



### 관계형 데이터베이스

- 중복의 냄새가 나면 효율적으로 개선할 여지가 있다는 뜻

| id   | title        | description        | author | profile        |
| ---- | ------------ | ------------------ | ------ | -------------- |
| 1    | MySQL        | MySQL is...        | egoing | developer      |
| 2    | Oracle       | Oracle is...       | duru   | data engineer  |
| 3    | MySQL Server | MySQL Server is... | egoing | developer      |
| 4    | PostgreSQL   | PostgreSQL is...   | egoing | developer      |
| 5    | MongoDB      | MongoDB is...      | taeho  | data scientist |

- author table을 따로 만들고, topic 테이블에서는 author로 참조값만을 주자

| id   | title        | description        | author_id |
| ---- | ------------ | ------------------ | --------- |
| 1    | MySQL        | MySQL is...        | 1         |
| 2    | Oracle       | Oracle is...       | 2         |
| 3    | MySQL Server | MySQL Server is... | 1         |
| 4    | PostgreSQL   | PostgreSQL is...   | 1         |
| 5    | MongoDB      | MongoDB is...      | 3         |

- 중복에서 나오는 비효율은 제거했다. 하지만 테이블을 나누면 아무래도 한 눈에 정보가 보이지 않는다.
- 저장은 분할해서 하고, 읽어올 때는 합쳐서 읽을 수 있다면?

### JOIN!!!

```mysql
SELECT * FROM topic LEFT JOIN author ON topic.author_id=author.id;

SELECT topic.id AS topic_id, title, description, created, name, profile FROM topic LEFT JOIN author ON topic.author_id=author.id;
```

