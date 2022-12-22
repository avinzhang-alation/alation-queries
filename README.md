# Useful queries

* How BI reports are curated with no description
```
SELECT SUM(CASE WHEN description IS NULL THEN 1 ELSE 0 END) as no_description, count(*) as Total, 100 * SUM(CASE WHEN description IS NULL THEN 1 ELSE 0 END) / COUNT(*) AS Percentage FROM public.bi_report;
```

* BI reports that have no stewards 
```
SELECT 
  SUM(CASE WHEN steward is NULL THEN 1 ELSE 0 END)  as No_Steward
FROM 
    public.bi_report
WHERE 
    EXISTS( SELECT 
                1 
            FROM 
                information_schema.columns 
              WHERE  column_name = 'steward'
              AND    table_name = 'bi_report');
```
