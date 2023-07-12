# PostGIS学习

## 起步

### 安装PostgreSQL

[PostgreSQL 11 Windows版下载](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

exe方式安装：

- 计算机名称不能为中文，不然会出错
- 设置默认密码123456、端口号5432

### 安装PostGIS

[官方文档](https://postgis.net/documentation/getting_started/)

[PostGIS 3.3.3 Windows版本下载](https://download.osgeo.org/postgis/windows/pg11/)

选择windows64位exe安装包，安装在PostgreSQL的安装目录

## 数据库操作

### 创建数据库

```shell
createdb -U postgres dish
```

### 连接数据库

使用系统默认用户名，并输入密码

```
psql -U postgres
```

查看版本信息

```
SELECT version();
```

退出

```
\q
```

导入数据

```
psql -U postgres -f nyc_buildings.sql nyc
```

