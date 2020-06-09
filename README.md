# DataBase

特点：
1、多线程连接池模型
2、异步操作，支持为每个查询操作注册回调函数，类的非静态成员函数也支持注册
3、回调函数返回的查询结果为双层结构（行列），用户可以快速的转换成自定义的结构（数据库表结构映射）
4、代码总共两百余行，低耦合，通俗易懂

详情请见： Example.cpp 包含各个案例


简单介绍几个模块：
=====================================
线程池单例
核心成员：线程队列，任务队列，连接池对象

1、用户抛出任务时，会添加进任务队列
2、各个线程会从任务队列中取出相关任务，并从连接池取出空闲连接，通知其执行相关操作

=====================================
连接池
核心成员：连接队列，连接管理线程

1、连接队列包含多个连接对象，每次 mysql 操作会从队列 pop 出一个连接，执行完毕后再 push 进队列，无需重复创建和销毁连接。
2、当任务请求频繁，连接池的连接数量已经不满足需求时，管理线程会自动开辟新连接。
3、当任务请求量低，连接池的连接时常处于空闲状态时，管理线程会销毁相关连接。

=====================================
连接对象
基本增删改查操作
