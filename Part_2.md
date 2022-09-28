Fetch Reward DA Coding Exercise : 
Part 2 :

Q1 :What are the top 5 brands by receipts scanned for most recent month? 
	- I design a new table for the rewardsReceiptItemList and use the receipt ID to join with Receipts Table and barcode to join the Brands Table 

--------------------------------------------------------------------------------------------------------------------------------------------------

WITH brand_cnt_table AS (
	SELECT 
		b.name , COUNT(DISTINCT r._id) AS cnt
	FROM
		Receipts r
	JOIN
		rewardsReceiptItemList rl ON r._id = rl.receipt_id
	JOIN 
		Brand b ON rl.barcode = b.barcode
	WHERE
		DATEDIFF() <= 30
	GROUP BY
		b.name
),
	rank_table AS (
	SELECT
		name, DENSE_RANK() OVER(ORDER BY cnt DESC) AS rk
	FROM 
		brand_cnt_table
)

SELECT
	name
FROM
	brand_cnt_table
WHERE  
	rk <= 5;         

# Since I use dense_rank above, there will be no gap for the tied values. And this may include more than 5 brands because of tie ranks.

