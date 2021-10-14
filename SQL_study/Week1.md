# Week1 SQL Study

활용할 쿼리문

- select
컬럼을 선택할 때 쓰는 쿼리문으로 From과 같이 사용한다.

```
SELECT db.year, db.month, db.id
FROM database as db
```

- orderby
Sorting을 해주는 쿼리문 

```
SELECT *
FROM database as db
orderby db.artist
```

- select distinct
Unique 값들을 추출할 때 쓰인다.

```
SELECT DISTINCT month
FROM database.album
```

- where
파이썬의 조건문과 같은 기능을 가진다.

```
SELECT * 
FROM database
WHERE month = 1
```

- Limit
파이썬의 df.head() 와 같은 기능으로 데이터의 형태를 확인하기 위한 쿼리문

- Fetch

보통 orderby와 offset이 같이 쓰이며 offset에 설정된 시작점에서 Fetch에 설정된 번호가 나타나기까지 순차적으로 진행되게 하는 쿼리문
파이썬의 range()와 비슷한 듯 하다.

```
SELECT *
FROM table
ORDERBY column
OFFSET (starting point)
FETCH NEXT k(constant) ROWS ONLY;
```

- In

Logical Operator로 리스트안의 값들이 조건문 값의 속해있는지 확인할 수 있는 쿼리문

```
SELECT *
FROM table
WHERE rank in (1,2,3)
```

- Between
Logical Operator로 범위내에 있는 row들만 선택할 수 있게 해주는 쿼리문

```
SELECT * 
FROM table
WHERE year_rank Between 5 AND 10
```

- Like
Logical Operator로 파이썬의 정규식과 같은 기능을하는 쿼리문

```
SELECT *
FROM table
WHERE "group" LIKE 'Snoop%'
```

- Isnull
Logical Operator로 결측치가 포함된 row만 추출하게 하는 쿼리문

```
SELECT * FROM table
WHERE artist IS NULL
```



#관계형 데이터베이스란?

**Realational** Database

- 데이터 테이블간의 관계성을 이용하여 새로운 테이블을 정의할 수 있다.
- 중복되는 유저, 작가, 사용자 등을 처리하기 용이하다. 

# 문제풀이

* 문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.

```

```

* 문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.

* 문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요

* 문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?

* 문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?

* 문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.

* 문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.

* 문제8번) 고객의 id 가  50,100,150 ..등 50번의 배수에 해당하는 고객들에 대해서,
회원 가입 감사 이벤트를 진행하려고합니다.
고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서
확인해주세요.

* 문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.

* 문제10번)	city 테이블의 city 갯수는 몇개인가요?

* 문제11번)	영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.

* 문제12번)	고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.

* 문제13번)	Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.

* 문제14번)	rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년6월20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.

* 문제15번)	film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.

* 문제16번)	langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .

* 문제17번)	film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.

* 문제18번)	film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.

* 문제19번)	Actor 테이블을 이용하여,  이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.

* 문제20번)	Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , firstname, lastname 으로 컬럼명을 바꿔서 보여주세요
