---
title: GROUP_CONCAT(MySQL)
description: GROUP_CONCAT(MySQL)
categories:
 - MySQL
tags: MySQL
---

## SQL 명세
```sql
SELECT col1, col2, ..., colN
GROUP_CONCAT ( [DISTINCT] col_name1 
[ORDER BY clause]  [SEPARATOR str_val] ) 
FROM table_name GROUP BY col_name2;
```


### 특정 컬럼의 모든 로우를 concat하고 싶은 경우
```sql
SELECT GROUP_CONCAT([DISTINCT] col_name1 
[ORDER BY clause]  [SEPARATOR str_val])
FROM table_name GROUP BY 'all';
```