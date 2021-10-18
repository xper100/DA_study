# Week 1 Quiz

문제1번)  각 제품 가격을 5 % 줄이려면 어떻게 해야 할까요?

```
select productnumber, productname, retailprice * 0.95 as sale_price_5per
from products;
```

문제2번)  고객이 주문한 목록을 주문 일자로 내림차순 정렬해서 보여주세요.

```
select * from orders o 
order by orderdate desc;
```

문제3번)  employees 테이블을 이용하여, 705 아이디를 가진 직원의 , 이름, 성과  해당 직원의  태어난 해를 확인해주세요.

```
select EmpFirstName, EmpLastName, extract (year from empbirthdate) as birth_year
from employees
where employeeid = 705;
```

문제4번)  customers 테이블을 이용하여,  고객의 이름과 성을 하나의 컬럼으로 전체 이름을 보여주세요 (이름, 성 의 형태로  full_name 이라는 이름으로 보여주세요)

```
select concat(CustFirstName, ', ', CustLastName) as full_name
from customers;
```

문제5번) orders 테이블을 활용하여, 고객번호가 1001 에 해당하는 사람이 employeeid 가 707인 직원으로부터  산 주문의 id 와 주문 날짜를 알려주세요.
* 주문일자 빠른순으로 정렬하여, 보여주세요.

```
select OrderNumber, OrderDate
from orders
where customerid =1001 and employeeid =707;
```

문제6번)  vendors 테이블을 이용하여, 벤더가 위치한 state 주가 어떻게 되는지, 확인해보세요.  중복된 주가 있다면, 중복제거 후에 알려주세요.

```
select distinct vendstate
from vendors;
```

문제7번) 주문일자가  2017-09-02~ 09-03일 사이에 해당하는 주문 번호를 알려주세요.

```
select ordernumber 
from orders
where date(orderdate) between date('2017-09-02') and date('2017-09-03') ;
```

문제8번) products 테이블을 활용하여, productdescription에 상품 상세 설명 값이 없는  상품 데이터를 모두 알려주세요.
- 풀이 1
```
from products
where productdescription is null or productdescription = ' ';
```

- 풀이 2
```
select *, coalesce(productdescription, 'Empty') as new_preduct
from products
where productdescription is null;
```


문제9 번) vendors 테이블을 이용하여, vendor의 State 지역이 NY 또는 WA 인 업체의 이름을 알려주세요.

```
select vendname
from vendors
where vendstate in ('NY', 'WA');
```

문제10번)  customers 테이블을 이용하여, 고객의 id 별로,  custstate 지역 중 WA 지역에 사는 사람과  WA 가 아닌 지역에 사는 사람을 구분해서  보여주세요.

- customerid 와, newstate_flag 컬럼으로 구성해주세요 .
- newstate_flag 컬럼은 WA 와 OTHERS 로 노출해주시면 됩니다.

```
select customerid, custstate,
	case 
		when custstate = 'WA' then 'WA'
		else 'OTHERS'
	end as newstate_flag
from customers;
```
