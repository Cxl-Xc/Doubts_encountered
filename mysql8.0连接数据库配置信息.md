配置文件jdbc.properties

 

jdbc.driver=com.mysql.cj.jdbc.Driver

jdbc.url=jdbc:mysql://localhost:3306/test?serverTimezone=UTC

jdbc.username=root

jdbc.password=root

 

Mysql8.0导入jar驱动包配置信息

<dependency>

   <groupId>mysql</groupId>

   <artifactId>mysql-connector-java</artifactId>

   <version>8.0.11</version>

</dependency>   

<dependency>

   <groupId>com.mchange</groupId>

   <artifactId>c3p0</artifactId>

   <version>0.9.5.2</version>

  </dependency>
