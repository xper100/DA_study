# Week1 SQL Study

## 활용할 쿼리문

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

```
SELECT * FROM table
LIMIT 100
```

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

## Part 1

* 문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.

```
SELECT DISTINCT date(rental_date) 
from rental
order by date(rental_date);
```

* 문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.


```
select title
from film 
where rental_duration >= 4 and length >= 120; 
```

* 문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요

```
select staff_id, first_name, last_name
from staff 
where staff_id = 2;
```

* 문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?

```
select amount
from payment 
where payment_id = 17510;
```

* 문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?

```
select category_id
from category ct 
where ct.name = 'Sci-Fi';
```


* 문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.

```
select count(distinct rating) as unique_rating_count
from film;
```

* 문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.

```
select * from rental 
where date(return_date) - date(rental_date) + 1 >= 10; # 대여일자부터 포함이니 +1
```


* 문제8번) 고객의 id 가  50,100,150 ..등 50번의 배수에 해당하는 고객들에 대해서,
회원 가입 감사 이벤트를 진행하려고합니다.
고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서
확인해주세요.

```
select customer_id, first_name, last_name, email
from customer 
where mod(customer_id, 50) = 0; # 50으로 나누었을 때 나머지가 0인 경우
```

* 문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.

```
select title
from film
where length(title) = 8
order by title;
```

* 문제10번)	city 테이블의 city 갯수는 몇개인가요?

```
select count(distinct city) as the_number_of_unique_city
from city;
```

* 문제11번)	영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.

```
select distinct Upper( concat(first_name, ' ', last_name) ) as full_name
from actor
order by full_name;
```

* 문제12번)	고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.

```
select count(distinct customer_id) as count_inactive
from customer 
where active = 0;
```

* 문제13번)	Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.

```
select count(distinct customer_id) as count_customer_store
from customer 
where store_id =1;
```

* 문제14번)	rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년6월20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.

```
select count(rental_id)
from rental
where date(return_date) = date('2005-06-20');
```

* 문제15번)	film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.

```
select *
from film 
where (release_year = 2006) and (rating = 'G') and rental_duration = 3;
```

* 문제16번)	langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .

```
select language_id, name
from language;
```

* 문제17번)	film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.

```
select film_id, title, description
from film
where rental_duration >= 7
order by film_id ;
```

* 문제18번)	film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.

```
select film_id, title, description
from film
where rental_duration between 3 and 5
order by title;
```

* 문제19번)	Actor 테이블을 이용하여,  이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.

```
select actor_id, first_name, last_name
from actor 
where first_name ='Nick' or last_name ='Hunt';
```

* 문제20번)	Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , firstname, lastname 으로 컬럼명을 바꿔서 보여주세요

```
select first_name as firstname, last_name as lastname
from actor;
```


# 처음 알게된 것
- MOD(dividend, divider) => 파이썬의 '%' operation과 같은 것
- concat(a,b,c) as new_colname => 컬럼을 함쳐서 새로운 컬럼으로 재생산


## Part 2

문제1번) film 테이블을 활용하여,  film 테이블의  100개의 row 만 확인해보세요.

```
select * 
from film
limit 100;
```

문제2번) actor 의 성(last_name) 이  Jo 로 시작하는 사람의 id 값이 가장 낮은 사람 한사람에 대하여, 사람의  id 값과  이름, 성 을 알려주세요.

```
select actor_id, first_name, last_name
from actor
where last_name like 'Jo%'
order by actor_id  
offset 0
fetch next 1 rows only;
```

문제3번)film 테이블을 이용하여, film 테이블의 아이디값이 1~10 사이에 있는 모든 컬럼을 확인해주세요.

```
select *
from film
where film_id between 1 and 10;
```

문제4번) country 테이블을 이용하여, country 이름이 A 로 시작하는 country 를 확인해주세요.

```
select country
from country
where country like 'A%';
```

문제5번) country 테이블을 이용하여, country 이름이 s 로 끝나는 country 를 확인해주세요.

```
select country
from country
where country like '%s';
```

문제6번) address 테이블을 이용하여, 우편번호(postal_code) 값이 77로 시작하는  주소에 대하여, address_id, address, district ,postal_code  컬럼을 확인해주세요.

```
select address_id, address, district, postal_code
from address
where postal_code like '77%';
```

문제7번) address 테이블을 이용하여, 우편번호(postal_code) 값이  두번째글자가 1인 우편번호의  address_id, address, district ,postal_code  컬럼을 확인해주세요.

```
select address_id, address, district, postal_code
from address
where postal_code like '_1%';
```

문제8번) payment 테이블을 이용하여,  고객번호가 341에 해당 하는 사람이 결제를 2007년 2월 15~16일 사이에 한 모든 결제내역을 확인해주세요.

```
select *
from payment
where (customer_id = 341) and date(payment_date) in (date('2007-02-15'), date('2007-02-16')) ;
```

문제9번) payment 테이블을 이용하여, 고객번호가 355에 해당 하는 사람의 결제 금액이 1~3원 사이에 해당하는 모든 결제 내역을 확인해주세요.

```
select *
from payment
where customer_id = 355 and amount between 1 and 3;
```

문제10번) customer 테이블을 이용하여, 고객의 이름이 Maria, Lisa, Mike 에 해당하는 사람의 id, 이름, 성을 확인해주세요.

```
select customer_id, first_name, last_name
from customer
where first_name in ('Maria', 'Lisa', 'Mike');
```

문제11번) film 테이블을 이용하여,  film의 길이가  100~120 에 해당하거나 또는 rental 대여기간이 3~5일에 해당하는 film 의 모든 정보를 확인해주세요.

```
select *
from film
where length between 100 and 120 or rental_duration between 3 and 5
order by title;
```

문제12번) address 테이블을 이용하여, postal_code 값이  공백('') 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.

```
select *
from address
where postal_code in ('', '35200', '17886');
```

문제13번) address 테이블을 이용하여,  address 의 상세주소(=address2) 값이  존재하지 않는 모든 데이터를 확인하여 주세요.

```
select * 
from address
where address2 is null;
```

문제14번) staff 테이블을 이용하여, staff 의  picture  사진의 값이 있는  직원의  id, 이름,성을 확인해주세요.  단 이름과 성을  하나의 컬럼으로 이름, 성의형태로  새로운 컬럼 name 컬럼으로 도출해주세요.

```
select staff_id, concat(first_name, ', ', last_name) as full_name
from staff
where picture notnull; 
```

문제15번) rental 테이블을 이용하여,  대여는했으나 아직 반납 기록이 없는 대여건의 모든 정보를 확인해주세요.

```
select * 
from rental
where return_date is null;
```

문제16번) address 테이블을 이용하여, postal_code 값이  빈 값(NULL) 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.

```
select *
from address
where postal_code in ('', '35200', '17886');
```

문제17번) 고객의 성에 John 이라는 단어가 들어가는, 고객의 이름과 성을 모두 찾아주세요.

```
select first_name, last_name
from customer
where last_name like 'John%';
```

문제18번) 주소 테이블에서, address2 값이 null 값인 row 전체를 확인해볼까요?

```
select *
from address
where address2 is null;
```
