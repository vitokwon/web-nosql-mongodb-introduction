# 섹션 25, NoSQL & 몽고 DB 

## 모듈 요약
 - NoSQL 데이터베이스가 어떻게 작동하는지
 - 몽고 DB, NoSQL 데이터베이스 시스템
 - NoSQL CRUD 작업

## NoSQL 데이터베이스 시스템에 대한 이해
 - 스키마나 데이터구조 집중하지 않고 데이터 저장
 - 테이블의 구조를 정의할 필요 없음
 - '컬렉션(데이터 컨테이너)'으로 작업.
   - Collection1
    - Document1{"id":"abc","name":"Max"}
    - Document2{"id":"cde","title":"Book"}
   - Collection2
    - Document1{...}
    - Document2{...}
 - 자바스크립트 객체 구조와 비슷하지만, 데이터베이스 엔진(시스템)
 - 자바스크립트(프로그래밍언어), 프로그램이 실행되는 동안에만 메모리에서 관리.
 - NoSQL(데이터베이스), 파일에 저장되어 지속. (key:value)
 - 동일한 컬렉션에 서로 다른 구조 문서가 저장될 수 있음 (유연성 제공)
 - 중첩된 객체 존재
```JavaScript
Books{
"id":"abc","title":"Hary Potter",
"author":{"name":"J.K. Rowling"}, // 다른 문서 내부의 객체
"movies":["m1","m2"]
}
Movies{
 "id":"m1",
 "title":"Harry Potter 1",
 "director":{...}
}
{...}
```
 - 실행할 쿼리의 종류에 관해 생각하는 것이 중요, 데이터로 무엇을 할지,
 - 효율적인 쿼리 필요 (자주 함께 쿼리되는 데이터는 함께 저장)
 - NoSQL 데이터베이스 구성이 편리하지만 복잡해질 수 있음

## 몽고DB 설치 및 일반 설정
 1) 몽고DB 다운로드
   - Community Server(free version)
   - Enterpirse Server(기업용 추가 구성, 보안 기능)
   - Compass(GUI)
 2) 몽고DB Docs - Server
   - 일반적인 몽고DB 기능 조회
 3) 몽고DB 설치
   - Community server, windows, msi package 다운로드
   - installer 실행
    - custom installation - 'Server', 'Client'
    - Data Directory, Log Directory 기본 경로
    - Install MongoDB Compass 체크
    - '윈도우 시작' - 'Service' - 'MongoDB' 백그라운드 실행 상태 확인
    - 'command prompt' - 'net stop MongoDB' or ' net start MongoDB'
    - 'MongoDB 설치 경로' - 'bin 폴더' - 'mongo.exe' - 셸 실행 및 서버 연결
    - 'show dbs' - 데이터베이스 확인

 4) 몽고DB 셸 설치
   - 'mongodb shell' 구글검색 - 최신 셸 버전 설치 ('mongosh.exe' 최신버전)
   - 'MongoDB Shell Download | MongoDB'로 이동 - '다운로드'
   - '기본경로'로 설치 - '윈도우 검색 mongosh 실행

 5) 몽고DB 데이터 삽입
   - 'mongosh' - 'show dbs'
   - 사전 정의된 고정된 데이터 유형 구조 설정은 필요 없음
   - 'use ratingportal' 새로운 데이터 베이스 생성 및 전환
   - 'db.restaurants.insertOne({ name: "Munich Schnitzelhouse", address: { street: "Some Street 5", streetNumber: "23b" } })'
   - 삽입되는 모든 문서에 대해 고유한 ID를 자동 생성
   - 'db.restaurants.insertOne({ name: "Burger House", address: { street: "Another Street", streetNumber: "15" } })'

 6) 몽고DB 데이터 읽기 & 필터링
   - 'db.restaurants.find()' 모든 데이터를 조회
   - 'db.restaurants.find({ name: "Munich Schnitzelhouse"  }) 해당하는 값만 조회
   - 'db.restaurants.find({}, {name: 1})' id, name 필드만 조회
   - 'db.restaurants.find({}, {name: 1, _id: 0})' name 필드만 조회
   - '첫번쨰 필드{}'는 조건 정의, '두번째 필드{}'는 표시할 필드값.
   - 'db.restaurants.findOne({ name: "Munich Schnitzelhouse"  })' 조건을 충족하는 첫번째 일치 문서 조회

 7) 몽고DB 문서 업데이트
   - 'db.restaurants.updateOne({_id: ObjectId("...")}, {$set: { "address.street": "Some Street" }})'
   - 'db.restaurants.find() 업데이트 확인

 8) 몽고DB 문서 삭제
   - 'db.restaurants.deleteOne({_id: ObjectId("...")}) 해당 id 데이터 삭제
   - 'db.restaurants.find() 삭제 확인

 9) 완전한 데이터베이스 디자인/레이아웃 계획
   - SQL : 4개의 테이블, 각 테이블 ID, 관계 형성
   - NoSQL : 실행하려는 쿼리 계획

 10) 계획된 디자인 & 레이아웃 구현
   - 'db.restaurants.deleteMnay({}) 모든 문서를 삭제
   - 'db.types.insertOne({ name : "German" })
   - 'db.types.insertOne({ name : "Italian" })
   - 'db.types.insertOne({ name : "American" })
   - 'db.types.find()' types DB 조회
   - 'db.restaurants.insertOne({ name: "Munich Schnitzelhouse", address: { street: "Foodstreet", streetNumber: "23b", postalCode: 80333, city: "Munich", country: "Germany"}, type: {typeId: ObjectId("..."), name: "German"}})
   - 'db.restaurants.insertOne({ name: "Barlin Bugerhouse", address: { street: "Hamstreet", streetNumber: "12", postalCode: 80333, city: "Barlin", country: "Germany"}, type: {typeId: ObjectId("..."), name: "American"}})
   - 'db.restaurants.find()' restaurants DB 조회
   - 'db.restaurants.findOne({})
   - 'db.reviews.insertOne({ reviewer: { fistName: "Max", lastName: "kwon" }, rating: 3, text: "This was okay - could be better!", date: new Data("2021-09-10"), restaurant: { id: ObjectId("..."), name: "Munich Schnitzelhouse" }})
   - 'mongodatabase data' 구글검색 - 공식문서에서 사용법 확인 (JavaScript와 유사함)
   - db.reviews.find() reviews DB 조회

 11) 기타 필터링 연산자
   - 'db.reviews.insertOne({ reviewer: { fistName: "Max", lastName: "kwon" }, rating: 5, text: "This was amazing!", date: new Data("2021-09-10"), restaurant: { id: ObjectId("..."), name: "Barlin Burgerhouse" }})
   - 'db.reviews.find({rating:5})'
   - 'db.reviews.find({rating: {$gt: 4 }})' >4
   - 'db.reviews.find({rating: {$gt} })' >
   - 'db.reviews.find({rating: {$lt} })' <
   - 'db.reviews.find({rating: {$gte} })' >=
   - 'db.reviews.find({rating: {$lte} })' <=
   - 'db.reviews.find({$and: [{ rating: {$gt: 1}}, {rating: {$lt: 3}}] })' >1 and 3<
   - 'db.restaurants.find() 데이터 조회
   - 'db.restaurants.updateOne({_id: ObjectId("...")}, {$set: {"address.street": "Schnitzelstreet"}}) 업데이트
   - 'db.restaurants.find()
   - 'db.reviews.find()
   - 'db.reviews.deleteOne({_id: ObjectId("...")})