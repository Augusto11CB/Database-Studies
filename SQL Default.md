# SQL Default

### SQL COUNT Rows Result in separate columns
selects in the `select` clause and don't need a `from`.
```sql 

SELECT (SELECT COUNT(1)
        FROM  (SELECT DISTINCT DPOINT, RNUM 
               FROM ECOUNT
               WHERE DTYPE = 'STR' 
                 AND MONTH(EDATE) = '07') AS X
       ) AS ROWS1,
      (SELECT COUNT(1)
       FROM  (SELECT DISTINCT DPOINT, RNUM 
              FROM ECOUNT  
              WHERE DTYPE = 'NCD' 
                AND MONTH(EDATE) = '07') AS X
      ) AS ROWS2;
```

