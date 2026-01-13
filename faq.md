# FAQ


#### Q1:TiDB当前覆盖多少地域？

TiDB当前支持华北（北京2）、华东（上海2）、广东等地域。在其它地域有需要使用的用户请联系技术支持或者客户经理开放使用。

#### Q2：每个小时业务要创建一些中间表，完成计算，然后删除掉，频繁创建和删除表对TiDB 性能影响大吗？

数据量较小的情况下可以忽略影响， 在数据量较大的情况下删除操作耗时较长，另外对GC有一定的影响， 如果堆积需要GC的数据量大，会影响写入的性能. 此种场景建议中间表使用分区表来避免性能影响。

#### Q3: slow log 里面的时间是什么时区？

admin show slow 是跟着服务所在宿主机的时区的，没法设置，建议使用select语句查询，select 会应用你设置的时区信息。

```
select * from information_schema.slow_query ;

admin show slow log top 4;
```

#### Q4: 对于一张大表insert into t2 select * from t1; 失败报错 ERROR 2013 (HY000): Lost connection to MySQL server during query

这种情况基本上是 Transaction too large

TiDB对事务有限制：

单个事务包含的 SQL 语句不超过 100000 条。每个键值对不超过 6MB，键值对的总大小不超过 100MB。

#### Q5: TiDB 是否支持 select for update？

支持，使用悲观锁时与MySQL基本一致。 使用乐观锁时语义上和 MySQL 有区别，TiDB 是分布式数据库，select for update 不在事务开启就锁住数据，而是其他事务在提交的时候进行冲突检查，如有冲突，会进行回滚。

#### Q6: 事务太大(insert ... select)，或者select..for update，以及网络问题，执行事务TiDB会返回错误，有什么办法可以区分吗？

可根据TiDB的 error codes 去判断


#### Q7: TIDB能否支持session级别的悲观锁

执行 set @@tidb_txn_mode = 'pessimistic';，使这个 session 执行的所有显式事务（即非 autocommit 的事务）都会进入悲观事务模式。


#### Q8：TiDB binlog的目标端在以下情况不支持udb-mysql5.6.41版本

udb-mysql5.6.41的索引键前缀默认限制为767字节，TiDB的表设计的key过长，全量同步时会报错。

如果一定要使用udb-mysql5.6版本，需如下操作：

1.目标端启用系统变量innodb_large_prefix

      1).系统变量innodb_large_prefix为ON

      2).系统变量innodb_file_format为Barracuda

如果用户权限不够，先调整自己的super权限：
```
mysql>update mysql.user set super_priv = 'Y' where user = 'root';

mysql>flush privileges;

mysql>set global innodb_large_prefix=on;

mysql>set global innodb_file_format=Barracuda;
```

2.源端需要修改表属性：

```
mysql> ALTER TABLE TEST ROW_FORMAT=DYNAMIC;
```

目标端支持：udb-mysql 5.7


#### Q9：在 TiDB 中，表或字段设置为utf8 和 设置为 utf8mb4 的效果是否一样

v2.1.3及其后续版本，默认字符集由uft8改为utf8mb4，效果是一样的，但为了保证备份数据和binlog导出的数据能兼容其他数据库，需显式的指定为utf8mb4

#### Q10: TiDB 加个联合索引会锁表吗

首先，TiDB内部没有锁表的机制：https://pingcap.com/docs-cn/dev/reference/sql/statements/flush-tables/#mysql-%E5%85%BC%E5%AE%B9%E6%80%A7

其次，TiDB 中，ADD INDEX 为在线操作，不会阻塞表中的数据读写。https://pingcap.com/docs-cn/dev/reference/sql/statements/add-index/

但是， 如果在创建索引的时候，刚好跟要读写的数据 碰巧是同一部分数据，这个是会影响的，因为创建索引需要填充数据，也会涉及读写操作。

从历史版本读就不影响读取的速度。


#### Q11:TiDB默认时区

当前实例创建完成后，默认时区为UTC时间，如果用户需要CST时间，需要手动设置时区+8

```
SET time_zone = '+8:00';   SET GLOBAL time_zone ='+8:00';
```

重新连接mysql即可生效


#### Q12:查看TiDB创建索引的过程是否已经结束

通过“admin show ddl;”语句查看当前job的进度


#### Q13：TiDB 最大连接数

默认为2048， 可通过控制台修改 https://docs.ucloud.cn/tidb/guide/configure

#### Q14: SQL执行时间突然变长

在执行SQL语句前，TiDB会通过统计信息计算出执行计划，选择全表扫还是从索引中获取数据，如果一张表数据量非常大，TiDB的选择算法误差比较大，一旦选择全表扫，会严重影响集群性能，建议强制使用索引

use index(index_name):https://book.tidb.io/session4/chapter6/sql-optimization-cases.html#%E6%A1%88%E4%BE%8B5-sql-%E6%89%A7%E8%A1%8C%E8%AE%A1%E5%88%92%E4%B8%8D%E5%87%86

#### Q15: 如何通过tableID 查找表名

INFORMATION_SCHEMA.TABLES 中有对应关系。

#### Q16: 如何查看当前版本

```
select tidb_version();
```

#### Q17: 如何查看慢查询

4.0 及以后的版本查询information_schema.cluster_slow_query表， 例如：
```
select query_time, query from information_schema.cluster_slow_query 
  where is_internal = false and time > '2021-05-24 05:00:00' 
  order by query_time desc limit 10;
```

4.0 之前的版本通过information_schema.slow_query表查看当前连接点的慢查询语句，如果要查询所有节点的慢查询， 请联系技术支持。

#### Q18: 监控图看有慢查询， 但是information_schema.cluster_slow_query表中无法查询到？

要使用有完全权限的账号，建议使用root账号

#### Q19: 如何处理 TiCDC 创建同步任务或同步到 MySQL 时遇到 Error 1298: Unknown or incorrect time zone: 'UTC' 错误?

这是因为下游 MySQL 没有加载时区，可以通过 mysql_tzinfo_to_sql 命令加载时区，加载后就可以正常创建任务或同步任务。

mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root mysql -p

#### Q20: TiDB数据库报错 ERROR 1105 (HY000): Out Of Memory Quota处理方法

目前有两种处理方式

1. 客户在执行oom的语句前设置session变量 SET tidb_mem_quota_query = 8 << 30;

2. 联系技术支持，让后端同学修改tidb的配置文件，使其永久生效。