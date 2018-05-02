# Java面试题目总结

最近求职中遇到的一些Java技术点

---

## redis

- 主要特点: 分布式、独占、持久化

- 取top 1000

    - sort 命令
    
    - sorted set 

- 使用场景

    - 分布式锁

    - 集群session共享

    - 存储token
    
    - 数据缓存

---

## 消息队列

- RabbitMQ

    + 消息模式 

        + 简单模式 1个生产者、1个消费者、1个队列
        
        + 工作队列模式Work Queue 一个生产者，多个消费者，每个消费者获取到的消息唯一，多个消费者只有一个队列
        
        + 发布/订阅模式 Publish/Subscribe 一个生产者发送的消息会被多个消费者获取。一个生产者、一个交换机、多个队列、多个消费者
        
        + 路由模式 生产者发送消息到交换机并且要指定路由key，消费者将队列绑定到交换机时需要指定路由key
        
        + 通配符模式 生产者P发送消息到交换机X，type=topic，交换机根据绑定队列的routing key的值进行通配符匹配
        
            > 符号#：匹配一个或者多个词 lazy.# 可以匹配 lazy.irs或者lazy.irs.cor
            
            > 符号*：只能匹配一个词 lazy.* 可以匹配 lazy.irs或者lazy.cor

    + exchange模式

        + Direct Exchange 不需要绑定exchange,使用默认的exchange,消息必须带routekey,通过routekey投递消息,没有找到对应routekey直接丢弃
        
        + Fanout Exchange 提前将exchange和queue绑定,然后发往指定的exchange的消息会沿着固定的绑定发送至固定queue,不需要带routekey
        
        + Topic Exchange exchange也和queue绑定,且一对多,根据routekey决定exhange进而确定queue.需要带routekey

    + 如何防止同一个节点失败后 消息重发又发回上次失败的节点
        
        + 保证代码幂等性,允许反复执行

    + 消息吞吐量

---

## 数据库

- 表关联

    + 几种join

    + left join 与 right join 与 inner join的区别

- having子句

- 事务四大特性: ACID

    + Atomicity 原子性

    + Consistency 一致性

    + Isolation 隔离性

    + Durability 持久性


- 事务隔离级别
    
    + Read Uncommit 读未提交 会出现脏读

    + Read Commit 读已提交 会出现不可重复读

    + Repeatable Read 重复读 会出现幻读

    + Serializable 序列化 最高隔离级别，锁各种操作


- 数据库范式：六种 上级范式肯定要满足下级范式

    + 第一范式(1NF): 数据库表中的每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性
    
    + 第二范式(2NF): 数据库表中的每个实例或行必须可以被唯一地区分。为实现区分通常需要为表加上一个列，以存储各个实例的唯一标识。
    
    + 第三范式(3NF): 每个非主属性都不传递依赖于R的候选键，则称R是第三范式的模式
    
    + 巴斯-科德范式(BCNF): 如果关系模型R是第一范式，且每个属性都不传递依赖于R的候选键，那么称R为BCNF的模式。
    
    + 第四范式(4NF): 设R是一个关系模型，D是R上的多值依赖集合。如果D中存在凡多值依赖X->Y时，X必是R的超键，那么称R是第四范式的模式
    
    + 第五范式(5NF,又称完美范式): 关系模式R依赖均由R候选码所隐含。

- mysql

- 分组取topN

---

## 设计模式: 23种


### 三大类

#### 创建型：5种

- 工厂模式（Factory Pattern）
 
- 抽象工厂模式（Abstract Factory Pattern）

- 单例模式（Singleton Pattern）

- 建造者模式（Builder Pattern）

- 原型模式（Prototype Pattern）

#### 结构型: 7种

- 适配器模式（Adapter Pattern）

- 装饰器模式（Decorator Pattern）

- 代理模式（Proxy Pattern）

- 门面模式（Facade Pattern）

- 桥梁模式（Bridge Pattern）

- 组合模式（Composite Pattern）

- 享元模式（Flyweight Pattern）

#### 行为型: 11种

- 策略模式（Strategy Pattern）

- 模板方法模式（Template Method Pattern）

- 观察者模式（Observer Pattern）

- 迭代器模式（Iterator Pattern）

- 责任链模式（Chain of Responsibility Pattern）

- 命令模式（Command Pattern）

- 备忘录模式（Memento Pattern）

- 状态模式（State Pattern）

- 访问者模式（Visitor Pattern）

- 中介者模式（Mediator Pattern）

- 解释器模式（Interpreter Pattern）


### 设计原则 六大原则 

*总原则 - 开闭原则(Open Closed Principle)*

- 单一职责原则(Single Responsibility Principle)

- 里氏替换原则(Liskov Substitution Principle)

- 依赖倒置原则(Dependence Inversion Principle)

- 接口隔离原则(Interface Segregation Principle)

- 迪米特法则(Demeter Principle最少知道法则)

- 合成复用原则(Composite Reuse Principle)

---

## Java

- 反射

- 泛型

- 面向切面编程(AOP)

- Filter过滤器和Interceptor拦截器

- Servlet

    + 生命周期

    + 详情

- 文件流

- Socket

- Hibernate 

    + N+1问题

    + NamedQuery

    + Query

    + session.flush()

    + hibernate中对象的状态
        
        - 临时状态（Transient）：刚刚使用new语句创建，还没有被持久化，不处于Session的缓存中。处于临时状态的状态的Java对象被称为临时对象
        
        - 持久化状态（Persistent）：已经被持久化，加入到Session的缓存中。处于持久化状态的Java对象被称为持久化对象

        - 游离状态（Detached）：已经被持久化，但不处于session的缓存中。处于游离状态的Java对象被称为游离对象。

- mybatis

    + $ 和 # 的区别
    
    + namespace的作用

- spring

    + 简述spring

    + springMvc

    + springboot

        - 介绍

        - 核心实现方式 

            + conditionOnClass/conditionOnMissingBean

            + META.INFO 中的factories 

    + spring 事务

- EhCache

    + 使用场景

    + 特点

- dubbo

    - 分布式服务框架： 软负载均衡 额外提供监控中心Monitor和调用中心

    - 关注的层 Service层

    - 实现原理 rpc

    - 使用步骤

        + 引入Service层interface

        + 引入Domain层实体类

---

## Linux

- scp

- ps

- tail

- top

- mv

- nohup

---

## Kettle

- 使用场景

- 增量抽取

- 数据吞吐量

---

## 前端

- react

  + state

  + 双向绑定 Object.defineProperty()

- angular

---

## 简述最有成就感的技术的使用
