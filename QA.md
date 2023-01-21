What are your risk areas? Identify and describe them.

- Duplication of data: 
Had multiple rows of excessive and redudant duplicate entries in allsesions table. Had to create an inner join with analytics and   sales_by_SKU table to understand the duplication and finnaly removed te unnesscary rows.

- Unconsistency:
Standardised the data in various columns like productSKU in sales_by_sku table where some data was different acronyms and Bold letters. Date in Raw_alanytics table had to converted and formated. Some values in Sentiment magnintude column in products table had to be converted to decimal points to align with given other values. 

-Incomplete and Missing Data:
Columns like sentiment score in products table, City and Country in allsessions table and many more columns had null values which had to be replaced or filled appropriately as per other values in rows.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Unconsitency of data had to sorted out with certain queries wherein conversion of data and formats were changed like:
          select *, TO_DATE("date",'YYYYMMDD') from raw_analytics
          SELECT TRIM (leading "productsku") from salesbysku1
          select CAST ("sentimentmagnitude" as float) from products
          
VALIDATION:   select "date" from raw_analytics where "date" = 2017

Missing Data was filled with the help of averages of numerical columns and null values were deleted to maintained accuracy.

          DELETE FROM
		             allsessions
                 WHERE city = 'not available in demo dataset'
          
          Select FROM
                 allsessions
                 WHERE Country = '(not set)'
                 
          select sr.stocklevel, pp.stocklevel
                 from salesreport sr
                 left join products pp
                 on sr.stocklevel = pp.stocklevel
                 where sr.stocklevel != 0 and pp.stocklevel = 0
                 
VALIDATION:      select Count(stocklevel) from salesreport
                        where stocklevel = 0                 

Duplication of data values were removed so that accuracy can be maintained:
              
          DELETE FROM
               salesbysku1 ss
               USING salesbysku1 sk
               WHERE ss.productsku < sk.productsku;          
              
              
          DELETE FROM
	             allsessions
               WHERE fullvisitorid in
	 	          (SELECT fullvisitorid FROM
		          (SELECT fullvisitorid,
		          ROW_NUMBER() OVER( PARTITION BY visitid
	            ORDER BY fullvisitorid ) AS row_num
	            FROM allsessions ) rn
	            WHERE rn.row_num > 1);

VALIDATION:   select count(city, country) from allsessions
                where city = (not set)
