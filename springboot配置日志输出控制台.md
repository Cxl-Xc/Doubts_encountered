springboot配置日志输出控制台

在application.yml文件中输入

```java
mybatis-plus:
	configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

