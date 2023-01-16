Question 1: Understanding sources of channel from where customers engage.

SQL Queries:

Select channelgrouping, count(channelgrouping)
from allsessions
group by channelgrouping

Answer: 
"Organic Search"	2550
"Display"	75
"Referral"	1797
"Affiliates"	101
"Paid Search"	187
"Direct"	1262
"(Other)"	2



Question 2: Time spent on site and for what product category

SQL Queries:

Select timeonsite, productcategory, Round(avg(timeonsite),1) as avg, sum(timeonsite)
from allsessions2
group by timeonsite,productcategory

Select productcategory, max(timeonsite), min(timeonsite)
from allsessions2
group by productcategory

Answer:

"Home/Brands/"	55	55
"Home/Accessories/Stickers/"	2366	8
"Home/Accessories/Sports & Fitness/"	1785	13
"Home/Accessories/Fun/"	1502	9
"Home/Apparel/Kid's/Kids-Youth/"	1223	9
"Home/Shop by Brand/"	1435	10
"Home/Drinkware/Mugs and Cups/"	1657	32
"Home/Apparel/Kid's/Kid's-Toddler/"	1588	29
"Home/Accessories/Pet/"	534	12
"Home/Brands/YouTube/"	2327	21




Question 3: 

Average product price per product category

SQL Queries:

select productcategory, round(avg("Product-Price"),1) as avgprice
from allsessions2
group by productcategory

Answer:
"Home/Brands/"	18.5
"Home/Accessories/Stickers/"	1.6
"Home/Accessories/Sports & Fitness/"	19.7
"Home/Accessories/Fun/"	5.9
"Home/Apparel/Kid's/Kids-Youth/"	20.3
"Home/Shop by Brand/"	22.2
"Home/Drinkware/Mugs and Cups/"	11.5
"Home/Apparel/Kid's/Kid's-Toddler/"	19.5
"Home/Accessories/Pet/"	3.9
"Home/Brands/YouTube/"	16.9
