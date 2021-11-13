MySQL一般定义字段用下划线表示，比如book_name，good_id···

在spring boot项目中使用mybatis框架开发时，在写好SQL select查询语句后，发现查询出的对象字段都为null，这个时候要检查SQL语句查询返回的字段和映射实体类属性是否一致，如果不使用mybatis配置，则需要使用as重命名，不过这样要敲很多键盘，不仅写出的SQL有些冗杂，而且易出错，所以推荐使用mybatis配置项

mybatis.configuration.map-underscore-to-camel-case=true

该配置项就是指将带有下划线的表字段映射为驼峰格式的实体类属性。

因此，添加了该项配置后，在开发中只需要根据查询返回的字段，创建好实体类就可以了！
