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

## dubbo

- 分布式服务框架： 软负载均衡 额外提供监控中心Monitor和调用中心

- 关注的层 Service层

- 实现原理 rpc

- 使用步骤
    + 引入Service层interface
    + 引入Domain层实体类

---

## 消息队列

- RabbitMQ

    + 消息模式 
        
        + 简单模式 1个生产者、1个消费者、1个队列
        + 工作队列模式Work Queue 一个生产者，多个消费者，每个消费者获取到的消息唯一，多个消费者只有一个队列
        + 发布/订阅模式 Publish/Subscribe 一个生产者发送的消息会被多个消费者获取。一个生产者、一个交换机、多个队列、多个消费者
        + 路由模式 生产者发送消息到交换机并且要指定路由key，消费者将队列绑定到交换机时需要指定路由key
        + 通配符模式 生产者P发送消息到交换机X，type=topic，交换机根据绑定队列的routing key的值进行通配符匹配；
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

- 单例模式（Singleton Pattern）
    
    + 确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例

- 工厂模式（Factory Pattern）

    + 定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类
    
    + 简单工厂模式：一个模块仅需要一个工厂类，没有必要把它产生出来，使用静态的方法

    + 多个工厂类：每个人种（具体的产品类）都对应了一个创建者，每个创建者独立负责创建对应的产品对象，非常符合单一职责原则

    + 代替单例模式：单例模式的核心要求就是在内存中只有一个对象，通过工厂方法模式也可以只在内存中生产一个对象

    + 延迟初始化：ProductFactory负责产品类对象的创建工作，并且通过prMap变量产生一个缓存，对需要再次被重用的对象保留

- 抽象工厂模式（Abstract Factory Pattern）
    
    + 为创建一组相关或相互依赖的对象提供一个接口，而且无须指定它们的具体类

- 模板方法模式（Template Method Pattern）
    
    + 定义一个操作中的算法的框架，而将一些步骤延迟到子类中。使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤
    
- 建造者模式（Builder Pattern）

- 代理模式（Proxy Pattern）

- 原型模式（Prototype Pattern）

- 中介者模式（Mediator Pattern）

- 命令模式（Command Pattern）

- 责任链模式（Chain of Responsibility Pattern）

- 装饰模式（Decorator Pattern）

- 策略模式（Strategy Pattern）

- 适配器模式（Adapter Pattern）

- 迭代器模式（Iterator Pattern）

- 组合模式（Composite Pattern）

- 观察者模式（Observer Pattern）

- 门面模式（Facade Pattern）

- 备忘录模式（Memento Pattern）

- 访问者模式（Visitor Pattern）

- 状态模式（State Pattern）

- 解释器模式（Interpreter Pattern）

- 享元模式（Flyweight Pattern）

- 桥梁模式（Bridge Pattern）

### 设计原则

- Single Responsibility Principle：单一职责原则
- Liskov Substitution Principle：里氏替换原则
- Dependence Inversion Principle：依赖倒置原则
- Open Closed Principle：开闭原则


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

    + hibernate中数据的状态

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
