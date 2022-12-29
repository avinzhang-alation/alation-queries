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

* Count users from each user group
```
select 
 AG.group_name,
 count(distinct U.user_id) AS count_of_users
 FROM users AS U
 left join user_group_membership AS G
 ON G.user_id = U.user_id
 left join alation_group as AG
 on AG.group_id = G.group_id
 where is_active = 'true'
 Group By AG.group_name;
```

* List of article custom fields being created
```
SELECT 
  a.title AS article_title,
  date(ch.ts_updated) AS article_updated,
  u.display_name,
  ch.field_name,
  a.article_url,
  ch.action,
  ch.value
FROM public.curation_history ch
LEFT JOIN public.article a 
ON a.article_id = ch.object_id
LEFT JOIN public.users u 
ON ch.user_id = u.user_id
WHERE ch.object_type = 'article'
ORDER BY ch.object_id ASC;
```

* BI objects
  ** bi-reports that have domain vs total 
  ```
  select count(*) as has_domain, (select count(*) from bi_report) as total from public.bi_report br inner join domain_members dm on br.bi_report_id = dm.object_id where dm.object_type = 'bi_report'
  ```
