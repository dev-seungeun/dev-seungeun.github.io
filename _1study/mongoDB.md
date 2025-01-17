---
title: MongoDB가 뭘까..
author: SeungEun Baek
date: 2022-01-27 20:30 
description: noSQL과 MongoDB에 대한 짧은 정리
category: bse
layout: post
---
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fdev-seungeun.github.io%2F1study%2FmongoDB%2F&count_bg=%23FEC8E6&title_bg=%23B2ADAD&icon=&icon_color=%23515050&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

## 관계형 데이터베이스(RDBMS)
> Table과 Row, Column이 존재하고 Table간의 관계를 연결시켜서 데이터를 조회하는 형태
```
1. Oracle
2. Mysql
3. MariaDB
4. MS-SQL
5. PostgreSQL
```
   
## 비 관계형 데이터베이스(NoSQL) 
#### __* 대표적으로 MongoDB *__   
> MongoDB는 우리가 개발시 흔히 주고받는 JSON형태의 데이터가 DB에 저장되는 형태 (실제저장은 BSON이지만) : 비정형 데이터
> post와 comment가 있을 경우, RDBMS는 Table 2개로 분리하여 Join하지만 NoSQL에서는 post내부에 comment를 넣는다(서브다큐먼트)
```
1. Wide Columnar Store : 카산드라
2. Document Store : MongoDB
3. Key-Value Store : 다이나모, 레디스
4. Graph Store : Neo4j
```

### MongoDB
#### [Document]
```   
데이터의 필드와 값(Field-Value)를 묶어 한 쌍의 데이터로 구성하고 저장하는 방법
Document다수가 모여서 만들어진 것을 MongoDB에서 Collection이라 부름
Document는 동적 스키마를 갖고있기 때문에, 같은 Collection안의 Document끼리 다른 스키마를 갖고 있을 수 있다. 즉, 서로 다른 데이터들을 가지고 있을 수 있다.
Field(Key)는 12bytes의 hexadecimal값으로서, 각 document의 유일함(uniquness)을 제공한다.
  - 첫 4bytes는 현재 timestamp, 다음 3bytes는 machine id, 다음 2bytes는 MongoDB 서버의 프로세스id, 마지막 3bytes는 순차번호이다. 추가될때마다 값이 높아진다는 말이 된다.
```

#### [Collection]
```   
MongoDB Document의 그룹
```

#### [특징]
```
1. Document-Oriented Storage : 모든 데이터가 JSON형태로 저장되며 스키마가 없음
  - 스키마가 없기 때문에 각 필드는 서로 다른 데이터 타입을 가질 수 있고 데이터베이스에 저장된 Documents도 각기 다른 다양한 필드를 가질 수 있다.
  - 복잡한 구조를 쉽게 저장할 수 있다.
  - Join을 사용하지 않아도 되고 다형성을 가능케 한다.
2. Full Index Support : RDBMS에 뒤지지 않는 다양한 인덱싱 제공
  - 강력한 Index기능 덕분에 거의 모든 쿼리들을 빠르게 처리할 수 있다.
  - 다른 NoSQL에서 찾아보기 힘든 장점이다.
3. Replication & High Availability : 데이터 복제를 통한 가용성 향상
4. Auto-Sharing : Primary Key를 기반으로 여러 서버에 데이터를 나누는 scale-out이 가능
5. Querying : Key기반의 get, put 뿐만 아니라 다양한 종류의 쿼리 제공
  - 강력한 쿼리 기능과 다양한 쿼리를 지원한다.
6. Fast In-Place Updates : 고성능 atomic peration 지원
7. MapReduce : 맵리듀스 지원
8. GridFS : 
  - 별도의 스토리지 엔진을 통해 파일 저장. 스토리지 엔진에서 DB엔진을 분리하는 새로운 아키텍처를 도입
  - MongoDB의 기본 스토리지 엔진인 와이어드 타이거(Wired Tiger)는 높은 쓰기 성능을 제공하고, 압축을 기본 내장해 더 적은 스토리지 비용을 요구
  - 별개의 인메모리와 암호화 데이터스토어를 추가. 또한 MongoDB는 스파크 커넥터를 제공해 대용량 인메모리 분석을 지원
```

#### [장점] 
```
* 쌓아놓고 삭제가 없는 경우가 가장 적합 *

1. Flexibility : Schema-less라서 어떤 형태의 데이터라도 저장할 수 있다.
2. Performance : Read & Write 성능이 뛰어나다. 캐싱이나 많은 트래픽을 감당할 때 써도 좋다.
3. Scalability : 애초부터 스케일아웃 구조를 채택해서 쉽게 운용가능하다. Auto sharding 지원
4. Deep Query ability : 문서지향적 Query Language 를 사용하여 SQL 만큼 강력한 Query 성능을 제공한다.
5. Conversion / Mapping : JSON형태로 저장이 가능해서 직관적이고 개발이 편리하다.  
```

#### [단점] 
```
* 정합성이 떨어지므로 트랜잭션이 필요한 경우에는 부적합 *

1. JOIN이 없다. join이 필요없도록 데이터 구조화 필요
2. memory mapped file으로 파일 엔진 DB이다. 메모리 관리를 OS에게 위임한다. 메모리에 의존적, 메모리 크기가 성능을 좌우한다.
3. SQL을 완전히 이전할 수는 없다.
4. B트리 인덱스를 사용하여 인덱스를 생성하는데, B트리는 크기가 커질수록 새로운 데이터를 입력하거나 삭제할 때 성능이 저하된다. 이런 B트리의 특성 때문에 데이터를 넣어두면 변하지않고 정보를 조회하는 데에 적합하다.
```

## 향후과제
MongoDB는 학습을 위한 적응 용량은 무료로 이용할 수 있다.
MongoDB 사이트에 가입하고 Atlas를 이용해서 데이터를 CRUD해볼 수 있다. 
  * [MongoDB Site](www.mongodb.com)

또, MongoDB University를 가입해서 강의를 들으면 수료증과 Certificate를 취득할 수 있는 시험을 볼 수도 있다. 
  * [MongoDB University](university.mongodb.com)
