# TABLE
----------
## 1.Hash table, sort table, standy table
	对于排序内表，不要使用append，append lines附加数据，使用insert、insert lines向排序内表中插入数据
## 2.search help 的创建
	
## 3.create table
	(1) Field：定义Short Description 分配Element应注意命名，缩略
	(2) Element：定义Field Label 和分配相应的Domain
	(3) Domain：定义具体的字段属性，Data type，NO.Cahracters，Decimal Places，Output Length
	(4) SM30：表维护   Utilities ----->Table maintenance generator
			Technical Dialog Details
				Authorization Group：&NC&
				Function group：ZFG_tablename
			Maintenance Screens : 
				Maintenance type：one step
## 4. 表操作
	双层loop循环时，第二个内部表定义为SORTED TABLE会大大的提高处理速度
## 5. SQL 语句的执行顺序
	书写顺序：SELECT[DISTINCT]-->FROM-->WHERE-->GROUP BY-->HAVING-->UNION-->ORDER BY
	其执行顺序为：FROM-->WHERE-->GROUP BY-->HAVING-->SELECT-->DISTINCT-->UNION-->ORDER BY
	1、FROM 才是 SQL 语句执行的第一步，并非 SELECT 
	2、SELECT 是在大部分语句执行了之后才执行的，严格的说是在 FROM 和 GROUP BY 之后执行的。
	3、无论在语法上还是在执行顺序上， UNION 总是排在在 ORDER BY 之前。
# Open SQL
----------
## Focus
	1. 多表的结合的查询，首先进行各自的关键字查询，然后进行表与表转换可以提升效率
	2. 表相应字段非关键字时，寻找有该关键字的相对应的关联表进行查询并合并表数据
	3. binary search 和 sort 进行使用时注意sort的关键字与binary search 保持统一
	4. MODIFY itab1 INDEX sy-tabix.进行修改时最好定义变量记录sy-tabix.
	5. inner join 和 left outer join 的连接条件最好都是关键字.
