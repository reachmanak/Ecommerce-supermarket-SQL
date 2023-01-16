What issues will you address by cleaning the data?

Duplicate values deletion:
     -Removing Duplicate values in multiple table will deduce wrong analysis and longer time to complete
Removing Irrelevant Data:
    -Removal of irrelevant data and unrequired data helps in reducing file size and quicker calculations
Column Conversions:
    -Type conversions of columns of wrong datatype based columns help in maintaining consistancy
Fix Structural Errors:
    - This helped in standardisation of data values
 

Queries:
Below, provide the SQL queries you used to clean your data.

Select * 
into raw_analytics1
from raw_analytics


ALTER TABLE allsessions DROP COLUMN "transactionId"
ALTER TABLE allsessions DROP COLUMN "transactionRevenue"
ALTER TABLE allsessions DROP COLUMN "itemRevenue"
ALTER TABLE allsessions DROP COLUMN "eCommerceAction_type"
ALTER TABLE allsessions DROP COLUMN "eCommerceAction_option"
ALTER TABLE allsessions DROP COLUMN "eCommerceAction_step"
ALTER TABLE allsessions DROP COLUMN "productPrice"
ALTER TABLE allsessions2 RENAME COLUMN "v2ProductNAme" to "productname"
ALTER TABLE allsessions2 RENAME COLUMN "v2ProductCategory" to "productcategory"
ALTER TABLE allsessions2 RENAME COLUMN "to_timestamp" to "Time"



DELETE FROM
	   allsessions
WHERE fullvisitorid in
	 	(SELECT fullvisitorid FROM
		(SELECT fullvisitorid,
		ROW_NUMBER() OVER( PARTITION BY visitid
	ORDER BY fullvisitorid ) AS row_num
	FROM allsessions ) rn
	WHERE rn.row_num > 1);
	
DELETE FROM
		allsessions
WHERE city = 'not available in demo dataset' or country = '(not set)'

select *, "productPrice"/1000000 as "Product-Price"
from allsessions


DELETE FROM
salesbysku1 ss
USING salesbysku1 sk
WHERE ss.productsku < sk.productsku;


DELETE FROM
raw_analytics1 ra
USING raw_analytics1 rw
WHERE ra.fullvisitorid = rw.fullvisitorid
