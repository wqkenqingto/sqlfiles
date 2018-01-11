# 相关业务sql的暂存
我们现在总注册人数帮忙统计下 &nbsp; 还有区分下渠道，小黑卡XXX，51YOUXXX，游鱼XXX，通信XXX，HiAppXXX

```sql
select regist_channel,count(1) as a from user_base_info where create_time>='2017-09-30 00:00:00'  and user_status=1 group by  regist_channel order by a desc;
```
截止到现在的 所有下单支付订单的会员用户数(排重) ，平均下单2次的会员用户数 ， 及下单3次以上的会员用户数？

```sql
select  count(1) a from ( select m.user_id,m.order_no from order_main ,order_pay_info p on m.order_no=p.order_no and p.pay_type =1 or 2 and p.pay_status=1 )tt

select count(*) from ( select count(*) a from order_main m  join order_pay_info p on m.order_no=p.order_no  and p.pay_status=1 and (p.pay_type =1 or p.pay_type=2) group by m.user_id having a >0)tt
select m.user_id,count(*) a from order_main m  join order_pay_info p on m.order_no=p.order_no  and p.pay_status=1 and (p.pay_type =1 or p.pay_type=2) group by m.user_id having a =2

select h.* from order_hotel_expand h join order_main m  on m.order_no =h.order_no  where rate_plan_pay_method=1 or rate_plan_pay_method=3  and   '2016-11-21 00:00:00'  < order_time < '2016-12-26 23:59:59'
```
机票业务

```sql
select  main.order_no,main.create_time,info.pay_time,main.depart_city,main.arr_city,main.depart_date
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
left join order_main_expand expand on detail.order_no=expand.order_no 
left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2))
left join order_mts_detail mts on main.order_no =mts.ref_order_no 
left join order_main as mts_main on mts.order_no =mts_main.order_no
left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) 
and main.order_type =4  group by order_no
```
相应注册渠道注册数统计
```sql
select regist_channel,count(1) as a from user_base_info where regist_channel is not null 
and (regist_channel like 'jp%' or regist_channel like '122%' or regist_channel like 'wjf%' or regist_channel like 'yt%') and create_time<='2017-12-27 00:00:00' and create_time>='2017-12-17 00:00:00'  group by regist_channel order by a desc;
```
注册总数

```sql
select count(distinct user_id) from user_login_record where create_time&gt;='2017-12-17 00:00:00' and create_time&lt;='2017-12-27 00:00:00'&nbsp; and device_type=5
```
麻烦帮导一下12月份12.01-28号 机票订单
```sql
select  main.order_no,main.create_time,info.pay_time,main.depart_city,main.arr_city,main.depart_date
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
left join order_main_expand expand on detail.order_no=expand.order_no 
left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2))
left join order_mts_detail mts on main.order_no =mts.ref_order_no 
left join order_main as mts_main on mts.order_no =mts_main.order_no
left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) 
and main.order_type =4  and main.create_time >='2017-12-01 00:00:00' and main.create_time <='2017-12-28 23:59:59'
group by order_no
```
各注册渠道注册数统计

```sql
select regist_channel ,count(1) from user_base_info  where create_time <'2017-9-30 00:00:00'   order  by  create_time ;

select regist_channel ,count(1) from user_base_info  where  regist_channel is  null  and  create_time >'2017-9-30 00:00:00' order by  create_time ;

select regist_channel,count(1) as a from user_base_info where  user_status=1 group by  regist_channel order by a desc;



select main.order_time,adult_num,child_num,total_price,order_no,main.depart_city_name,main.arr_city_name,detail.carrier_name 
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
left join order_main_expand expand on detail.order_no=expand.order_no 
left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2))
left join order_mts_detail mts on main.order_no =mts.ref_order_no 
left join order_main as mts_main on mts.order_no =mts_main.order_no
left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334)
and main.order_type =4 group by order_no;
```
各航司机票订单

```sql
select main.order_time,
,main.total_price,main.order_no,main.depart_city_name,main.arr_city_name,detail.carrier_name 
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
/* left join order_main_expand expand on detail.order_no=expand.order_no  */
/* left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2)) */
/* left join order_mts_detail mts on main.order_no =mts.ref_order_no  */
-- left join order_main as mts_main on mts.order_no =mts_main.order_no
-- left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334)
and main.order_type =4  and detail.carrier_name='西部航空' group by order_no;
```
海GO活动

```sql
select info.product_name, main.order_time,main.total_price ,main.order_no ,main.depart_city_name,main.arr_city_name ,main.depart_date from db_order.order_main_expand expand left join db_order.order_main main on main.order_no=expand.order_no 
left join db_ucenter.user_base_info inf on inf.user_id=main.user_id  
left join order_pay_info info on (main.order_no=info.order_no and info.pay_type in (1,2))
-- left join order_pay_info yi_info on (main.order_no=yi_info.order_no and yi_info.pay_type =3) 
where supplier_id=11  and user_type=0 and  main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) and main.create_time>='2017-12-01 00:00:00' and main.create_time<'2017-12-31 23:59:59' 
-- and inf.user_name='Hi客101006769';
```
机票各航司订单明细(万)
```sql
select  main.order_no,main.create_time,info.pay_time,main.depart_city_name,main.arr_city_name,main.code,detail.carrier_name,main.total_price, main.depart_date
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
-- left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
left join order_main_expand expand on detail.order_no=expand.order_no 
left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2))
-- left join order_mts_detail mts on main.order_no =mts.ref_order_no 
-- left join order_main as mts_main on mts.order_no =mts_main.order_no
-- left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) 
and main.order_type =4  and main.create_time >='2017-12-01 00:00:00' and main.create_time <='2017-12-31 23:59:59' group by main.order_no;
```
两位好：由于我部需要每周一、三上午统计APP上的机票销售数据，烦请两位根据附件内容提供所需的数据，万分感谢
```sql
select  main.order_no,main.create_time,flight.pay_time,main.depart_city_name,main.arr_city_name,main.code,detail.carrier_name, detail.adult_num+detail.child_num 出行人数,main.total_price, main.depart_date
from order_customer_info cus left join  order_flight_detail detail on cus.order_sub_no =detail.order_sub_no  
left join order_flight flight on detail.order_no=flight.order_no 
left join order_main main  on  detail.order_no=main.order_no 
left join order_main_expand expand on detail.order_no=expand.order_no 
left join order_pay_info info on (detail.order_no=info.order_no and info.pay_type in (1,2))
-- left join order_mts_detail mts on main.order_no =mts.ref_order_no 
-- left join order_main as mts_main on mts.order_no =mts_main.order_no
-- left join order_pay_info yi_info on (detail.order_no=yi_info.order_no and yi_info.pay_type =3)
-- left join order_flight_detail as other on (cus.order_sub_no =other.order_sub_no and other.order_source not in ('TUN','ABE'))
where 
-- main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) 
flight.pay_time is not null
-- and main.order_type =4 
 and  main.create_time>='2017-10-01 00:00:00' and main.create_time <='2017-12-31 23:59:59' group by order_no
 ```
 ```sql
 select info.product_name, main.order_time,main.total_price ,main.order_no ,main.depart_city_name,main.arr_city_name ,main.depart_date from db_order.order_main_expand expand left join db_order.order_main main on main.order_no=expand.order_no 
left join db_ucenter.user_base_info inf on inf.user_id=main.user_id  
left join order_pay_info info on (main.order_no=info.order_no )
-- left join order_pay_info yi_info on (main.order_no=yi_info.order_no and yi_info.pay_type =3) 
where supplier_id=11  and user_type=0 and  main.order_status in (194,195,404,405,412,413,414,420,302,300,305,306,307,308,309,313,310,311,312,314,315,333,334) and main.create_time>='2017-12-01 00:00:00' and main.create_time<'2017-12-31 23:59:59' and info.pay_type in (1,2)
-- and inf.user_name='Hi客101006769';
```