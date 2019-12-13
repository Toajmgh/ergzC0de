title: 图解SQL的各种join用法
author: ergz
tags:
  - sql
categories:
  - SQL
date: 2019-11-09 15:50:00
---
**下图展示了LEFT JOIN、RIGHT JOIN、INNER JOIN、OUTER JOIN 相关的 7 种用法**

![upload successful](/images/sql_201912121520.png)
<!-- more -->
#### 具体分解如下：
##### 1.INNER JOIN（内连接）
![upload successful](/images/sql_201912121523.png)
```sql
SELECT <select_list> 
FROM Table_A A
INNER JOIN Table_B B
ON A.Key = B.Key
```
##### 2.LEFT JOIN（左连接）
![upload successful](/images/sql_201912121558.png)
```sql
SELECT <select_list>
FROM Table_A A
LEFT JOIN Table_B B
ON A.Key = B.Key
```
##### 3.RIGHT JOIN（右连接）
![upload successful](/images/sql_201912121528.png)
```sql
SELECT <select_list>
FROM Table_A A
RIGHT JOIN Table_B B
ON A.Key = B.Key
```
##### 4.OUTER JOIN（外连接）
![upload successful](/images/sql_201912121532.png)
```sql
SELECT <select_list>
FROM Table_A A
FULL OUTER JOIN Table_B B
ON A.Key = B.Key
```
##### 5.LEFT JOIN EXCLUDING INNER JOIN（左连接-内连接）
![upload successful](/images/sql_201912121531.png)
```sql
SELECT <select_list> 
FROM Table_A A
LEFT JOIN Table_B B
ON A.Key = B.Key
WHERE B.Key IS NULL
```
##### 6.RIGHT JOIN EXCLUDING INNER JOIN（右连接-内连接）
![upload successful](/images/sql_201912121552.png)
```sql
SELECT <select_list>
FROM Table_A A
RIGHT JOIN Table_B B
ON A.Key = B.Key
WHERE A.Key IS NULL
```
##### 7.OUTER JOIN EXCLUDING INNER JOIN（外连接-内连接）
![upload successful](/images/sql_201912121553.png)
```sql
SELECT <select_list>
FROM Table_A A
FULL OUTER JOIN Table_B B
ON A.Key = B.Key
WHERE A.Key IS NULL OR B.Key IS NULL
```