# MongoDB

- 기간 : 2022.07.12



## MongoDB 설치

- [MongoDB 다운로드](https://www.mongodb.com/try/download/community)
  - Install MongoD as a Service 체크
    - MongoDB 서버를 서비스로 등록시킬지 여부로 체크하면 작업관리자에서 항상 실행됨
    - Daemon : 멀티태스킹 운영체제에서 사용자가 직접적으로 제어하지 않고, 백그라운드에서 돌면서 여러 작업을 하는 프로그램
  - Install MongoDB Compass 체크 해제
    - workbench 프로그램인데 설치할 경우 문제가 많기 때문에 체크 해제 필수

![image-20220712143218201](mongodb01.assets/image-20220712143218201.png)

![image-20220712143451252](mongodb01.assets/image-20220712143451252.png)

- 환경변수 설정
  - 윈도우 검색창 > 시스템 환경변수 편집 > 고급 > 환경 변수 > 시스템 변수 > Path 클릭 > 편집 > 새로 만들기 > bin 폴더 경로 변수로 설정
  - 나의 경우 bin 폴더는 C 드라이브에 있고,  Data Directory는 D 드라이브로 설정



## MongoDB 에 대하여

### 1. MongoDB

- 몽고DB 는 크로스 플랫폼 도큐먼트 지향 데이터베이스 시스템
- NoSQL 데이터베이스로 JSON 과 같은 동적 스키마형 도큐먼트들을 선호

### 2. NoSQL

- 고정되지 않은 스키마
  - 필요할 때마다 필드를 추가 및 제거 가능해서 개발 속도 향상

- 데이터 간의 관계를 정의하지 않은 데이터베이스
  - db > collection > document

- 분산형 구조 (대용량 데이터 저장 용이)
  - sharding 지원 (클러스터 데이터 상호 복제)


### 3. Shell

- interactive javascript interface 
  - javascript interpreter 사용
  - js program, library, function 활용 가능
  - mongo에서 사용하는 js function은 console.log 대신 `print`, `printjson` 사용


```
mongo
```

```
> function test(){
... print('test')
... }
> test()
test
> function test2(n){
... return 2 + n
... }
> test2(3)
5
```

### 4. Structure

- database
  - 독립적인 하나의 권한, 각각의 database는 분리된 파일로 저장
- collection
  - document 들의 group (rdbms - table)
  - schema 를 가지지 않음 (document들의 field가 각각 다름)
- document
  - data record 를 [BSON(Binary JSON)](https://bsonspec.org/) 으로 저장
    - BSON 은 JSON 문서의 이진 표현이지만 JSON 보다 더 많은 [데이터 타입](https://www.mongodb.com/docs/manual/reference/bson-types/)을 포함
  - field(key) 중복 불가





## Documents

- MongoDB 는 데이터를 BSON documents 로 저장

![image-20220712152129095](mongodb01.assets/image-20220712152129095.png)

### 1. Document Structure

- MongoDB의 documents는 field 와 value 의 한 쌍으로 구성되어 있음

```
{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}
```

- value 값은 다른 document, arrays, documents 의 arrays 등을 포함하여 BSON 모든 데이터 타입이 될 수 있음
  - BSON 타입 : Double, String, Object, Array, Binary data, Undefined, **ObjectId**, Boolean, Date, Null, Regular Expression, JavaScript, 32-bit integer, Timestamp, 64-bit integer, Decimal128, Min key, Max key

```
var mydoc = {
				_id: ObjectId("5099803df3f4948bd2f98391"),
				name: {first: "Alan", last: "Turing"},
				birth: new Date('Jun 23, 1912'),
				death: new Date('Jun 07, 1954'),
				contribs: ["Turing machine", "Turing test", "Turingery"],
				views : NumberLong(1250000)
}
```

- field 이름은 string 이며, `_id` 는 primary key 로 사용됨

### 2. Dot Notation

- 특정 배열의 값 접근 : `<array>.<index>`

```
{
   ...
   contribs: [ "Turing machine", "Turing test", "Turingery" ],
   ...
}
```

```
contribs.2
```

- 특정 document 의 field 접근 : `<embedded document>.<field>`

```
{
   ...
   name: { first: "Alan", last: "Turing" },
   contact: { phone: { type: "cell", number: "111-222-3333" } },
   ...
}
```

```
contact.phone.number
```

### 3. Document Structure 활용

#### 3.1. Query Filter Documents

- Query Filter Documents 는 어떤 데이터를 읽고, 수정하고, 삭제할 것인지 조건을 특정지을 때 사용

```
{
	field1: "value1",
	field2: { operator: "value" },
	...
}
```

#### 3.2. Update Specification Documents

- Update Specification Documents 는 [update operators](https://www.mongodb.com/docs/manual/reference/operator/update/#std-label-update-operators) 를 사용해 데이터 수정을 특정지을 때 사용

```
{
	operator1: { field1: "value1", ... },
	operator2: { field2: "value2", ... },
	...
}
```

#### 3.3. Index Specification Documents

- Index Specification Documents 는 인덱싱할 필드와 유형 정의할 때 사용

```
{ field1: type1, field2: type2, ... }
```



## MongoDB 시작하기

- database 목록 확인
  - admin : root database
  - local
    - 복제되지 않는 db(특정 서버에만 저장하는 경우)
    - 여기서 저장한 collections 들은 다른 server에 복제 안됨
  - config : shard 정보 저장

```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
```

- 현재 사용 중인 database 확인

```
> db
test
```

- 특정 database 로 이동

```
> use local
switched to db local
```

- 현재 database 에서의 collection 목록 확인

```
> show collections
multi
```



## CRUD Operations

### 1. [Create] Insert Documents

```
db.collection.insertOne()
db.collection.insertMany()
```

- `db.collection.insert()`
  - 명시한 `collection`이 존재하지 않는 경우 자동 생성
  - `_id` 을 명시하지 않으면 자동 생성

```
> db.multi.insert({name: 'hong-gd', class: 'DE', midterm: {kor: 100, eng: 70, math: 60}})
WriteResult({ "nInserted" : 1 })

> db.multi.find()
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
```

- `db.collection.insertOne()`
  - 변수에 넣어서 삽입

```
> var lee = {name: 'lee-ss', class: 'DS', kor: 80, eng: 100, math: 100}
> db.multi.insertOne(lee)
{
        "acknowledged" : true,
        "insertedId" : ObjectId("62cd856fbcf0ff805b72e04c")
}

> db.multi.find()
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
```

- `db.collection.insertMany()`

```
> db.multi.insertMany(
... [
...  {'name':'류재선', class:'de', kor:100},
...  {'name':'김세진', eng:100, math:100},
...  {'name':'윤지원', sci:100, hobby: 'study'}
... ]
... )
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("62cd1817c3c040c04e330449"),
                ObjectId("62cd1817c3c040c04e33044a"),
                ObjectId("62cd1817c3c040c04e33044b")
        ]
}
```

### 2. [Read] Query Documents

```
db.collection.find( { filter } )
```

#### 2.1. SQL 과 Query

- 전체 데이터

```sql
SELECT * FROM collection
```

```
db.collection.find({})
db.collection.find()
```

- 등식 조건에 맞는 데이터

```sql
SELECT * FROM collection WHERE status = "D"
```

```
db.collection.find({status: "D"})
```

```sql
SELECT _id, item, status from inventory WHERE status = "A"
```

```
db.collection.find( { status: "A" }, { item: 1, status: 1 } )
```

- [**Query Operators** ](https://www.mongodb.com/docs/manual/reference/operator/query/)를 사용한 조건에 맞는 데이터

```sql
SELECT * FROM collection WHERE status in ("A", "D")
```

```
db.collection.find({status: {$in: ["A", "D"]}})
```

- AND 조건에 맞는 데이터

```sql
SELECT * FROM collection WHERE status = "A" AND qty < 30
```

```
db.collection.find({status: "A", qty: {$lt: 30}})
```

- OR 조건에 맞는 데이터

```sql
SELECT * FROM collection WHERE status = "A" OR qty < 30
```

```
db.collection.find({$or: [{status: "A"}, {qty: {$lt: 30}}]})
```

- AND 와 OR 조건에 맞는 데이터

```sql
SELECT * FROM collection WHERE status = "A" AND (qty < 30 OR item LIKE "p%")
```

```
db.collection.find({
	status: "A",
	$or: [{qty: {$lt: 30}}, {item: /^p/}]
})
```

#### 2.2. Query Selectors

##### Comparison

- 특정 값 초과 데이터

```
{ field: { $gt: value } }
```

- 특정 값 이상 데이터

```
{ field: { $gte: value } }
```

- 특정 값 미만 데이터

```
{ field: { $lt: value } }
```

- 특정 값 이하 데이터

```
{ field: { $lte: value } }
```

- 지정한 배열 안의 값이 있는 데이터

```
{ field: { $in: [ value1, value2, ... , valueN ] } }
```

- 특정 값이 아닌 데이터

```
{ field: { $ne: value } }
```

- 지정한 배열 안의 값이 없거나 해당 필드가 존재하지 않는 데이터

```
{ field: { $nin: [ value1, value2, ... valueN ] } }
```

##### Logical

```
db.collection.find( { $and: [ { price: { $ne: 1.99 } }, { price: { $exists: true } } ] } )
db.collection.find( { price: { $not: { $gt: 1.99 } } } )
db.collection.find( { $nor: [ { price: 1.99 }, { sale: true } ]  } )
db.collection.find( { $or: [ { quantity: { $lt: 20 } }, { price: 10 } ] } )
```

##### Element

```
db.collection.find( { qty: { $exists: true, $nin: [ 5, 15 ] } } )
db.collection.find( { "zipCode" : { $type : 2 } } );
db.collection.find( { "zipCode" : { $type : "string" } } );
```

#### 2.3. find() 실습

```
> db.multi.find({})
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
```

- 전체 출력

```
> db.multi.find()
```

- class 가 DE 인 doc 만 출력

```
> db.multi.find({class: 'DE'})
```

- midterm 이 존재하는 doc 만 출력

```
> db.multi.find({midterm: {$exists: true}})
```

- kor 이 90점 이상인 doc 출력

```
> db.multi.find({kor: {$gte: 90}})
```

- midterm field 안에 있는 kor 이 50점 이상인 doc 출력

```
> db.multi.find({'midterm.kor': {$gte: 50}})
```

- kor 점수가 50보다 크고 90보다 작거나 같은 doc

```
> db.multi.find({$and: [{kor: {$gt: 50}}, {kor: {$lte: 90}}]})
```

- 아무런 조건 없이 name field 만 출력

```
> db.multi.find({}, {name: 1})
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd" }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss" }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선" }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진" }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원" }
```

- 아무런 조건 없이 name field 만 출력 (_id 미포함)

```
> db.multi.find({}, {name: 1, _id:0})
{ "name" : "hong-gd" }
{ "name" : "lee-ss" }
{ "name" : "류재선" }
{ "name" : "김세진" }
{ "name" : "윤지원" }
```

#### 2.4. sort() 실습

- 1: asc / -1: desc

- 이름 기준으로 오름차순 정렬

```
> db.multi.find().sort({name: 1})
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
```

- 이름 기준으로 내림차순 정렬

```
> db.multi.find().sort({name: -1})
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
```

- 국어점수 기준으로 오름차순 정렬 (국어점수 없는 애들부터 나열)

```
> db.multi.find().sort({kor: 1})
```

```
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "hong-gd", "class" : "DE", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
```

- 국어점수 기준으로 오름차순 정렬 (국어점수 있는 애들만 나열)

```
> db.multi.find({kor: {$exists: true}}).sort({kor: 1})
```

#### 2.5. limit() 실습

- 국어점수 상위 1개만 출력
  - cursor 메서드로 sort() 없어도 사용 가능

```
> db.multi.find({kor: {$exists: true}}).sort({kor: -1}).limit(1)
```

#### 2.6. skip() 실습

- 국어점수 하위 1개 스킵하고 나머지 출력
  - cursor 메서드로 sort() 없어도 사용 가능

```
> db.multi.find({kor: {$exists: true}}).sort({kor: -1}).skip(1)
```

#### 2.7. cursor

- find 의 결과를 cursor 에 저장 가능
  - hasNext(), forEach(), toArray(), ...
  - cursor 한 번 사용(출력)하면 소모됨

```
> var cursor = db.multi.find({kor: {$exists: true}})

> cursor.hasNext()
true

> cursor.next()
{
        "_id" : ObjectId("62cd856fbcf0ff805b72e04c"),
        "name" : "lee-ss",
        "class" : "DS",
        "kor" : 80,
        "eng" : 100,
        "math" : 100
}

> cursor
{ "_id" : ObjectId("62cd99f2bcf0ff805b72e04d"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }

> cursor.hasNext()
false
```



### 3. [Update] Update Documents

```
db.collection.updateOne( filter, 바꿀내용 )
db.collection.updateMany( filter, 바꿀내용 )
db.collection.replaceOne( filter, 대체할 문서 )
```

- `db.collection.updateOne( 조건, 바꿀내용 )`
  - 조건에 맞는 애 하나만 변경 (document의 field 수정)
- `db.collection.updateMany( 조건, 바꿀내용 )`
  - 조건에 맞는 애 전부 변경 (document의 field 수정)

```
> db.multi.updateOne({name: /hong/}, {$set: {name: '홍길동'}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

> db.multi.updateMany({midterm: {$exists: true}}, {$set: {class: 'graduated'}})
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "홍길동", "class" : "graduated", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "lee-ss", "class" : "DS", "kor" : 80, "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
```

- `db.collection.replaceOne( 조건, 대체할 문서 )`
  - 조건에 맞는 문서를 아예 대체 (document 수정)
  - 해당 id의 문서가 새로운 문서로 대치됨 (id는 그대로)


```
> db.multi.replaceOne({name: 'lee-ss'}, {name: '이순신'})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

> db.multi.find()
{ "_id" : ObjectId("62cd14c5c3c040c04e330447"), "name" : "홍길동", "class" : "graduated", "midterm" : { "kor" : 100, "eng" : 70, "math" : 60 } }
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "이순신" }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
```



### 4. [Delete] Delete Documents

```
db.collection.deleteOne( filter, options )
db.collection.deleteMany( filter, options )
```

- `db.collection.deleteOne( filter, options )`

```
> db.multi.deleteOne({name: {$exists: true}})
{ "acknowledged" : true, "deletedCount" : 1 }

> db.multi.find()
{ "_id" : ObjectId("62cd16b2c3c040c04e330448"), "name" : "이순신" }
{ "_id" : ObjectId("62cd1817c3c040c04e330449"), "name" : "류재선", "class" : "de", "kor" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044a"), "name" : "김세진", "eng" : 100, "math" : 100 }
{ "_id" : ObjectId("62cd1817c3c040c04e33044b"), "name" : "윤지원", "sci" : 100, "hobby" : "study" }
```

- `db.collection.deleteMany( 조건, options )`

```
> db.multi.deleteMany({name: {$exists: true}})
{ "acknowledged" : true, "deletedCount" : 4 }

> db.multi.find()
```

- `db.collection.deleteMany({})`
  - 전체 삭제


```
> db.inventory.deleteMany({})
{ "acknowledged" : true, "deletedCount" : 20 }
```



## 과제

```
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```

// 문제

item 필드의 값이 "notebook" 인 데이터의 size 를 가져와라

- db.inventory.find({item: "notebook"})

size 필드의 uom 이 cm 인 데이터의 item 이름을 가져와라(_id 포함)

- db.inventory.find({"size.uom": "cm"}, {item: 1})

item 필드의 값이 s 로 시작하는 데이터를 모두 가져와라

- db.inventory.find({"item": /^s/})

qyt 필드의 값이 90 미만이거나 size 필드의 uom 필드의 값이 in 인 데이터의 item 과 status 를 가져와라(_id 제외)

- db.inventory.find({$or: [{qty: {$lt: 90}}, {'size.uom': "in"}]}, {item: 1, status: 1, _id: 0})

size 필드의 w 필드의 값이 21 이상인 데이터를 모두 가져와라

- db.inventory.find({'size.w': {$gte: 21}})

status 필드의 값이 A 인 데이터를 qty 필드 기준으로 상위 2개만 가져와라

- db.inventory.find({status: 'A'}).sort({qty: -1}).limit(2)

status 필드의 값이 A와 D가 아닌 데이터 모두를 qty 필드를 10 으로 수정해라

- db.inventory.updateMany({status: {$nin: ["A", "D"]}}, {$set: {qty: 10}})

status 필드의 값이 A 인 상품의 데이터에 price 필드를 추가하고 값은 10000 으로 지정해라

- db.inventory.updateMany({status: "A"}, {$set: {price: 10000}})

price 필드가 존재하는 데이터를 size 필드의 h 필드 값 기준으로 내림차순 정렬하라

- db.inventory.find({price: {$exists: true}}).sort({'size.h': -1})

status 가 D 가 아니면서 qty 필드 값이 80 이상인 데이터 중 1번째 데이터를 삭제해라

- db.inventory.deleteOne({status: {$ne: "D"}, qty: {$gte: 80}})

status 가 D 가 아니면서 qty 필드 값이 80 이상인 데이터 중 2번째 데이터를 삭제해라??

- db.inventory.deleteOne({status: {$ne: "D"}, qty: {$gte: 80}}, {hint: '2'})



---

## Reference

- [Documents](https://www.mongodb.com/docs/manual/core/document/)
- [Insert documents](https://www.mongodb.com/docs/manual/tutorial/insert-documents/)
- [Query documents](https://www.mongodb.com/docs/manual/tutorial/query-documents/)
  - [regex-정규표현식](https://www.mongodb.com/docs/manual/reference/operator/query/regex/#mongodb-query-op.-regex)
  - [Query Filter Document](https://www.mongodb.com/docs/manual/core/document/#std-label-document-query-filter)
  - [Query Operators](https://www.mongodb.com/docs/manual/reference/operator/query/)
  - [Project Fields to Return from Query](https://www.mongodb.com/docs/manual/tutorial/project-fields-from-query-results/)
- [Update documents](https://www.mongodb.com/docs/manual/tutorial/update-documents/)
  - [Update Operators](https://www.mongodb.com/docs/manual/reference/operator/update/#std-label-update-operators)
- [Delete documents](https://www.mongodb.com/docs/manual/tutorial/remove-documents/)



# 배열 예제 한번 얘기해주세요
