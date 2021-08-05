### MySQL大数据量分页性能优化

##### 1.  直接用limit start, count分页语句， 也是我程序中用的方法：

select * from product limit start, count
当起始页较小时，查询没有性能问题，我们分别看下从10， 100， 1000， 10000开始分页的执行时间（每页取20条）， 如下：

select * from product limit 10, 20  0.016秒
select * from product limit 100, 20  0.016秒
select * from product limit 1000, 20  0.047秒
select * from product limit 10000, 20  0.094秒

我们已经看出随着起始记录的增加，时间也随着增大， 这说明分页语句limit跟起始页码是有很大关系的，那么我们把起始记录改为40w看下（也就是记录的一般左右）                   select * from product limit 400000, 20  3.229秒

再看我们取最后一页记录的时间
select * from product limit 866613, 20  37.44秒

难怪搜索引擎抓取我们页面的时候经常会报超时，像这种分页最大的页码页显然这种时
间是无法忍受的。

从中我们也能总结出两件事情：
1）limit语句的查询时间与起始记录的位置成正比
2）mysql的limit语句是很方便，但是对记录很多的表并不适合直接使用。

##### 2.  对limit分页问题的性能优化方法

利用表的覆盖索引来加速分页查询
我们都知道，利用了索引查询的语句中如果只包含了那个索引列（覆盖索引），那么这种情况会查询很快。

因为利用索引查找有优化算法，且数据就在查询索引上面，不用再去找相关的数据地址了，这样节省了很多时间。另外Mysql中也有相关的索引缓存，在并发高的时候利用缓存就效果更好了。

在我们的例子中，我们知道id字段是主键，自然就包含了默认的主键索引。现在让我们看看利用覆盖索引的查询效果如何：

这次我们之间查询最后一页的数据（利用覆盖索引，只包含id列），如下：
select id from product limit 866613, 20 0.2秒
相对于查询了所有列的37.44秒，提升了大概100多倍的速度

那么如果我们也要查询所有列，有两种方法，一种是id>=的形式，另一种就是利用join，看下实际情况：

SELECT * FROM product WHERE ID > =(select id from product limit 866613, 1) limit 20
查询时间为0.2秒，简直是一个质的飞跃啊，哈哈

另一种写法（延迟关联）
SELECT * FROM product a JOIN (select id from product limit 866613, 20) b ON a.ID = b.id



### 延迟关联

一个包含查询所需的字段的索引称为 covering index 覆盖索引。MySQL只需要通过索引就可以返回查询所需要的数据，而不必在查到索引之后进行回表操作，减少IO，提供效率。

   当你对一个sql 使用explain statement 查看一个sql的执行计划时，在EXPLAIN的Extra列出现Using Index提示时，就说明该select查询使用了覆盖索引。


2.延迟关联

采用延迟索引。"延迟关联" ：通过使用覆盖索引查询返回需要的主键,再根据主键关联原表获得需要的数据。

![](图片\20160118102512191.png)

