## Tomcat8.0  zip安装方式

### 下载并解压

[官网下载](https://tomcat.apache.org/download-80.cgi) 选择Core-ZIP，即完整的二进制版本

解压后，将bin目录添加到环境变量

### conf目录配置文件

#### server.xml文件

Tomcat服务器的主要配置文件。在这个文件中，可以设置端口、连接器、主机等。

```xml
<!-- 这是一个Tomcat服务器配置文件的示例 -->
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">

  <!-- 添加一个版本记录器监听器 -->
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />

  <!-- 添加一个Apr生命周期监听器，并启用SSL引擎 -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />

  <!-- 添加一个JRE内存泄漏预防监听器 -->
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />

  <!-- 添加一个全局资源生命周期监听器 -->
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />

  <!-- 添加一个线程本地泄漏预防监听器 -->
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

  <!-- 添加一个全局命名资源，这里定义了一个名为jdbc/mydb的JDBC数据源 -->
  <GlobalNamingResources>
    <Resource name="jdbc/mydb" auth="Container"
              type="javax.sql.DataSource" driverClassName="com.mysql.jdbc.Driver"
              url="jdbc:mysql://localhost:3306/mydb"
              username="myuser" password="mypassword"
              maxTotal="20" maxIdle="10" maxWaitMillis="-1"/>
  </GlobalNamingResources>

  <!-- 添加一个名为Catalina的服务，包含了两个连接器和一个引擎 -->
  <Service name="Catalina">

    <!-- 添加一个普通的HTTP连接器 -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

    <!-- 添加一个启用SSL的HTTPS连接器 -->
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
               maxThreads="150" scheme="https" secure="true"
               keystoreFile="conf/keystore.jks" keystorePass="changeit"
               clientAuth="false" sslProtocol="TLS" />

    <!-- 添加一个引擎，引擎名为Catalina，包含了一个LockOutRealm和一个UserDatabaseRealm，
    以及一个名为localhost的主机 -->
    <Engine name="Catalina" defaultHost="localhost">
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>
      <Host name="localhost" appBase="webapps" unpackWARs="true"
            autoDeploy="true">
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
    </Engine>
  </Service>
</Server>

```



#### web.xml文件

#### context.xml文件

#### logging.properties文件

Tomcat服务器的日志配置文件。它可以定义Tomcat服务器的日志级别、日志文件位置、格式等。

### 常用命令

进入bin目录

- 启动Tomcat

```
startup
```

​	

- 停止Tomcat

```
shutdown
```

​	

- 查看Tomcat状态

```
status
```

​	



