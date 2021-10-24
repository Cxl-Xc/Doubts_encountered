type-aliases-package作用

在Mybatis的mapper.xml文件中resultType的type或者paramterType会返回自定义entity，此时可以用全类名名来指定这些实体。
举例：

```java
<select id="getUserInfoList" resultType="com.leo.model.UserInfo">
    SELECT
    <include refid="BaseSql"/>
    FROM USER_INFO WHERE 1=1
</select>
```

里面的resultType返回的就是全类名。

其实可以使用type-aliases-package中指定entity扫描包类让mybatis自定扫描到自定义的entity。
