mybatis模糊查询

* like
  * LIKE'%值%'
  * 例：like("name","王")--->name like '%王%'
* notLike
  * NOT LIKE '%值%'
  * 例：notLike("name","王")--->name not like'%王%'
* likeLeft
  * LIKE'%值%'
  * 例：likeLeft("name","王")--->name like '%王'     前面是任何字符 最后是'王'
* likeRgiht
  * Like'值'
  * 例：likeRight("name","王")--->name like'王%'