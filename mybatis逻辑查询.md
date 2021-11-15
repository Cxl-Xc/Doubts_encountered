mybatis逻辑查询

* or
  * 拼接OR
  * 主动调用or表示紧接着下一个方法不是用and连接!(不调用or则默认为使用and连接)
* and
  * AND嵌套
  * 例：and(i ->i.eq("name","李白").ne("status","活着"))--->and(name='李白'and status <> '活着')

