# Week 2 Quiz

1.주문일이 2017-09-02 일에 해당 하는 주문건에 대해서,  어떤 고객이, 어떠한 상품에 대해서 얼마를 지불하여  상품을 구매했는지 확인해주세요.

```
select orders.customerid , orders.ordernumber , order_details.quotedprice 
from orders
left join order_details on orders.ordernumber = order_details.ordernumber 
where date(orders.orderdate) = date('2017-09-02') ;
```

2.헬멧을 주문한 적 없는 고객을 보여주세요.

- 헬맷은, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
```
select distinct customers.customerid, cust_helmet.customerid
from customers
left outer join (
		select distinct customers.customerid
		from customers
		left join orders on orders.customerid = customers.customerid 
		left join order_details on order_details.ordernumber = orders.ordernumber 
		right join products on products.productnumber = order_details.ordernumber 
		where products.productname like '%Helmet'
	 ) as cust_helmet
on customers.customerid = cust_helmet.customerid
where cust_helmet.customerid is null;
```

3.모든 제품 과 주문 일자를 나열하세요. (주문되지 않은 제품도 포함해서 보여주세요.)

```
select products.productname, orders.orderdate
from products 
left outer join order_details on products.productnumber = order_details.productnumber 
left outer join orders on orders.ordernumber = order_details.ordernumber 
order by (products.productname, orders.orderdate);
```

4.캘리포니아 주와 캘리포니아 주가 아닌 STATS 로 구분하여 각 주문량을 알려주세요. (CASE문 사용)

```
select sum(order_details.quantityordered) as order_sum,
	case when customers.custstate = 'CA' then  'CA'
	else 'Others'
	end as STATS
from customers
left join orders on customers.customerid = orders.customerid 
left join order_details on order_details.ordernumber = orders.ordernumber 
group by stats ;
```


5.공급 업체 와 판매 제품 수를 나열하세요. 단 판매 제품수가 2개 이상인 곳만 보여주세요.

```
select product_vendors.vendorid, count(product_vendors.productnumber) as count_prod
from vendors
left join product_vendors on vendors.vendorid = product_vendors.vendorid
group by product_vendors.vendorid 
having count(product_vendors.productnumber) >= 2;
```


6. 가장 높은 주문 금액을 산 고객은 누구인가요?
- 주문일자별, 고객의 아이디별로, 주문번호, 주문 금액도 함께 알려주세요.

```
select orders.orderdate, orders.customerid , orders.ordernumber, sum(db.total_price) as sum_price
from orders
join (
	 select order_details.quotedprice * order_details.quantityordered as total_price,
	 		orders.customerid , orders.orderdate , orders.ordernumber 
	 from order_details 
	 join orders on order_details.ordernumber =orders.ordernumber 
 ) as db on orders.customerid = db.customerid
group by orders.orderdate, orders.customerid , orders.ordernumber
order by sum_price desc
limit 1;
```

7.주문일자별로, 주문 갯수와,  고객수를 알려주세요.

- ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때 -> 해당의 경우는 고객수는 1명으로 계산해야합니다.

```
select db.orderdate, sum(db.count_orders) as count_orders, count(db.customerid) as count_customer
from
	(
	select orders.orderdate , orders.customerid , count(orders.ordernumber) as count_orders
	from orders
	group by orders.orderdate , orders.customerid
	) as db
group by db.orderdate
order by db.orderdate;
```

8번 생략

9.타이어과 헬멧을 모두 산적이 있는 고객의 ID 를 알려주세요.

- 타이어와 헬멧에 대해서는 , Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```
select db.customerid, sum(db.helmet) as orders_helmet, sum(db.tire) as orders_tire
from 		
	(
	select orders.customerid, products.productname,
		case when products.productname like '%Helmet' then 1 else 0 end as helmet,
		case when products.productname like '%Tire' then 1 else 0 end as tire
	from orders
	join order_details on orders.ordernumber = order_details.ordernumber 
	join products on products.productnumber = order_details.productnumber
	) as db
group by db.customerid
having sum(db.helmet) > 0 and sum(db.tire) > 0
order by db.customerid;
```

10. 타이어는 샀지만, 헬멧을 사지 않은 고객의 ID 를 알려주세요. Except 조건을 사용하여, 풀이 해주세요.
- 타이어, 헬멧에 대해서는, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```
select distinct orders.customerid 
from orders
join order_details on orders.ordernumber = order_details.ordernumber 
join products on order_details.productnumber =products.productnumber 
where products.productname like '%Tire'
order by orders.customerid 

except

select orders.customerid
where products.productname like '%Helmet';
```

