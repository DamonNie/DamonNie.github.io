---
title: PHP面试题
top: false
cover: false
date: 2020-04-07 13:12:39
password: 20200407
summary: 保存下来，后面慢慢消化
tags:
 - 操作系统
 - 数据结构
 - 算法
 - nginx
 - php
 - redis
categories: 后端
---

建议不要去背面试题，因为大部分问题都会追问，是不是理解了一问就问得出来。面试题只是帮助我们了解自己技术方面的缺陷，背几个面试题对自己没有任何帮助。获取答案的方式我已经加上了。
* [操作系统（CSAPP/APUE）]( "")

* [PHP（php 源代码，主流 php 框架源代码）]( "")

* [mysql（高性能 mysql/mysql 技术内幕 innodb 储存引擎 / 数据库系统实现）]( "")

* [redis（redis 源代码）]( "")

* [nginx]( "")

* [数据结构]( "")

* [算法]( "")

##操作系统（CSAPP/APUE）[#](#fb7f1e "")

    
* IO 多路复用是什么？有哪些 api？
* select 和 epoll 的区别？水平触发和边缘触发的区别是啥？使用的时候需要注意什么？
* epoll 储存描述符的数据结构是什么？
* select 有描述符限制吗？是多少？
* 进程 / 线程 / 协程区别？go 和 swoole 的协程实现有啥区别？（分配资源的基本单位 / 运行和调度的基本单位 / 用户线程，M:N 模型和 N:1 模型）


##PHP（php 源代码，主流 php 框架源代码）[#](#33748b "")

    
* 描述一下 cli 模式下的几个生命周期？
* php-fpm 运行机制？（master 管理，worker 循环 accept）
* php-fpm 模式下，kill -9 master-pid，会怎么样？kill matser-pid 呢？（信号机制）
* 内存分配流程？为什么要这么设计？
* GC 的出现是为了解决什么问题？什么时候会触发 GC？说下大概流程
* php 里的数组是怎么实现的？（这里要注意下 php5 和 php7 实现的区别，优化了非常多）
* nginx 和 php-fpm 的通信机制？fast-cgi 和 cgi 区别？
* php-fpm 创建 worker 进程的规则是什么？不同场景下怎么选择？
* php 和 mysql 的通信机制？长链接和短链接啥区别？怎么实现的？连接池要怎么实现？
* swoole 协程的原理？（遇到阻塞时引发 php 栈和 c 栈的切换，细节可以参考下我的文章）
* 依赖注入是什么？如何实现的？能解决什么问题？（代码层面不再依赖具体实现，解耦）


##mysql（高性能 mysql/mysql 技术内幕 innodb 储存引擎 / 数据库系统实现）[#](#6544ed "")

    
* innodb 的索引组织方式？（聚簇索引必须要很清楚，注意 innodb 聚簇索引叶子结点保存的是完整数据，innodb 普通索引叶子保存的是记录的主键，myisam 索引叶子保存的是记录的位置 / 偏移量）
* B + 树的结构和插入细节？为什么主键一般都要自增？和 B 树什么区别？为什么索引要使用 B + 树不是 B 树也不是其他的平衡树？为什么 redis 可以用跳表？（关键词：页的分裂，随机 IO，缓存体系）
* 常见的优化（这里我就不展开了，主要考察覆盖索引查询和最左匹配，其实只要清楚 innodb 索引的结构，这些都不需要记忆，自然而然推导出来的）
* redolog/undolog/binlog 的区别？binlog 的几种格式？说下两阶段提交？
* 事务隔离级别和不同级别会出现的问题，innodb 默认哪个级别？MVCC 怎么实现的？快照读和当前读有啥区别？幻读的问题怎么解决？
* 死锁什么时候会出现？应用层应该怎么做避免死锁？mysql 是怎么处理死锁的呢？
* int 占多少字节？bigint 呢？int (3) 和 int (11) 有区别吗？可以往 int (3) 里存 1 亿吗？varchar 最长多少？
* sql 的执行流程（原始 sql-> 词法语法分析生成 AST-> 关系代数表达式（逻辑计划）-> 逻辑优化（谓词下推 / 常量传递）-> 物理查询优化（计算最佳 cost 路径，扫表还是使用索引，join 算法）-> 执行，仅做参考）


##redis（redis 源代码）[#](#864a3c "")

    
* sds 的结构是什么？为什么要存长度？跟 c 里的字符串有什么区别？（关键词：获取长度复杂度 O (1) 和 O (n)，二进制安全，保存 \0，跟 C 库字符串函数可以通用）
* hash 怎么实现的？怎么解决 hash 冲突？除了 hashTable 还有别的吗？
* zset 怎么实现的？跳表是怎么插入的？为什么选择跳表不用其他平衡二叉树？除了跳表还有别的吗？
* rehash 过程？会主动 rehash 吗？
* 用 redis 可以实现队列吗？有什么优点和缺点？
* 用 redis 怎么实现一个延时队列？
* rdb 和 aof 过程？rdb 为什么可以用创建子进程的方式进行？（这里考察一个 cow）这两种持久化方式会丢数据吗？
* redis 为什么快？（主要考察一个 IO 多路复用和单线程不加锁）
* 一致性哈希是什么？节点较少时数据分布不均匀怎么办？
* 简单说下几种 key 的淘汰策略，redis 里的 lru 算法，什么时候会触发？实现细节是什么？怎么保证淘汰合理的 key？
* lua 脚本的作用是什么？
* 缓存击穿 / 穿透 / 雪崩的处理策略


##nginx[#](#ee4340 "")

    
* LVS 和 Nginx 分别作用在 osi 哪一层？
* 负载均衡算法


##数据结构[#](#0fea7c "")

    
* 布隆过滤器，什么时候用？优点是什么？


##算法[#](#046a89 "")

    
* leetcode easy 级别的题目，具体我就不写了，难度比较低

