## MySQL 设置远程访问

### 防火墙开启端口

#### 1.windows系统

防火墙高级设置，新建入站规则，添加mysql服务端口如3306

#### 2.linux系统

命令行firewall设置

### MySQL Server添加远程主机访问权限

首先登录mysql，查看所有用户和主机信息

```
use mysql;
select Host, User from user;
```

增加一个新的用户及地址

```
create user 'username'@'%' identified by 'password';
grant all on *.* to 'username'@'%';
```

