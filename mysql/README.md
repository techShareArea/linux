### 前言
mysql圣经

#### myisam与innodb的5大区别
```
1. innodb支持事务，而myisam不支持事务
2. innodb支持行级锁，而myisam支持表级锁
3. innodb支持mvcc，而myisam不支持      
4. innodb支持外键，而myisam不支持
5. innodb不支持全文索引，而myisam支持    
```

#### SQL语言的四大模型
```
1. 数据定义(DDL): create,alter,drop等
2. 数据操纵(DML): select,insert,update,delete等
3. 数据控制(DCL): grant,revoke等
4. 数据查询(DQL): select等
```

#### 数据库事务的四大特性
```
1. 原子性
2. 一致性
3. 独立性
4. 持久性
```

#### 优化数据库的方法
```
1. 选取最适用的字段属性，尽可能减少定义字段宽度，尽量把字段设置为NOTNULL，例如:"省份"，性别最好使用ENUM
2. 使用连接(JOIN)来代替子查询
3. 使用联合(UNION)来代替手动创建临时表
4. 事务处理
5. 锁定表，优化事务处理
6. 使用外键，优化锁定表
7. 建立索引
8. 优化查询语句
```

#### 主键、外键和索引的区别
```
1. 定义
1.1 主键: 唯一标识一条记录，不能有重复的，不允许为空
1.2 外键: 表的外键是另一表的主键，外键可以为空值
1.3 索引: 可以为空值

2.作用
2.1 主键: 用来保证数据完整性
2.2 外键: 用来和其它表建立联系
2.3 索引: 提高查询速度

3.个数
3.1 主键: 只能有一个
3.2 外键: 一张表可以有多个外键
3.3 索引: 一个表可以有多个唯一索引
```

#### mysql主从

##### 形式
```
1. 一主一从
2. 一主多从
3. 互为主从
4. 多主一从
5. 联级复制
```

##### 作用
```
1. 实时灾备，用于故障切换
2. 读写分离，使数据库能支撑更大的并发
3. 备份，避免影响业务
4. 架构的扩展
```

#### 主从复制步骤
```
1. 主库将所有的写操作记录在binlog日志中，并生成log dump线程，将binlog日志传给从库的i/o线程
2. 从库生成两个线程，一个是i/o线程，另一个是sql线程
3. i/o线程去请求主库的binlog日志，并将binlog日志的文件写入relay log(中继日志)中
4. sql线程会读取relay log中的内容，并解析成具体的操作，来实现主从的操作一致，达到最终数据一致的目的
```
补充说明:       
```
1. 主库db的更新事件(update、insert、delete)被写到binlog
2. 从库发起连接，连接到主库
3. 主库创建一个binlog dump thread线程，把binlog的内容发送到从库
4. 从库启动，创建一个i/o线程，读取主库传过来的binlog内容，并写入到relay log
5. 从库创建sql线程，从relay log里面读取内容，从exec_master_log_pos位置开始执行读取到的更新事件，将更新内容写入到slave的db
```

##### 配置步骤
```
1. 确保从数据库与主数据里的数据一致
2. 在主数据里创建一个同步账户授权给从数据库使用
3. 配合主数据库(修改配置文件)
4. 配合从数据库(修改配置文件)
```

##### 解决主从同步复制，从库延迟，主库宕机导致从库不能及时同步数据的问题
```
1. 半同步复制    # 解决主库数据丢失问题
2. 并行复制      # 解决主从同步延时问题
```

##### 主从复制涉及到的线程
```
1. 主库: binlog
2. 从库: i/o，sql
```

#### mysql读写分离
基于主从复制架构，主库负责写，也会自动把数据同步到从库
