## MySQL8.0  zip安装方式

### 1.下载并解压

### 2.初始化配置文件

[官方教程](https://dev.mysql.com/doc/refman/8.0/en/windows-install-archive.html)

​	1.bin同级目录新建my.ini文件,复制配置内容

```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\mysql-8.0.27-winx64 
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql-8.0.27-winx64\data  
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```

​	2.修改basedir

​	3.修改datadir

### 3.初始化数据库

​	1.管理员打卡cmd，进入bin目录

​	2.生成随机密码方式

```
mysqld   --initialize   --console
```

会得到一串临时密码，**要保存！！！**

3.不生成密码

```
mysqld   --initialize--insecure
```



### 4.安装mysql服务

1.输入命令

```
mysqld  --install  [MySQL3307]
```

默认名称是MySQL,

2.启动服务器

```
net  start  MySQL3307
```

3.关闭服务器

```
net stop MySQL3307
```

4.删除服务

```
SC DELETE MySQL3307
```

或者

```
mysqld --remove
```



### 5.进入mysql并修改密码

进入mysql

```
 mysql -u root -p
```

修改密码

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码'; 
```



## MySQL5.7 zip安装

```
[mysql]
# 设置mysql客户端默认字符集
default-character-set = utf8

[mysqld]

# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin

# These are commonly set, remove the # and set as required.
# 设置mysql的安装目录
basedir = D:\path-to-mysql
# 设置mysql数据库的数据的存放目录
datadir = D:\path-to-mysql\data
#设置3306端口
port = 3306
# server_id = .....

# 允许最大连接数
max_connections = 200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server = utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine = INNODB

# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M 

sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
# wait_timeout = 28800
```

需要保存为ANSI格式，其他操作步骤一样
