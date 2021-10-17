# 1. SELECT
```sql
SELECT [PREDICATE] [테이블명] 속성명 [AS 별칭] [, [테이블명] 속성명, ...]
[, 그룹함수 (속성명) [AS 별칭]]
[, WINDOW 함수 OVER (PARTITION BY 속성명1, 속성명2, ...
                    ORDER BY 속성명3, 속성명4, ...)]
FROM 테이블명[, 테이블명,...]
[WHERE 조건]
[GROUP BY 속성명, 속성명,...]
[HAVING 조건]
[ORDER BY 속성명 [ASC|DESC]];
```
## SELECT 절
   ### PREDICATE : 불러올 튜플 수를 제한할 명령어를 기술
      - ALL : 모든 튜플을 검색할 때 지정하는 것으로, 주로 생략한다.
      - DISTINCT : 중복된 튜플이 있으면 그 중 첫 번째 한 개만 검색한다.
      - DISTINCTROW : 중복된 튜플을 제거하고 한 개만 검색하지만 선택된 속성의 값이 아닌, 튜플 전체를 대상으로 한다.
   ### AS : 속성 및 연산의 이름을 다른 제목으로 표시하기 위해 사용된다.
   ### 그룹함수 : GROUP BY 절에 지정된 그룹별로 속성의 값을 집계할 함수를 기술한다.
      - COUNT(속성명) : 그룹별 튜플 수를 구하는 함수
      - SUM(속성명) : 그룹별 합계를 구하는 함수
      - AVG(속성명) : 그룹별 평균을 구하는 함수
      - MAX(속성명) : 그룹별 최대값을 구하는 함수
      - MIN(속성명) : 그룹별 최소값을 구하는 함수
      - STDDEV(속성명) : 그룹별 표준편차를 구하는 함수
      - VARIANCE(속성명) : 그룹별 분산을 구하는 함수
      - ROLLUP(속성명, 속성명, ...)
         - 인수로 주어진 속서을 대상으로 그룹별 쇼계를 구하는 함수.
         - 속성의 개수가 n개이면, n+1 레벨까지, 하위 레벨에서 상위 레벨 순으로 데이터가 집계된다.
      - CUBE(속성명, 속성명,...)
         - ROLLUP과 유사한 형태이나 CUBE는 인수로 주어진 속성을 대상으로 모든 조합의 그룹별 소계를 구한다.
         - 속성의 개수가 n개이면, 2^n레벨까지, 상위 레벨에서 하위 레벨 순으로 데이터가 집계된다.
        
   ### WINDOW 함수 : GROUP BY 절을 이용하지 않고 속성의 값을 집계할 함수를 기술한다.
      - PARTITION BY : WINDOW 함수가 적용될 범위로 사용할 속성을 지정한다.
      - ORDER BY : PARTITION 안에서 정렬 기준으로 사용할 속성을 지정한다.
      - ROW_NUMBER() : 윈도우벼로 각 레코드에 대한 일련 번호를 반환한다.
      - RANK() : 윈도우 별로 순위를 반환하며, 공동 순위를 반영한다.
      - DENSE_RANK(): 윈도우별로 순위를 반환하며, 공동 순위를 무시하고 순위를 부여한다.
## GROUP BY 절
- 특정 속성을 기준으로 그룹화하여 검색할 때 사용한다. 일반적으로 GROUP BY 절은 그룹함수와 같이 사용된다.
## HAVING 절
- GROUP BY 와 함께 사용되며, 그룹에 대한 조건을 지정한다.
## GROUP BY 의 성
# 2. JOIN
- JOIN은 2개의 테이블에 대해 연관된 튜플들을 결합하여, 하나의 새로운 릴레이션을 반환한다.
- JOIN은 크게 INNER JOIN 과 OUTER JOIN으로 구분된다.
- JOIN은 일반적으로 FROM 절에 기술하지만, 릴레이션이 사용되는 어느 곳에서나 사용할 수 있다.
## INNER JOIN 
### EQUI JOIN
- JOIN 대상 테이블에서 공통 속성을 기준으로 '='(equal) 비교에 의해 같은 값을 가지는 행을 연결하여 결과를 생성하는 JOIN 방법
```sql
SELECT ~~
FROM 테이블명1, 테이블명2, ...
WHERE 테이블명1.속성명 = 테이블명2.속성명
```
```sql
SELECT ~~
FROM 테이블명1
(INNER) JOIN 테이블명2 on 테이블명1.속성명 = 테이블명2.속성명
# 속성명은 동일하지 않아도 된다
```
### NON-EQUI JOIN
- JOIN 조건에 >,<, <>, >=, <= 연산자를 사용하는 JOIN 방법
```sql
SELECT~~~
FROM 테이블명1, 테이블명2, ...
WHERE 테이블명1.속성명 <=(비교연산자) 테이블명2.속성명;
```
```sql
SELECT ~~~
FROM 테이블명1
(INNER) JOIN 테이블명2 ON 테이블명1.속성명 <=(비교연산자) 테이블명2.속성명;
```
## OUTER JOIN
- 릴레이션에서 JOIN 조건에 만족하지 않은 튜플도 결과로 출력하기 위한 JOIN 방법이다
- LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN이 있다.
### LEFT OUTER JOIN
- INNER JOIN 의 결과를 구한 후, 우측 항 릴레이션의 어떤 튜플과도 맞지 않는 좌측 항의 릴레이션에 있는 튜플들에 NULL 값을 붙여서 INNER JOIN의 결과에 추가한다.
```sql
SELECT ~~~
FROM 테이블명1
LEFT (OUTER) JOIN 테이블명 2
ON 테이블명1.속성명 = 테이블2.속성명 (순서 상관 X);
```
```sql
SELECT ~~~
FROM 테이블명1, 테이블명2
WHERE 테이블명1.속성명 = 테이블명2.속성명(+);
```
### RIGHT OUTER JOIN
- INNER JOIN 의 결과를 구한 후, 좌측 항 릴레이션의 어떤 튜플과도 맞지 않는 우측 항의 릴레이션에 있는 튜플들에 NULL 값을 붙여서 INNER JOIN의 결과에 추가한다.
```sql
SELECT ~~~
FROM 테이블명1
RIGHT (OUTER) JOIN 테이블명 2
ON 테이블명1.속성명 = 테이블2.속성명 (순서 상관 X);
```
```sql
SELECT ~~~
FROM 테이블명1, 테이블명2
WHERE 테이블명1.속성명(+) = 테이블명2.속성명;
```
### FULL OUTER JOIN
- LEFT OUTER JOIN 과 RIGHT OUTER JOIN을 합쳐 놓은 것이다.
```sql
SELECT ~~
FROM 테이블명1
FULL OUTER JOIN 테이블명2
ON 테이블명1.속성명 = 테이블명2.속성명;
```
### SELF JOIN
- 같은 테이블에서 2개의 속성을 연결하여 EQUI JOIN을 하는 JOIN 형식이다
```sql
SELECT ~~
FROM 테이블명1 [AS] 별칭1 
JOIN 테이블명1 [AS] 별칭2
ON 별칭1.속성명 = 별칭2.속성명;
```
```sql
SELECT ~~
FROM 테이블명1 [AS] 별칭1, 테이블명1 [AS] 별칭2
WHERE 별칭1.속성명 = 별칭2.속성명;
```
## JOIN 의 성능
# SUB QUERY
## INLINE VIEW
## SCALAR SUB QUERY
## SUB QUERY
## MULTI COLUMN SUB QUERY
