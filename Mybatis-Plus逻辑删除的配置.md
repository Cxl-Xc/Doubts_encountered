Mybatis-Plus逻辑删除的配置

application.properties 加入配置

此为默认值，如果你的默认值和mp默认的一样,该配置可无

```java
#   未删除状态的值为：0
mybatis-plus.global-config.db-config.logic-not-delete-value=0

#   删除状态的值为：1
mybatis-plus.global-config.db-config.logic-delete-value=1
```

