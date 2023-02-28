## MySQL主从备份

### 通过日志备份

#### 主库配置

my.ini配置文件

```
[mysqld]
#server_id不能和从库一样
server_id= 2
#log-bin包含路径和文件名
log-bin=/path-to-log/mysql-bin
```

重启数据库

```
net stop mysql
net start mysql
```

登录主数据库，查询bin-log是否打开

```
mysql> show variables like '%log_bin%';
```

建立用于主从复制的账号

```
mysql> CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' WITH GRANT OPTION;
mysql> flush privileges;
```

查看账号

```
mysql> select user,host,plugin from user;
```



#### 从库配置

设置server-id并关闭binlog功能

```
server_id=21
skip-log-bin
```

重启数据库

#### 已有数据备份

可通过 mysqldump 备份日志坐标节点前的所有数据

```
mysqldump --all-databases --master-data > dbdump.db
```



#### 主库日志文件与开始位置

主库查询

```
mysql> show master status;
```

从库配置主库连接信息、日志路径和位置

```
mysql> CHANGE MASTER TO
    -> MASTER_HOST='localhost',
    -> MASTER_PORT=3306,
    -> MASTER_USER='repl',
    -> MASTER_PASSWORD='xxxx',
    -> MASTER_LOG_FILE='mysql-bin.000001',
    -> MASTER_LOG_POS=1220;
```

启动

```
start slave;
```

查看slave进程状态

```
show slave status\G
```



#### 验证

主库

```
create database test;
```

从库

```
show database;
```



### 通过GTID备份