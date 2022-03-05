(강의주소: https://www.youtube.com/watch?v=qw--VYLpxG4)

실행순서: FROM -> WHERE -> GROUP BY -> SELECT -> DISTINCT -> ORDER BY -> LIMIT  
SQL 파일 불러오기: \i [파일주소] (같은 테이블명으로 수정하고 다시 불러와도 덮어쓰기 안됨 삭제하고 새로 불러와야함) 
  
CREATE: CREATE TABLE [테이블명] ([컬럼명1] [데이터 종류] [NULL 여부] [PK 여부], [컬럼명2] ...);  
DROP: DROP TABLE [테이블명];  
INSERT: INSERT INTO [테이블명] ([컬럼명1], [컬럼명2], ...) VALUES ([컬럼에 들어갈 데이터 종류에 맞게1], ...), ([컬럼에 들어갈 데이터 종류에 맞게2], ...);  
SELECT: SELECT <DISTINCT> [컬럼명] FROM [테이블명] <WHERE [조건식]> <OFFSET [~까지 조회X]> <LIMIT [원하는 갯수]>  <ORDER BY [정렬 기준 컬럼] [ASC/ DESC]> ;  
조건식 반복될 때 ex. WHERE country IN ('China', 'Brazil', 'France')  
BETWEEN: 범위지정 ex. BETWEEN DATE '2000-01-01' AND '2015-01-01';  
Like: ex. WHERE email LIKE '________(글자 갯수)@%(임의의 문자열)';  
iLike: Like와 기능은 똑같고 대소문자를 구별하지 않음.  
Alias: as 붙이면 컬럼명 지정할 수 있음   
GroupBy: 지정한 컬럼의 데이터가 같은 레코드끼리 묶음 ex. SELECT country, COUNT(*) FROM person GROUP BY country  
Having: Group by 다음 Order by 이전에 사용해야함. 그룹화 후의 조건을 나타냄 ex. SELECT country, COUNT(*) FROM person GROUP BY country HAVING COUNT(*) > 100;  
COUNT/MAX/MIN/SUM/AVG: GROUP BY와 함께 쓰이는 집계함수, count(*)은 null을 카운트 count(column)은 X  
COALESCE: COALESCE(요소1, 요소2, ...) 요소 값 중 NULL이 아닌 첫 번째 값으로 반환함. ex. SELECT COALESCE(email, 'Email not provided') FROM person  
NULLIF: NULLIF(요소1, 요소2) 두 요소가 같으면 NULL, 같지 않다면 요소1을 반환. 0으로 나누는 에러 핸들링 가능 ex. SELECT COALESCE(10 / NULLIF(0, 0), 0)  
NOW: 현재 시간을 가져옴 ex. SELECT NOW(); or SELECT NOW()::DATE; or SELECT NOW()::TIME;  
INTERVAL: 시간 연산에 이용 ex. SELECT NOW() - INTERVAL '1 YEAR';  
EXTRACT: 날짜 유형의 데이터로부터 날짜 정보를 분리하여 새로운 컬럼의 형태로 추출해 주는 함수 SELECT EXTRACT([날짜요소] FROM [컬럼명]) FROM [테이블명]; ex. SELECT EXTRACT(CENTURY FROM NOW());  
AGE: 두 DATE 사이 간격을 알려줌 ex. SELECT AGE(NOW(), date_of_birth) AS age FROM person   
ALTER: 컬럼과 제약 조건을 변경, 추가 또는 삭제하거나 파티션을 재할당하거나 제약 조건과 트리거를 설정 또는 해제하여 테이블 정의를 수정 ex. ALTER TABLE person DROP CONSTRAINT person_pkey; 
PRIMARY KEY: 하나의 테이블에 있는 데이터들을 식별하기 위한 기준으로 인식되는 제약조건.  기본 키는 한 개의 테이블에 하나만 생성 가능하며, Null값이 있으면 안되고 중복되지 않는 유일한 값 이어야 한다.  
PK ADD: ex. ALTER TABLE person ADD PRIMARY KEY(id)  
DELETE:  ex. DELETE FROM person WHERE id = 1;  
CONSTRAINT: 테이블에 잘못된 데이터의 입력을 막기위해 일정한 규칙을 지정하는 것 ALTER TABLE person ADD CONTRAINT <제약조건 이름 지정> <UNIQUE ([컬럼명])> <CHECK (조건식)>  
CHECK: 특정 조건들로 이루어진 수식을 통해 입력되는 데이터를 검증할 때 사용하는 제약 조건  
NOT NULL: 데이터 입력시에 누락이 되지 않도록 하는 제약 조건  
UNIQUE:  해당 테이블에 있어서 존재하는 값이 유일해야 한다. 만약 데이터 입력/수정시에 중복되는 값이 있다면 에러가 발생한다.하지만, Null값은 데이터로 인식하지 않기 때문에 Null에 대해서는 UNIQUE가 적용되지 않는다  
UPDATE: 레코드의 값을 변경 ex. UPDATE [테이블명] SET [어떤 컬럼을 무슨 값으로 바꿀지] WHERE [조건식]  
ON CONFLICT: 레코드 삽입 시 특정 상황을 만날 때 처리 ex. INSERT INTO [table_name] ([column_list]) VALUES([value_list]) ON CONFLICT [컬럼명 or ON CONSTRAINT 제약조건명] [DO NOTHING or DO UPDATE SET [변경사항] WHERE [조건식]];  
(INSERT에서 사용한 값을 다시 사용하고 싶으면 EXCLUDED.[컬럼명]을 사용하면된다.)  
FOREIGON KEY: 두 테이블 사이의 관계를 선언함으로써, 데이터의 무결성을 보장해 주는 역할을 한다. 외래 키 관계를 설정하게 되면 외래 키 테이블이 다른 테이블(기준 테이블)에 의존하게 된다.   
외래 키는 '기준 테이블'을 참조하기 때문에, 반드시 '기준 테이블'에 존재하는 데이터만 입력이 가능하며, 참조하는 '기준 테이블'의 컬럼은 반드시 PK이거나 Unique이어야 한다.  
외래 키로 참조되고 있는 레코드는 삭제 불가, 참조하는 레코드를 먼저 지워야지 지울 수 있음  
CREATE TABLE [참조할 테이블] ( [참조할 컬럼] REFERENCES [[참조될 테이블](참조될 컬럼)])  
CREATE TABLE [참조될 테이블] ( [참조될 컬럼] NOT NULL PRIMARY KEY)  
INNER JOIN: 공통적인 부분만 SEELCT, SELECT [컬럼명1], ... FROM [테이블명] JOIN [다른 테이블] ON [결합기준]  
LEFT JOIN: 조인기준 왼쪽에 있는거 다 SELECT 됨, SELECT [컬럼명1], ... FROM [테이블명] LEFT JOIN [다른 테이블] ON [결합기준]  
RIGHT JOIN: 조인기준 오른쪽에 있는거 다 SELECT됨, SELECT [컬럼명1], ... FROM [테이블명] RIGHT JOIN [다른 테이블] ON [결합기준]  
조인할 두 테이블의 결합 기준이 같은 컬럼명을 가지고 있다면 ON[결합기준] 대신 USING([컬럼명])을 이용하면 중복제거해서 조인해줌 
[ON vs WHERE]
ON : JOIN 을 하기 전 필터링을 한다 (=ON 조건으로 필터링이 된 레코들간 JOIN이 이뤄진다)
WHERE : JOIN 을 한 후 필터링을 한다 (=JOIN을 한 결과에서 WHERE 조건절로 필터링이 이뤄진다)
Exporting to CSV: \COPY ([SELECT 레코드]) TO 'CSV 파일 주소' DELIMETER ',' CSV HEADER;  
Serial & Sequences: SELECT * FROM person_id_seq; 로 serial테이블 불러올 수 있음 SELECT nextval('person_id_seq'::regclass); 명령할 때마다 nextval이 1씩 올라가면서 insert 될 레코드의 id를 정해주는 값이 바뀜  
ALTER SEQUENCE person_id_seq RESTART WITH [숫자];로 재지정 가능  
UUID Data Type: 네트워크 상에서 고유성이 보장되는 id를 만들기 위한 표준 규약.   
SELECT * FROM pg_available_extensions; -> CREATE EXTENSION IF NOT EXISTS "uuid-ossp"; -> SELECT uuid_generate_v4();  
UUID As Primary Keys: 데이터 타입을 UUID로 PK컬럼생성 -> INSERT할 때 값에 uuid_generate_v4() 지정  
 
