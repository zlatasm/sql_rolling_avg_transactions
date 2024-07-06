```
-- Database: PostgreSQL v15

SELECT tavg.rolling_avg
FROM
	(SELECT 
        tsum.day,
        AVG(tsum.sum) OVER (ORDER BY tsum.day ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as rolling_avg
    FROM (
        SELECT SUM(t.transaction_amount), t.day 
        FROM (
            SELECT transaction_amount, EXTRACT(DAY FROM transaction_time) as day 
            FROM transactions ORDER BY day
        ) t GROUP BY t.day
    ) tsum
    ) tavg
WHERE tavg.day = 31
```
<img width="248" alt="image" src="https://github.com/zlatasm/sql_rolling_avg_transactions/assets/91090292/64549f1a-baa5-43c4-af34-3b90705c5ccd">
