## sql1
### 首先是查询问题
```sql
SELECT xxx
FROM XXX AS XXX
WHERE XXX;
```
- where用于筛选
- 还可以使用布尔运算
     ```sql
    SELECT name , num dogs
    FROM Person
    WHERE age >= 18
    AND num dogs > 3 ;
    ```
- NULL
- - 首先 null做任何事情都会只能得到NULL
- - 其次使用NULL会导致直接短路
- - 永远都是假值
    ```sql
    SELECT name , num dogs
    FROM Person
    WHERE age <= 20
    OR num dogs = 3 
    ```
### 分组和聚合函数
#### 接下来是一些函数
1. sum:求和
2. avg：求平均
3. max：最大值
4. min：最小值
5. count：数量
#### 数据库汇总
1. having子句
   - 他在分组之后发生，过滤掉不想要的组
   - where在分组之前发生
   - ```sql
        SELECT product, SUM(amount) AS total_sales
        FROM sales
        GROUP BY product
        HAVING SUM(amount) > 500;

2. group语句
   - 和各种函数一起使用
   - 简化结果 
   - GROUP BY 会将结果集按照指定的列进行分组，从而合并重复的记录。每个分组只返回一条记录 
   - ```sql
        SELECT product, SUM(amount) AS total_sales
        FROM sales
        GROUP BY product
        HAVING SUM(amount) > 500;
### 排列方式
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```
asc表示升序，desc表示降序
### 限制
就比如说我想看到几行，那么就limit多少
```sql
SELECT name , num dogs
FROM Person
LIMIT 1 
```
### 板子
```sql
SELECT <columns>#此处可以使用distinct 使得返回内容不重复
FROM <t b l >
WHERE <p r e d i c a t e >
GROUP BY <columns>
HAVING <p r e d i c a t e >
ORDER BY <columns>
LIMIT <num>
```
接下来是完成作业的时候了
作业如下
We have a dogs table that looks like this:
```sql
CREATE TABLE dogs (
dogid integer ,
ownerid integer ,
name varchar ,
breed varchar ,
age integer
)
```
1. Write a query that finds the name of every dog that has an ownerid=3.
2. Write a query that lists the 5 oldest dogs’ name and age. Break ties by the dog’s name in
ascending alphabetical order.
CS 186, Spring 2021, Course Notes 10 Brian DeLeonardis

```sql
SELECT name
FROM dogs AS d
WHERE d.ownerid=3
```
```sql
SELECT name,age
FROM dogs AS d
ORDER BY d.age ASC
LIMIT 5;
```
```sql
SELECT breed,count(*)
FROM dogs AS d
GROUP BY d.breed
HAVING COUNT(*)>1
```
---
## sql2
主题是**连接**和**查询**
### 交叉连接
- 也可以叫做笛卡尔积
- 作用是把多个表连接到一起去
- 可以简单的合在一起也可以依据一些条件耦合在一起
1. 接下来是简单的连接
```sql
SELECT *
FROM ABIAO,BBIAO;
# 直接把a b两个表直接连接到一起去
```
2. 然后是有一些条件的连接
```sql
SELECT*
FROM A,B
WHERE A.a=B.a;
# 这会使得依据WHERE的条件创建生成表
```
### 内部连接
- **解释**：是SQL中用于结合两张或多张表的数据的一种操作。它只返回那些在连接条件上匹配的记录，也就是说，只有在所有参与的表中都存在的记录才会出现在结果集中。
- 代码示例
```sql
SELECT*
FROM A INNER JOIN B
ON A.a=B.a;
```
### 外部连接
- **解释**：外部连接（OUTER JOIN）是SQL中用于结合两张或多张表的数据的一种操作，它与内部连接的不同之处在于，外部连接会返回所有记录，即使它们在连接条件上没有匹配的记录。
- 分为左外连接右外连接和全外连接
```sql
SELECT *
FROM A RIGHT OUTER JOIN B
ON A.a=B.a;
# 这里把right换成 left all分别对应左外和全外连接
```
- 对于没有匹配的值 则变成NULL
### 名称冲突
不多讲，就是用as代替原来的名字方便对表格进行操作
### natural join 自然连接
- 如果要连接的列有相同的名称，自动连接不同表中的相同名称的列
```sql
SELECT *
FROM COURCS NATURAL JOIN ABDWO;
```
### 子查询
**就是嵌套的意思**
```sql
SELECT num
FROM enrollment
WHERE students>= (
    SELECT AVG(students)
    FROM enrollment ;
) ;
```
#### 必须值得一提的是，这里大于等于必须只能返回一行
#### 同时，也可以在form中使用子查询 这允许从查询中获得一个临时的表
---
> 完全的题外话，实际上只需要会查询就可以了，剩下的都可以用java或者py或者c++来解决，实际上用一些后端语言来解决大数据问题的时候处理的更快，接下来的便是有趣的手搓数据库