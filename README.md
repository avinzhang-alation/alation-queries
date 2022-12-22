# Useful queries

* How BI reports are curated with no description
```
SELECT SUM(CASE WHEN description IS NULL THEN 1 ELSE 0 END) as no_description, count(*) as Total, 100 * SUM(CASE WHEN description IS NULL THEN 1 ELSE 0 END) / COUNT(*) AS Percentage FROM public.bi_report;
```


