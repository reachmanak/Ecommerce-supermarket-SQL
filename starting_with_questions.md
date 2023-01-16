Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries: 
SELECT city, country , max(revenue) as "MAX REVENUE"
FROM
	(SELECT city, country, ("total_ordered"* "Product-Price") as revenue
 		FROM (SELECT * FROM allsessions2 al
			 INNER JOIN salesbysku ss
			 ON al."productSKU" = ss."productsku"
			 WHERE type = 'PAGE') as s)
			 as subq
GROUP BY city, country
ORDER BY "MAX REVENUE" DESC
LIMIT 2



Answer:
"Chicago"	"United States"	    28012
"London"	"United Kingdom"	23406



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT city, country, productname,ROUND(AVG("total_ordered"),1) as "AVG Orders"
 		FROM (SELECT * FROM allsessions2 al
			 INNER JOIN salesbysku ss
			 ON al."productSKU" = ss."productsku"
			 WHERE type = 'PAGE') as s
GROUP BY city, country, productname
ORDER BY "AVG Orders" DESC



Answer:

"Seattle"	    "United States"	"Ballpoint LED Light Pen"	456.0
"Seoul"	        "South Korea"	"Ballpoint LED Light Pen"	456.0
"Mountain View"	"United States"	"Ballpoint LED Light Pen"	456.0
"San Francisco"	"United States"	"Ballpoint LED Light Pen"	456.0
"Santa Clara"	"United States"	"Ballpoint LED Light Pen"	456.0
"Palo Alto"	    "United States"	"Ballpoint LED Light Pen"	456.0
"Sunnyvale"	    "United States"	"Ballpoint LED Light Pen"	456.0
"Pune"	             "India"	"Ballpoint LED Light Pen"	456.0
"Boston"	    "United States"	"Ballpoint LED Light Pen"	456.0
"New York"	    "United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Dublin"	       "Ireland"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Sunnyvale"	    "United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Mountain View"	"United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Pune"	             "India"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Chicago"	    "United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Toronto"	        "Canada"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"London"	   "United Kingdom"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Shinjuku"	         "Japan"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"Santa Clara"	"United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0
"San Francisco"	"United States"	"Google 17oz Stainless Steel Sport Bottle"	334.0




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT city,country, max(productcategory) as "Product Category"
FROM 
	(SELECT *, ROW_NUMBER() OVER (PARTITION BY productcategory, country) as rn
							FROM allsessions2) s
WHERE rn = 1
GROUP BY city,country
ORDER BY country



Answer:

"Ahmedabad"	     "India"	"Home/Shop by Brand/Google/"
"Alexandria"	 "Egypt"	"Home/Apparel/Men's/Men's-T-Shirts/"
"Amã"	        "Jordan"	"Home/Bags/"
"Amsterdam"	  "Netherlands"	"Home/Shop by Brand/YouTube/"
"Ann Arbor"	"United States"	"Home/Shop by Brand/YouTube/"
"Antwerp"	   "Belgium"	"Home/Office/"
"Ashburn"	"United States"	"Home/Lifestyle/Fun/"
"Asuncion"	  "Paraguay"	"Home/Apparel/Men's/Men's-T-Shirts/"
"Athens"	    "Greece"	"Home/Shop by Brand/YouTube/"
"Atlanta"	"United States"	"Home/Bags/"





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT country, max(productcategory) as ProductCategory, max(productname) as Productname from
		(SELECT  country, productcategory, productname
				, ROW_NUMBER () OVER (Partition BY country, productcategory )as ab
		 FROM (SELECT * FROM allsessions2 al
			 		INNER JOIN salesbysku ss
			 		ON al."productSKU" = ss."productsku") s) s2
WHERE ab = 1 and productcategory like '%Home%'
GROUP BY country
ORDER BY country



Answer:

"Argentina"	  "Home/Electronics/Flashlights/"	    "SPF-15 Slim & Slender Lip Balm"
"Australia"	   "Home/Shop by Brand/YouTube/"	    "YouTube Men's Short Sleeve Hero Tee Black"
"Austria"	           "Home/Apparel/Men's/"	    "Google Men's 100% Cotton Short Sleeve Hero Tee Red"
"Belgium"	           "Home/Office/"	            "Windup Android"
"Brazil"	"Home/Shop by Brand/YouTube/"	        "YouTube Custom Decals"
"Canada"	     "Home/Shop by Brand/YouTube/"	    "YouTube Men's Vintage Tank"
"Chile"	               "Home/Office/"	            "Sport Bag"
"China"	   "Home/Apparel/Men's/Men's-Outerwear/"	"Google Men's Airflow 1/4 Zip Pullover Lapis"
"Colombia"	   "Home/Shop by Brand/YouTube/"	    "YouTube Men's Short Sleeve Hero Tee Black"
"Croatia"	"Home/Apparel/Men's/Men's-Outerwear/"   "Google Men's Watershed Full Zip Hoodie Grey"




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT country, max(productcategory) as ProductCategory,max(productname) as Productname , max(revenue) as Revenue from
		(SELECT  country, productname, productcategory, ("total_ordered"* "Product-Price") as revenue
				, ROW_NUMBER () OVER (Partition BY country )as ab
		 FROM (SELECT * FROM allsessions2 al
			 		INNER JOIN salesbysku ss
			 		ON al."productSKU" = ss."productsku") s) s2
WHERE ab = 1 and ProductCategory LIKE '%Home%'
GROUP BY country
ORDER BY Revenue desc
LIMIT 10


Answer:

"Portugal"	    "Home/Apparel/Headgear/"	      "Google Blackout Cap"	                       3402
"Saudi Arabia"	    "Home/Office/"	              "Leatherette Journal"	                       3190
"Thailand"	      "Home/Drinkware/"	              "26 oz Double Wall Insulated Bottle"	       2328
"Ukraine"	 "Home/Office/Writing Instruments/"	  "Metal Texture Roller Pen"	               1344
"India"	     "Home/Shop by Brand/YouTube/"	      "YouTube Hard Cover Journal"	               1190
"Taiwan"	   "Home/Shop by Brand/YouTube/"	  "YouTube Hard Cover Journal"	               1190
"Romania"	          "Home/Bags/"	              "Google Canvas Tote Natural/Navy"	            930
"Croatia"  "Home/Apparel/Men's/Men's-Outerwear/"  "Google Men's Watershed Full Zip Hoodie Grey"	327
"Colombia"	         "Home/Apparel/"	          "Google Men's Watershed Full Zip Hoodie Grey"	327
"Singapore"	     "Home/Bags/Backpacks/"	          "Google Alpine Style Backpack"	            297







