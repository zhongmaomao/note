# 问题
### count(*) count(1) count(字段) 区别
```sql
select count(expr) from table_name;  
select expr from table_name;
```
Innodb引擎没有额外存储行数(相对于MyISAM)，count需要走B+树索引或全表查询，具体根据expr中是否包含索引字段

### update
1. 用户连接、权限判断、(清空缓存)
2. 语法分析、词法分析
3. 预处理器 -> 优化器 -> 执行器
4. 执行器通过条件查询到聚簇索引树上的行记录(有可能回表或全表搜索)
    1. 若行记录在buffer pool直接返回给执行器
    2. 若buffer pool不存在行记录，则将磁盘对应数据页写入buffer pool
5. 执行器查看返回的行记录与更新后的记录是否一致，不一致则将两条记录都传给innodb层处理
6. 开启事务，innodb层生成对应undo log，并用roll pointer连接成版本链用于MVCC，undo log写入buffer pool中的Undo页面，内存修改Undo页面后也会记入redo log
7. InnoDB开始更新记录，先更新内存buffer pool中数据页(同时标记为脏页)，然后记录写入redo log(redolog buffer / 内核page cache / 磁盘)。后台线程另择时机将内存中的脏页通过磁盘IO写入磁盘
8. 更新语句执行完毕。返回sever层并记录对应的binlog，binlog会写入内存中的binlog cache，(写入内核page cache缓存)，事务提交后再刷新到磁盘
9. 事务两阶段提交

### 磁盘IO高
1. 延长组提交时binlog等待时间和队列大小(sync_binlog = N)
2. redolog在Flush阶段不直接写入磁盘(innodb_flush_log_at_trx_commit = 2)，而是写入文件(page cache)，然后由操作系统刷盘

### 什么时候需要索引
1. 优点：提高查询速度  
   缺点：占用物理空间、创建维护索引耗时、降低表的删改效率
2. 适用：字段有唯一性限制、经常用于WHERE, GROUP BY, ORDER BY
3. 不用：不用于定位、大量重复数据、经常更新、数据量小

### 索引下推、索引覆盖

### 内部/外部XA事务

# 索引
## 分类
1. 物理结构：B+tree索引、Hash索引、Full-text索引
2. 物理存储：聚簇索引、二级索引
3. 字段特性：主键索引、唯一索引、普通索引、前缀索引
4. 字段数量：单列索引、联合索引

## 索引优化
1. 前缀索引优化、覆盖索引优化
2. 主键索引最好自增
3. 索引最好NOT ALL
4. 防止索引失效


## 索引失效
1. 当我们使用左或者左右模糊匹配的时候，也就是 like %xx 或者 like %xx%这两种方式都会造成索引失效(但可能全扫描索引树(索引覆盖))；
2. 当我们在查询条件中对索引列使用函数，就会导致索引失效(MySQL8函数索引)。
3. 当我们在查询条件中对索引列进行表达式计算，也是无法走索引的。
4. MySQL 在遇到字符串和数字比较的时候，会自动把字符串转为数字，然后再进行比较。如果字符串是索引列，而条件语句中的输入参数是数字的话，那么索引列会发生隐式类型转换，由于隐式类型转换是通过 CAST 函数实现的，等同于对索引列使用了函数，所以就会导致索引失效。
5. 联合索引要能正确使用需要遵循最左匹配原则，也就是按照最左优先的方式进行索引的匹配，否则就会导致索引失效(索引截断回表 or 索引下推)。
6. 在 WHERE 子句中，如果在 OR 前的条件列是索引列，而在 OR 后的条件列不是索引列，那么索引会失效。

![索引总结.drawio](https://raw.githubusercontent.com/zhongmaomao/img/master//md/pictures/索引总结.drawio.webp)

# 事务

## 特性
原子性(回滚日志)、隔离性(MVCC或锁)、持久性(重做日志)、一致性

## 隔离级别、效果及实现方式
1. 读未提交：脏读、不可重复读、幻读
2. 读已提交：不可重复读、幻读
3. 可重复读：幻读 -> (MySQL针对快照读MVCC和当前读next-key lock部分解决幻读方法)
4. 串行化

读已提交和可重复读Read View创建时机  
注意事务开始时机：begin/start transaction或start transaction with consistent snapshot  
DDL DML DCL
## Read View in MVCC
Read View四个字段：creator_trx_id, m_ids, min_trx_id, max_trx_id  
聚簇索引的隐藏列：trx_id, roll_pointer

# 锁

## 类型
1. 全局锁(MyISAM, Innodb -> MVCC)
2. 表级锁
>表锁：读锁、写锁  
元数据锁MDL：CRUD操作加MDL读锁、结构变更加MDL写锁，写锁在阻塞队列中优先于读锁  
意向锁：优化加表锁前对元数据锁的查询过程  
AUTO-INC锁：mode -> 0,1,2(innodb_autoinc_lock_mode = 2 + binlog_format = row)  

3. 行级锁
>Record Lock : 读锁S、写锁X  
Gap Lock : 间隙锁不存在互斥关系，只是为了防止插入幻影记录  
Next-Key Lock  
插入意向锁 : 特殊的间隙锁

## 查询加锁过程
select * from performance_schema.data_locks\G;
### 唯一索引查询
1. 等值查询：根据值是否存在退化为记录锁或间隙锁
2. 范围查询：扫描过程，每扫描到一个符合条件的点就加一个next-key lock(可能根据边界条件退化)
### 非唯一索引查询
非唯一索引查询会同时考虑对主键索引、二级索引(这个非唯一索引)加锁
1. 等值查询：二级索引存在则加临值锁(二级索引包含了主键索引用来确定唯一位置，字典序)，对应主键索引加记录锁，直到第一个不存在的索引退化为间隙锁
2. 范围查询：二级索引临值锁不退化
### 无索引或不走索引
update, delete, select..for update等若不走索引，因为进行全表扫描，会锁全表

## insert
insert正常执行不加锁，靠trx_id隐藏列作为隐式锁，只有在以下两种情况转换为显式锁
1. 插入记录的下一条记录有间隙锁(两事务由于幂等检查都申请不到插入意向锁，形成死锁)
2. 遇到唯一键(主键或唯一二级索引)冲突

# 日志

## undo log
1. undo log保证事务原子性，保存事务开始前状态，undo log对C、U、D操作有不同格式记录
2. undo log + Read View -> MVCC, undo log通过roll pointer指针串成版本链
3. undo log(undo页 in Buffer Pool)的持久化刷盘也通过redo log实现

## redo log
1. 事务先写redo log再写数据库的磁盘空间，redo log在单个文件表现为追加写入，顺序写，速度快于直接写入数据库中指定位置的随机写
2. redo log保证事务的持久性,物理日志：记录某表空间某数据页某偏移量处做了某更新
3. redo log在内存有redo log buffer,当事务提交(innodb_flush_log_at_trx_commit = 0, 1, 2)或者redo log buffer写入一半或者定时(1s)刷盘
4. redo log循环写入ib_logfile0和ib_logfile1, 擦除check point前的记录，在write pos写入新的记录，check point和write pos间即待落盘脏页记录

## bin log
1. bin log不由innodb引擎生成，由sever层管理
2. bin log：STATEMENT, ROW, MIXED
3. bin log与redo log区别：写入策略、发生的层级、作用、文件格式
4. 主从复制：半同步复制
5. bin log cache

## 两阶段提交
解决redolog和binlog因为刷盘半成功而出现的不一致。
1. 协调者(内部XA事务(XID)), 参与者(事务redolog, binlog); 
2. 准备阶段(redolog刷盘->ok)、提交阶段(binlog刷盘->ok、redolog修改提交状态为commit->ok)
3. 在任何时间系统崩溃时，比较磁盘中处于prepare阶段的redolog，如果binlog中有相同XID的记录，说明都提交成功了，否则回滚undolog
   
### binlog组提交
问题：磁盘IO次数多、锁竞争激烈(加锁保证多事务提交时两日志写入顺序一致)
1. flush阶段, sync阶段, commit阶段
2. 每个阶段维护一个队列，每个阶段加锁保护(不再锁整个事务过程，粒度减小)，队列头的事务负责整个队列的操作，完成后通知队内其他事务已完成
3. flush阶段：Flush redolog + write binlog  
   sync阶段：sync binlog
   commit阶段：InnoDB Commit

# 内存

1. Buffer Pool ：数据页、索引页、插入缓存页、undo页、自适应哈希索引、锁信息  
2. 控制块  
3. Free链表、Flush链表
4. LRU算法，提高缓存命中率：预读失效、Buffer Pool污染
5. LRU链表：young区域热点页、old区域  
   划分区域解决预读失效、控制块添加访问时间解决Buffer Pool污染


