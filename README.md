# Java面试题目总结

最近求职中遇到的一些Java技术点

---

## redis

- 主要特点: 分布式、独占、持久化

- 数据类型：

    + string(字符串)：二进制安全，可以包含任何数据，如图片和序列化的对象，一个key最大存储512MB。 set key value/get key

    + hash(哈希)：键值对集合，特别适合存储对象，每个hash可以存储键值对数量：$ 2^{32} -1 $ （40多亿）。HMSET k1 v1 k2 v2/HGET key 

    + list(列表)：有序字符串列表，按照插入顺序排序，可以添加到头部(lpush)或者尾部(rpush)。

    + set(集合)：string类型的无序集合，通过hash表实现，因此最多存储$2^{32}-1$个成员，添加、删除、查找都是O(1)。sadd key member

    + zset(sorted set:有序集合)：和set类似，string元素的集合，并且不允许重复成员。但每个元素都会关联一个double类型的分数(score)，通过分数为集合中成员进行从小到大排序。zadd key score member


- 取top 1000

    - sort 命令
    
    - zset 

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

- 异步编程

    + 锁的种类

        - 公平锁/非公共锁：
        多个线程同时获取一把锁，分配的原则就是公平性。公平锁按照线程入队顺序来分配，没有插队现象；非公平锁允许插队，可以让优先级更高的线程优先获得锁

        - 共享锁/独占锁：
        一个锁是否只能由一个线程持有。共享锁允许多个线程同时持有一把锁，例如CountDownLatch；独占锁某一时刻只能被一个线程持有，例如synchronized内置锁

        - 可重入锁/不可重入锁
        可重入锁指的是ReentrantLock，一个线程获取锁之后，可以不释放，直接再次获取锁，获取多次后可以对应多次释放。即线程可以进入任何一个它已经拥有的锁同步着的代码块。
        不可重入锁指的是，线程获取锁后，不释放该锁，就不能再次获取该锁。

        - 读写锁
        读锁和写锁，多个线程可以同时获取多个读锁，当获取写锁时，必须释放所有读锁，读锁可以升级为写锁，写锁不能降级为读锁。读锁是可重入的。

        - 悲观锁/乐观锁

            + 悲观锁：
            假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。比如独占锁，就是一种悲观策略。

            + 乐观锁：
            假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。基于冲突检测的乐观并发策略，通俗的说就是先进行操作，如果没有其他线程争用共享数据，就操作成功。如果共享数据有争用，产生了冲突，那就采取其他的补偿措施(最常用的就是不断重试),这种乐观的并发策略的许多实现都不需要把线程挂起，因此这种同步操作被称为 非阻塞同步(Non-Blocking Synchronization)

    + synchronized 关键字

        - 如果作用的对象是非静态的，那么取得锁的是对象

        - 如果作用的对象是静态方法或者一个类，那么取得的锁是类，该类的所有对象同用一把锁

        - 实现同步需要系统很大开销，甚至造成死锁，尽量避免无意义的同步控制
    
    + wait()/notify()/notifyAll()

        - wait()和notify()必须要与synchronized(resource)一起使用，即Obj.wait(),Obj.notify()必须在synchronized(Obj){...}语句块中使用。

        - wait() 线程在获取对象锁后，主动释放CPU控制权，主动释放对象锁，同时本线程休眠，直到有其他线程调用对象的notify()被唤醒，然后继续执行。

        - notify() 对对象锁的释放操作，但不立刻释放CPU控制权，而是在相应的synchronized(){...}语句块执行结束后再自动释放锁。
    
    + Thread

        - sleep() 只释放cpu控制权，不释放锁，这点和wait()不相同

        - join() 让两个线程保持同步 例如：A、B两种进程，如果在B中调用了A.join()，则表示A线程执行完了才开始执行B线程

        - interrupt() 只有被Obj.wait()、Thread.join()和Thread.sleep()三种方法之一阻塞的时候，调用interrupt()才有效果。它会提前结束阻塞，然后抛出InterruptedException异常。Obj.wait()要小心锁的问题，wait()过程中调用Interrupt()会先重新获取它的锁，再抛出异常，获得之前还是会阻塞。

- Filter过滤器和Interceptor拦截器

- Servlet

    + 生命周期
    
    + 内置对象

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

- mvc

    + 组成

        - Model(模型)：处理应用程序数据逻辑的部分，通常负责在数据库中存取数据

        - View(视图)：处理应用程序数据显示的部分，通常是依据模型数据创建的

        - Controller(控制器)：处理应用程序中用户交互的部分，通常负责从视图读取数据，控制用户输入，并向模型发送数据

    + 意义：有助于管理复杂的应用程序，简化了分组开发

    + 优点

        - 耦合性低

        - 重用性高

        - 生命周期成本低

        - 部署快

        - 可维护性高

        - 有利于软件工程化管理

    + 缺点

        - 没有明确定义

        - 不适合小型，中等规模的应用

        - 增加系统结构和实现的复杂性

        - 视图与控制器间连接过于紧密

        - 视图对模型数据的低效率访问

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

- overload与override

    + overload 重载 相同名称、返回值但参数表不同的方法

    + override 重写 对父类方法的覆盖，参数表与父类一致

- dubbo

    - 分布式服务框架： 软负载均衡 额外提供监控中心Monitor和调用中心

    - 关注的层 Service层

    - 实现原理 rpc

    - 使用步骤

        + 引入Service层interface

        + 引入Domain层实体类

---

## docker

- 使用场景

- 打包到image

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

- webpack

    + 作用

- angular

---

## 简述最有成就感的技术的使用
