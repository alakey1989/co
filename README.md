mysql知识

事务隔离级别由低到高:https://www.cnblogs.com/rjzheng/p/9950951.html

1 读未提交-> 造成脏读
2 读已提交-> 可避免脏读, 但造成不可重复读、幻读
3 可重复读(默认)-> 可避免脏读、不可重复读(因为读取的是某记录的快照), 但依然会存在幻读
4 串行化-> 可避免脏读、不可重复读、幻读的发生。

共享锁(S): 事务A加了共享锁,事务B只能加共享锁,而不能加排他锁
排他锁(X): 事务A加了排它锁,事务B既不能加共享锁,也不能加排他锁

普通select是不加任何锁的, 并且是快照读。但是! 当处于串行化隔离级别下, select语句都自动加上共享锁, 是当前读。
select ... lock in share mode 会加共享锁, 是当前读。
select ... for update 会加排他锁, 是当前读。

聚簇索引和非聚簇索引：https://blog.csdn.net/u013132035/article/details/82193763
注意：Innodb的索引储存是聚簇索引。Innodb里面没有非聚簇索引，更加准确的叫法是辅助索引。非聚簇索引针对MyISAM引擎而言。

MyISAM与InnoDB索引比较:
MyISAM的索引文件, 叶节点的data域存放的是数据记录的地址。
MyISAM主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而辅助索引的key可以重复。
而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。
InnoDB的辅助索引data域存储相应记录主键的值而不是地址。换句话说，InnoDB的所有辅助索引都引用主键作为data域。
因为InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有）



