```java
server:
  servlet:
    context-path: /test
  port: 9099
spring:
  datasource:
    password: root
    username: root
    url: jdbc:mysql://localhost:3306/testdb?useUnicode=true&characterEncoding=utf-8&useSSL=false
    driver-class-name: com.mysql.cj.jdbc.Driver
mybatis-plus:
  mapper-locations: classpath*:/mapper/*.xml
```