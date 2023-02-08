

## 函数

### 集中式事务型数据库应支持数值计算函数、字符处理函数、日期时间值函数、间隔函数、类型转换函数、位运算函数、聚合函数、格式化、系统信息等常用函数。

#### 支持数值计算函数：
  
(1) 创建表a :
create table a(
     aa int 
    )
(2)插入数据：
delete from a ;
insert into a(aa) values (1) ;
insert into a(aa) values (2) ;
(3) 计算求和：
SELECT sum(aa) from a ;
(4) 验证是否为3；

#### 支持字符处理函数：
  执行 SELECT concat('a','b') ;
  验证是否返回ab ;

#### 日期时间值函数:
   执行 ：SELECT current_date ;
   验证是否返回日期

#### 间隔函数 ：

（1）执行：
select 
  justify_interval(now - past::date) as interval_test
from (
  select '2022-12-20 00:00:01'::timestamp as now, '2021-12-20 00:00:01'::timestamp as past
) b;
（2）验证返回结果是否为1个月；

#### 类型转换函数：

 （1）执行：select to_char(current_timestamp,'HH:MI:SS') ;
   (2) 验证返回结果是否为字符串时间

#### 位运算函数：

执行  select  (60 << 2) ；
验证结果是否为240 ；

#### 聚合函数 ：

（1）创建表
 
（2）插入数据
insert into bricks values ( 'red', 'cube', 1 );

insert into bricks values ( 'red', 'pyramid', 2 );

insert into bricks values ( 'red', 'cuboid', 1 );

insert into bricks values ( 'blue', 'cube', 1 );

insert into bricks values ( 'blue', 'pyramid', 2 );

insert into bricks values ( 'green', 'cube', 3 );

（3）查询聚合：
 
select colour, count (*)

from   bricks

group  by colour;

（4）校验结果：
#### 
## 序列
### 集中式事务型数据库的扩展对象类型宜支持全局唯一的自增序列。

（1）创建序列
create sequence tbl_test_id_seq increment by 1 minvalue 1 no maxvalue start with 1;     

  (2) 查询序列值
select nextval('tbl_test_id_seq');

（3）再次查询
select nextval('tbl_test_id_seq');

（4）验证是否为2

### 集中式事务型数据库的扩展对象类型宜支持创建函数索引。

  (1) 创建表
create table a_f_i_t (
   a1 varchar(200)     
)
（2）创建函数索引
 create index f_i on a_f_i_t (upper(a1)) ;
（3）校验是否可以创建成功；

### 集中式事务型数据库的扩展对象类型宜支持定义同义词。
（1）创建表
create table s_t (
   a1 varchar(200)     
)
(2) 创建同义词指向表s_t 
create SYNONYM s_b for s_t ;
(3) 查询s_b ;
SELECT * from s_b ;
(4) 校验查询是否成功
## 查看数据库对象

### 支持查看数据库信息；

查看版本：
  select version() ;

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671518526013-5e74d0aa-67c2-4689-b584-e982865f8c37.png#clientId=u555655a5-41d8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=363&id=uf7479621&margin=%5Bobject%20Object%5D&name=image.png&originHeight=726&originWidth=1568&originalType=binary&ratio=1&rotation=0&showTitle=false&size=261379&status=done&style=none&taskId=u811df963-a4b3-4bdc-a551-4066a870e15&title=&width=784)

查看配置：
   select name,setting,unit  from pg_settings;

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671518544612-52a19b05-5b96-4eda-b718-b018c3a3b5e7.png#clientId=u555655a5-41d8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=451&id=ud82bde10&margin=%5Bobject%20Object%5D&name=image.png&originHeight=902&originWidth=2124&originalType=binary&ratio=1&rotation=0&showTitle=false&size=373778&status=done&style=none&taskId=u6ce9abfd-2a8f-48f2-8235-6bfc7765753&title=&width=1062)

### 支持查看表对象信息；

执行SQL查询元数据： 

SELECT b.nspname as schema_name, a.relname object_name, a.oid oid, CAST ( obj_description ( a.oid, 'pg_class' ) AS VARCHAR ) AS COMMENT, a.relkind as rel_kind, a.relispartition as relis_partition FROM pg_class a, pg_namespace b WHERE a.relnamespace = b.oid 

查看结果：

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671518322859-d58b57e7-a549-4289-aa97-285328a725d3.png#clientId=u555655a5-41d8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=426&id=u7066c840&margin=%5Bobject%20Object%5D&name=image.png&originHeight=852&originWidth=2852&originalType=binary&ratio=1&rotation=0&showTitle=false&size=439998&status=done&style=none&taskId=u31ab25a6-6df8-415c-9f5a-736131ffcd0&title=&width=1426)

### 支持查看索引对象信息；

执行SQL查询元数据库：

SELECT d.nspname as schema_name,b.relname as index_name, c.relname as object_name, indisunique as non_unique,b.oid as oid, c.relfilenode,pg_get_expr(a.indexprs, c.oid) as expression,pg_get_expr(a.indpred, c.oid) as filter_definition, e.amname as index_type , a.indkey as ind_key,  a.indcollation as ind_collation, a.indclass as ind_class, a.indoption as ind_option FROM pg_index a, pg_class b, pg_class c, pg_namespace d, pg_am e  WHERE a.indisprimary = false AND a.indexrelid = b.oid  AND a.indrelid = c.oid AND b.relnamespace = d.oid  AND b.relam = e.oid  and  a.indisvalid = true  AND b.relname NOT IN (SELECT e.conname FROM pg_catalog.pg_constraint e, pg_class f, pg_namespace g WHERE e.connamespace = g.oid AND e.conrelid = f.oid AND g.nspname = d.nspname AND f.relname = c.relname and e.contype in ('u','x'))

校验是否返回结果：：

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671518149896-b4e10d84-bcd3-4de3-88ad-73a062321aef.png#clientId=u555655a5-41d8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=444&id=ud8cd4fa5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=888&originWidth=2800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=497894&status=done&style=none&taskId=u230911aa-1839-4178-8f23-a3492731ba2&title=&width=1400)

### 支持查看字段对象信息；
执行SQL：
SELECT column_name, data_type as data_type ,data_type as column_type , datetime_precision, character_maximum_length as data_length, numeric_precision as data_precision, numeric_scale as data_scale, is_nullable , column_default , is_generated, generation_expression, col_description(b.oid, ordinal_position) as column_comment, pg_catalog.format_type(c.atttypid, c.atttypmod) as ex_data_type, c.attinhcount, c.atttypmod FROM information_schema.columns a, pg_class b, pg_attribute c, pg_namespace d WHERE  a.table_name = b.relname AND b.relnamespace = d.oid AND d.nspname = a.table_schema AND b.oid = c.attrelid AND a.column_name = c.attname 

查看结果： 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671440075115-6c40a984-1bfd-443d-8c9c-0cb9df954eef.png#clientId=u363ec7d4-64ac-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=567&id=uad94973e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1134&originWidth=2996&originalType=binary&ratio=1&rotation=0&showTitle=false&size=636281&status=done&style=none&taskId=u9f33949c-3dfa-4732-80e7-31c9af63ea9&title=&width=1498)

### 支持查看约束对象信息；
执行SQL：
SELECT c.nspname as schema_name, a.conname as constraint_name, a.contype as constraint_type, b.relname as object_name  FROM pg_constraint a, pg_class b, pg_namespace c WHERE a.connamespace = c.oid AND a.conrelid = b.oid 

校验是否返回结果：
![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671435304295-b4b03139-1cee-407e-b96d-34c6088a0897.png#clientId=u363ec7d4-64ac-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=496&id=u99a5f20c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=992&originWidth=2610&originalType=binary&ratio=1&rotation=0&showTitle=false&size=575488&status=done&style=none&taskId=u3ad13fbb-3f8e-4c1c-a722-0a8964984cc&title=&width=1305)



### 支持查看数据库实例信息；

执行SQL ：
  SELECT * FROM pg_database ORDER BY datname; 
校验是否返回结果：

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671518400668-fcd8e56a-d523-46d5-a3dc-cc5edbcb29b0.png#clientId=u555655a5-41d8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=634&id=u9236d3e0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1268&originWidth=2908&originalType=binary&ratio=1&rotation=0&showTitle=false&size=532521&status=done&style=none&taskId=udf18c91d-da5c-4da9-b114-ef983b00960&title=&width=1454)
   
### 支持查看表空间信息。

执行SQL： select spcname from pg_tablespace; 
校验查看结果，返回默认表空间；


![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/109031/1671433554277-219c1564-da95-4e94-8586-879e84d193d9.png#clientId=u363ec7d4-64ac-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=353&id=ua5e54342&margin=%5Bobject%20Object%5D&name=image.png&originHeight=706&originWidth=1072&originalType=binary&ratio=1&rotation=0&showTitle=false&size=211628&status=done&style=none&taskId=u7ef91fbe-fdec-4dcb-80e1-d9f0ca97817&title=&width=536)
