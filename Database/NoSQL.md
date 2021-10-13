# NoSQL

## NoSQL 의 등장 배경
* RDBMS 의 한계
   * 빅데이터의 등장
       * 데이터와 트래픽의 비약적인 증가
       * 많은 제약 조건으로 인하여 대량의 데이터를 신속하게 입력하기 어려움

## NoSQL - Not Only SQL
- RDB 형태의 관계형 데이터베이스가 아닌 다른 형태의 데이터 저장 기술을 사용.
- RDB 보다 유연한 스키마 - 대규모의 반정형 / 비정형 데이터를 저장하는데 적합
- RDBMS 를 대체하는 것이 아니라 필요에 따른 보완이 목적
- 여러대의 서버에 분산하여 저장하고 상호 복제
- 비정규화 허용
- 최종 일관성이 목표

## NoSQL 의 유형
1. Key - Value
   ![image](https://user-images.githubusercontent.com/62740151/137187850-a402ee87-2d22-40bf-b8a6-6a249b4c2364.png)
  - 특징
      - 높은 처리량
      - 짧은 지연 시간의 읽기 * 쓰기
      - 확장성
    - DBMS
    - Redis
    - Memcached
    - HazelCast
- Redis (Remote Dictionary Server)
    - 특징
        - In-memory 기반의 데이터 처리 및 저장을 제공
        - 빠른 속도
        - 서버가 꺼지면 모든 데이터가 사라진다
    - CRUD
        - C : set "key1" "value1" / mset "key2" "value2" "key3" "value3"
        - R : get("key1") / mget "key2" "key3"
        - U : set "key1", "value4"
        - D : del "key1"
2. Document
   ![image](https://user-images.githubusercontent.com/62740151/137188346-468a555a-3747-4057-9e9d-e7e8845f57f9.png)
- 특징
    - Object: 개발자에게 효율적이고 직관적인 데이터 모델
        - 보다 손쉽게 데이터를 저장하고 쿼리할 수 있다
        - join, sort 가 가능한 DBMS도 있음
    ![image](https://user-images.githubusercontent.com/62740151/137188487-87467a6d-bd3f-4432-818c-f06c74178c35.png)
- DBMS
    - MongoDB
    - AWS DynamoDB
    - Firebase Realtime Database
    - CouchDB
    - RavenDB
- MongoDB
    - 특징
        - Database > Collections > Documents 구조로 구성
        - Document 는 key - value 형태의 BSON(Binary JSON)으로 구성
        - MQL(MongoDB Query Language) 를 이용
    - MQL
        - 연산자
            - 비교 연산자
                - $eq : (equals) 주어진 값과 일치하는 값
                - $gt : (greater than) 주어진 값보다 큰 값
                - $gte : (greater than on equals) 주어진 값보다 크거나 같은 값
                - $lt : (less than) 주어진 값 보다 작은 값
                - $lte : (less tahn or equals ) 주어진 값보다 작거나 같은 값
                - $ne : (not equal) 주어진 값과 일치하지 않는 값
                - $in : 주어진 배열 안에 속하는 값
                - $nin : 주어진 배열 안에 속하지 않는 값
            - 논리 연산자
                - $or
                - $and
                - $not
                - $nor
        - CRUD
            - C : insert
                
                ```sql
                db.collection_name1.insert([
                
                {
                
                "key1": "value1-1"
                
                "key2": "value2-1"
                
                },
                
                {
                
                "key1": "value1-2"
                
                "key2": "value2-2"
                
                }
                
                ])
                ```
                
            - R : find(<query>, <projection>)
                - query : document 를 조회하는 조건
                - projection : document 를 조회할 대 보여질 field
                
                ```sql
                db.collection_name1.find().pretty() # collection의 모든 documnet 조회
                ```
                
                ```sql
                db.collection_name1.find({"key2":{$eq:"value2-1"}}).pretty() # key2 의 value가 value 2-1인 것 만 조회
                db.collection_name1.find({}, {"key1":true, "key2":false}).pretty() # key1 만 조회
                ```
                
            - U : update(<query>, <update>, {<upsert>, <multi>, <wrtieConcern>})
                - **update** : document 에 적용할 변동 사항
                - upsert : (boolean) true - query 한 document 가 없을 경우, insert
                    
                    기본값 false
                    
                - multi : (boolean) true - 여러개의 document 를 수정
                    
                    기본값 false
                    
                - writeConcern - document 를 update 할 때의 설정값 지정
                
                ```sql
                # key2 의 value 가 value2-1인 document 의 key1 의 value 값을 value 1-3으로 변경
                db.collection_name1.update({"key2":{$eq:"value2-1"}, {$**set**:{"key1":"value1-3"}})
                
                # document 가 존재하지 않는다면 새로 추가
                db.collection_name.update({"key1":"value1-3"}, {"key2":"value2-3"}, {upsert: true})
                
                ```
                
            - D : remove(criteria, justOne)
                - criteria : 삭제할 데이터의 기준 값. {} 인 경우 컬렉션의 모든 데이터를 제거
                - justOne : boolean - true : 1개의 document 만 제거
                    
                    기본값 : false
                    
                
                ```sql
                # Collection_name1 에서 key1 의 값이 value1-3인 document 제거
                db.collection_name1.remove({"key1":"value1-3"})
                ```
3. Graph
  - 특징
    - **node** 간의 관계를 저장
    - node 의 이름은 **label**
    - node의 **속성**은 **name-value** 쌍 들이 저장
    ![image](https://user-images.githubusercontent.com/62740151/137188671-c8e41f63-94bd-40b9-ad76-663f742b7bb4.png)
- DBMS
    - Neo4j
    - OrientDB
    - AWS Neptune
4. Column Family
  ![image](https://user-images.githubusercontent.com/62740151/137188722-29f64702-ac61-4ff8-a3f7-1f48ceae4682.png)
  - 특징
    - **Row key**에 의해 처리
    - 두 단계의 집합(MAP) 구조. Row Key에 다수의 column-value가 저장
    - column명은 columns이라는 속성의 데이터로 저장된다
    - RDB와 다르게 컬럼의 정의가 자유롭다.
    - 컬럼이 고정되지 않음 (컬럼에 해당하는 데이터가 존재하지 않으면 데이터를 입력하지 않아도 된다)
- DBMS
    - Cassandra
    - Apache HBase
