# 面试知识点总结
* 笔记向，不定期更新。
* 没有太多需要理解的东西，需要理解的部分自行学习。
* 自己总结，如果有错的地方欢迎指正。

# 数据库
1. 索引相关
    * 什么是索引  
    索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息。
    * 什么时候使用索引  
    能够提升效率的时候使用索引。  
        * 在作为主键的列上，唯一性约束的列（不过大部分数据库会自动加索引）；
        * 在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度；
        * 在经常需要排序的列上创建索引，这样查询可以利用索引的排序，加快排序查询时间；
        * 在经常使用在WHERE子句中的列上面创建索引，加快条件的判断速度。
    * 什么时候不能使用索引  
    不能够提升效率的时候不使用索引，例如：该列很少进行查找或排序操作
    * 什么时候索引会失效
        * 查询的结果太大，性能消耗大于使用索引
        * where里面存在!=
        * 模糊查询的时候，通配符在第一个
        * where查询null值
        
3. 数据库连接方法
    1. DriverManager
    ```java
    Connection conn = DriverManager.getConnection("连接URL","用户名","密码");
    PreparedStatement ps = conn.prepareStatement(sql);
    ps.setInt(index, value);
    ps.executeUpdate()
    //或者
    Statement stmt=conn.createStatement();
    stmt.executeUpdate(sql)...
    ```
    2. 用JDBC连接池，如C3P0

1. 事务（Transaction）
    * 事务是是一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位。
    * 事务的四个属性 
        * 原子性 Atomic  
        就是事务应作为一个工作单元,事务处理完成，所有的工作要么都在数据库中保存下来，要么完全回滚，全部不保留 
        * 一致性 Consistent  
        事务完成或者撤销后，都应该处于一致的状态
        * 隔离性 Isolated  
        多个事务同时进行，它们之间应该互不干扰.应该防止一个事务处理其他事务也要修改的数据时，不合理的存取和不完整的读取数据 
        * 永久性 Durable  
        事务提交以后，所做的工作就被永久的保存下来 

1. 什么是存储过程？有哪些优缺点？  
存储过程是一些预编译的SQL语句。  
更加直白的理解：存储过程可以说是一个记录集，它是由一些SQL语句组成的代码块，这些SQL语句代码像一个方法一样实现一些功能（对单表或多表的增删改查），然后再给这个代码块取一个名字，在用到这个功能的时候调用他就行了。
    * 存储过程是一个预编译的代码块，执行效率比较高
    * 一个存储过程替代大量SQL语句 ，可以降低网络通信量，提高通信速率
    * 可以一定程度上确保数据安全

1. 简单说一说drop、delete与truncate这三种删除的区别  
   * delete和truncate只删除表的数据不删除表的结构
   * 速度,一般来说: drop> truncate >delete 
   * delete语句是dml,这个操作会放到rollback segement中,事务提交之后才生效；如果有相应的trigger，执行的时候将被触发。truncate，drop是ddl, 操作立即生效，原数据不放到rollback segment中，不能回滚，操作不触发trigger。
 
   使用场景
   * 不再需要一张表的时候，用drop
   * 想删除部分数据行时候，用delete，并且带上where子句
   * 保留表而删除所有数据的时候用truncate

1. 超键、候选键、主键、外键分别是什么？
    * 超键：在关系中能唯一标识元组的属性集称为关系模式的超键。
    * 候选键：没有冗余元素的超键。
    * 主键：可以唯一地标识表中的某一条记录的键（和候选键一样，只不过一个表只能有一个主键）
    * 外键：在一个表中存在的另一个表的主键称此表的外键。

1. 什么是视图？以及视图的使用场景有哪些？  
    视图是一种虚拟的表，具有和物理表相同的功能。可以对视图进行增，改，查，操作，试图通常是有一个表或者多个表的行或列的子集。对视图的修改不影响基本表。它使得我们获取数据更容易，相比多表查询。
    * 只暴露部分字段给访问者，所以就建一个虚表，就是视图。
    * 查询的数据来源于不同的表，而查询者希望以统一的方式查询，这样也可以建立一个视图，把多个表查询结果联合起来，查询者只需要直接从视图中获取数据，不必考虑数据来源于不同表所带来的差异

1. 三大范式
    * 第一范式（1NF）：数据库表中的字段都是单一属性的，不可再分；
    * 第二范式（2NF）：数据库表中不存在字段对任一候选关码段的部分函数依赖；
    * 第三范式（3NF）：数据表中如果不存在字段对任一候选码字段的传递函数依赖。

1. DML、DDL、DCL区别
* DML(Data Manipulation Language，数据操作语言)  
  如：SELECT、UPDATE、INSERT、DELETE，这4条命令是用来对数据库里的数据进行操作的语言
* DDL(Data Definition Language，数据定义语言)  
  主要的命令有CREATE、ALTER、DROP等，DDL主要是用在定义或改变表的结构，数据类型，表之间的链接和约束等初始化工作上，他们大多在建立表时使用
* DCL(Data Control Language，数据控制语言)  
  用来设置或更改数据库用户或角色权限的语句，包括grant,deny,revoke等语句。

1. sql的join
    * http://www.w3school.com.cn/sql/sql_join.asp
    
1. B-tree
    * 假设B-tree为m阶
    * 树中每个结点至多有m个孩子；
    * 除根结点和叶子结点外，其它每个结点至少有有ceil(m / 2)个孩子；
    * 若根结点不是叶子结点，则至少有2个孩子(特殊情况：没有孩子的根结点)
    * 【不懂】所有叶子结点都出现在同一层，叶子结点不包含任何关键字信息(可以看做是外部结点或查询失败的结点，实际上这些结点不存在，指向这些结点的指针都为null)；
    * 每个非终端结点中包含有n个关键字信息: (P0，K1，P1，K2，P2，......，Kn，Pn)。其中：
        * Ki (i=1...n)为关键字，且关键字按顺序排序K(i-1)< Ki。
        * Pi为指向子树根的结点，且指针P(i-1)指向子树中所有结点的关键字均小于Ki，但都大于K(i-1)。
        * 关键字的个数n必须满足： ceil(m / 2)-1 <= n <= m-1。
    * 插入过程：
    
# 框架
### Spring
1. spring 的优点？
    * 降低了组件之间的耦合性;
    * 提供了众多服务，如事务管理等 
    * 容器提供单例模式支持 
    * 容器提供了AOP技术，利用它很容易实现运行期监控等功能 
    * spring对于主流的应用框架提供了集成支持，如hibernate等 
    * spring属于低侵入式设计，代码的污染极低 
    * 独立于各种应用服务器 
    * spring的IOC机制降低了业务对象替换的复杂性 
    * Spring的高度开放性，并不强制应用完全依赖于Spring，开发者可以自由选择spring的部分或全部 

2. IOC
    * 在Java开发中，IoC意味着将你设计好的类交给系统去控制，而不是在你的类内部控制。  
    * 举个例子：以前的买东西是你和商家直接交易，而IOC之后，变成了你和淘宝交易，淘宝和商家交易。  
    * 我觉得就是在一个map里面存上类的具体路径，然后反射实现的
3. AOP  
这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。一般都用于日志等地方。

### Spring MVC
### Mybatis
### Hibernate
1. 优化Hibernate的7大措施【不懂。。】
    * 尽量使用many-to-one，避免使用单项one-to-many
    * 灵活使用单向one-to-many
    * 不用一对一，使用多对一代替一对一
    * 配置对象缓存，不使用集合缓存
    * 一对多使用Bag 多对一使用Set
    * 继承使用显示多态 HQL:from object polymorphism="exlicit" 避免查处所有对象
    * 消除大表，使用二级缓存
    
# 计算机网络
1. http
    1. http和https区别  
    HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全。  
    HTTPS和HTTP的区别主要如下：
        * https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
        * http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
        * http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
        * http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
    1. 说一下什么是Http协议？  
    对器客户端和服务器端之间数据传输的格式规范，格式简称为“超文本传输协议”。
    2. 什么是Http协议无状态协议？怎么解决Http协议无状态协议？（曾经去某创业公司问到）
        * 无状态协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息
        * 无状态协议解决办法： 通过1、Cookie 2、通过Session会话保存。
    3. 说一下Http协议中302状态  
    http协议中，返回状态码302表示重定向。这种情况下，服务器返回的头部信息中会包含一个 Location 字段，内容是重定向到的url
    4. Http协议有什么组成？
        1. 请求报文 包含三部分：
            * 请求行：包含请求方法、URI、HTTP版本信息
            * 请求首部字段
            * 请求内容实体
        2. 响应报文 包含三部分：
            * 状态行：包含HTTP版本、状态码、状态码的原因短语
            * 响应首部字段
            * 响应内容实体
    5. Http协议中有那些请求方式？
        * GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器
        * POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
        * PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
        * HEAD： 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。
        * DELETE：删除文件，与PUT方法相反，删除对应URI位置的文件。
        * OPTIONS：查询相应URI支持的HTTP方法。
    6. Http协议中Http1.0与1.1区别？
        * 在http1.0中，当建立连接后，客户端发送一个请求，服务器端返回一个信息后就关闭连接，当浏览器下次请求的时候又要建立连接，显然这种不断建立连接的方式，会造成很多问题。
        * 在http1.1中，引入了持续连接的概念，通过这种连接，浏览器可以建立一个连接之后，发送请求并得到返回信息，然后继续发送请求再次等到返回信息，也就是说客户端可以连续发送多个请求，而不用等待每一个响应的到来。
    8. get与post请求区别？
        * get重点在从服务器上获取资源，post重点在向服务器发送数据；
        * get传输数据是通过URL请求，post传输数据通过Http的post机制，将字段与对应值封存在请求实体中发送给服务器，这个过程对用户是不可见的；
        * Get传输的数据量小，因为受URL长度限制，但效率较高；Post可以传输大量数据，所以上传文件时只能用Post方式；
        * get是不安全的，因为URL是可见的，可能会泄露信息；post较get安全性较高；
    10. 常见Http协议状态？
        * 1**	信息，服务器收到请求，需要请求者继续执行操作
        * 200 - 请求成功
        * 301 - 资源（网页等）被永久转移到其它URL
        * 404 - 请求的资源（网页等）不存在
        * 500 - 内部服务器错误
    13. Http优化
        * 利用负载均衡优化和加速HTTP应用
        * 利用HTTP Cache来优化网站
    1. http缓存机制  
    【待补充】
    1. DNS解析的方法  
    【待补充】
    1. 跨域方法  
    【待补充】  
    1. xss, csrf  
    【待补充】
2. osi七层  
    ![osi](https://raw.githubusercontent.com/llysrv/Interview/master/img/OSI.gif)

3. TCP  
三次握手  
![三次握手](https://raw.githubusercontent.com/llysrv/Interview/master/img/%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.png)  
四次断开  
因为之前存在没有传送完的数据，所以要等待。  
![四次断开](https://raw.githubusercontent.com/llysrv/Interview/master/img/%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B.jpg)  
小故事  
三次握手：  
    * A：喂，你听得到吗？  
    * B：我听得到呀，你听得到我吗？  
    * A：我能听到你，balabala……  
四次挥手：  
    * A：喂，我不说了。
    * B：我知道了。等下，上一句还没说完。balabala…..  
    * B：好了，说完了，我也不说了。  
    * A：我知道了。  
    * A等待2MSL,保证B收到了消息,否则重说一次”我知道了”(如果没收到将进行重传)  
4. TCP与UDP之间的区别
    1. 基于连接vs无连接  
    TCP是面向连接的协议，而UDP是无连接的协议。
    2. 可靠性不同  
        * TCP提供交付保证，这意味着一个使用TCP协议发送的消息是保证交付给客户端的。
        * UDP是不可靠的，它不提供任何交付的保证。一个数据报包在运输途中可能会丢失。
    3. 有序性
        * TCP保证了消息的有序性，该消息将以从服务器端发出的同样的顺序发送到客户端，
        * UDP不提供任何有序性或序列性的保证，数据包将以任何可能的顺序到达。
    4. 数据边界  
        TCP不保存数据的边界，而UDP保证。
        在传输控制协议，数据以字节流的形式发送，
        并没有明显的标志表明传输信号消息（段）的边界。
        在UDP中，数据包单独发送的，只有当他们到达时，才会再次集成。包有明确的界限来哪些包已经收到，这意味着在消息发送后，在接收器接口将会有一个读操作，来生成一个完整的消息。虽然TCP也将在收集所有字节之后生成一个完整的消息，但是这些信息在传给传输给接受端之前将储存在TCP缓冲区，以确保更好的使用网络带宽
    5. 速度  
    因为TCP必须创建连接，以保证消息的可靠交付和有序性，他需要做比UDP多的多。
    6. 重量级vs轻量级  
    由于上述的开销，TCP被认为是重量级的协议，而与之相比，UDP协议则是一个轻量级的协议。因为UDP传输的信息中不承担任何间接创造连接，保证交货或秩序的的信息。这也反映在用于承载元数据的头的大小。
    7. 头大小  
    TCP具有比UDP更大的头。
    8. 流量控制  
        * TCP有流量控制。在任何用户数据可以被发送之前，TCP需要三数据包来设置一个套接字连接。TCP处理的可靠性和拥塞控制。
        * UDP不能进行流量控制。

1. https://www.nowcoder.com/test/question/done?tid=9501873&qid=46337#summary

# Java

### JVM

1. Java GC如何判断对象是否为垃圾  
根搜索算法，根：
    * 虚拟机栈中引用的对象（本地变量表）
    * 方法区中静态属性引用的对象
    * 方法区中常量引用的对象
    * 本地方法栈中引用的对象（Native对象）

16. 为什么要设置工作内存和主内存  
    缓存加速，相当于是内存和硬盘的关系。
17. 当发现虚拟机频繁GC时应该怎么办？  
    【待补充】  
2. 请描述java的内存分区  
    【待补充】  
3. 请描述java的对象生命周期，以及对象的访问？   
    【待补充】  
1. JVM调优  
    【待补充】  
### 并发
1. 乐观锁，悲观锁
    * 乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。
    * 悲观锁指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，
    因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制
1. volatile关键字的作用  
    保证变量的可见性
1. Callable与Future
    * CallableAndFutureTask，利用Thread启动单线程
    ```java
    void testCallableAndFutureTask() throws InterruptedException, ExecutionException {
        FutureTask<Integer> future = new FutureTask<>(new Callable<Integer>() { //后面用lambda了
            @Override
            public Integer call() throws Exception {
                return new Random().nextInt(100);
            }
        });
        new Thread(future).start();
        Thread.sleep(5000);// 可能做一些事情
        System.out.println(future.get());
    }
    ```
    * CallableAndFuture，利用线程池，启动单线程
    ```java
    void testCallableAndFuture() throws InterruptedException, ExecutionException {
        ExecutorService threadPool = Executors.newSingleThreadExecutor();
        Future<Integer> future = threadPool.submit(() -> new Random().nextInt(100));
            Thread.sleep(5000);// 可能做一些事情
            System.out.println(future.get());
    }
    ```

    * CallableAndFuture，利用线程池，启动单多线程
    ```java
    void multipleReturnValues() throws InterruptedException, ExecutionException {
        ExecutorService threadPool = Executors.newCachedThreadPool();
        CompletionService<Integer> cs = new ExecutorCompletionService<>(threadPool);
        for (int i = 1; i < 5; i++) {
            final int taskID = i;
            cs.submit(() -> taskID);
        }
        // 可能做一些事情
        for (int i = 1; i < 5; i++) {
                System.out.println(cs.take().get());
        }
    }
    ```
    * 和Runnable接口中的run()方法不同, Callable接口中的call()方法是有返回值的
1. CountDownLatch
    * 先看用法
    ```java
     public static void main(String[] args) throws InterruptedException {
         CountDownLatch latch = new CountDownLatch(2);//两个工人的协作  
         Worker worker1 = new Worker("张三", latch);
         Worker worker2 = new Worker("李四", latch);
         worker1.start();
         worker2.start();
         latch.await();
         System.out.println("all work done.");
     }
 
     static class Worker extends Thread {
         String workerName;
         CountDownLatch latch;
 
         Worker(String workerName, CountDownLatch latch) {
             this.workerName = workerName;
             this.latch = latch;
         }
 
         public void run() {
             try {
                 System.out.println("Worker " + workerName + " do work begin.");
                 Thread.sleep(1000);
                 System.out.println("Worker " + workerName + " do work complete");
             } catch (InterruptedException e) {
                 e.printStackTrace();
             } finally {
                 latch.countDown();//工人完成工作，计数器减一  
             }
         }
     }   
    ```
    * 主线程必须在启动其他线程后立即调用CountDownLatch.await()方法。这样主线程的操作就会在这个方法上阻塞，直到其他线程完成各自的任务。
    * CountDownLatch只有await()和countDown()方法，分别表示等待和减一
1. CyclicBarrier  
    * CyclicBarrier简单的理解就是内存屏障
    * CyclicBarrier初始化时规定一个数目，然后计算调用了CyclicBarrier.await()进入等待的线程数。当线程数达到了这个数目时，所有进入等待状态的线程被唤醒并继续。 
    * CyclicBarrier初始时还可带一个Runnable的参数， 此Runnable任务在CyclicBarrier的数目达到后，所有其它线程被唤醒前被执行。
    * 具体见代码
    ```java
    class TestCyclicBarrier {
        private static final int THREAD_NUM = 5;
    
        public static void main(String[] args) {
            //当所有线程到达barrier时执行，这里的lambda是Runnable(可选参数)
            CyclicBarrier cb = new CyclicBarrier(THREAD_NUM, () -> System.out.println("Inside Barrier"));
    
            for (int i = 0; i < THREAD_NUM; i++) {
                new Thread(() -> {
                    System.out.println("Worker's waiting");
                    //线程在这里等待，直到所有线程都到达barrier。
                    try {
                        cb.await();
                    } catch (InterruptedException | BrokenBarrierException e) {
                        e.printStackTrace();
                    }
                    System.out.println("ID:" + Thread.currentThread().getId() + " Working");
                }).start();
            }
        }
    }
    /* 执行结果
    Worker's waiting
    Worker's waiting
    Worker's waiting
    Worker's waiting
    Worker's waiting
    Inside Barrier
    ID:15 Working
    ID:14 Working
    ID:12 Working
    ID:16 Working
    ID:13 Working*/
    ```

1. 阻塞队列BlockingQueue
    * 就是一个带阻塞功能的队列
    * 放入数据：
        * offer(anObject):如果BlockingQueue可以容纳, 则返回true,否则返回false.
        * offer(E o, long timeout, TimeUnit unit): 可以设定等待的时间，如果在指定的时间内，还不能往队列中，则返回失败。
        * put(anObject): 如果BlockQueue没有空间,则调用此方法的线程被阻塞，直到BlockingQueue里面有空间再继续.
    * 获取数据：
        * poll(): 若不能立即取出返回null;
        * poll(long timeout, TimeUnit unit): 如果在指定时间内，队列一旦有数据可取，则立即返回队列中的数据，否则返回null
        * take():若BlockingQueue为空， 则进入等待状态直到BlockingQueue有新的数据被加入; 
        * drainTo(Collection<? super E> c, int maxElements):一次性从BlockingQueue获取所有可用的数据对象（还可以指定获取数据的个数，第二个参数可选），通过该方法，可以提升获取数据效率；不需要多次分批加锁或释放锁。
1. 锁（lock）和监视器（monitor）有什么区别？
    * 锁是为了实现监视器排他性的一种手段
1. 怎么检测一个线程是否持有对象监视器
    * Thread类提供了一个holdsLock(Object obj)方法
    * 当且仅当对象obj的监视器被某当前线程持有的时候才会返回true，注意这是一个static方法.
1. Executor框架 【待学习】
    * 内容有点多，有空整理
1. Java编程写一个会导致死锁的程序
    ```Java
    class DeadLock {
        private static final String obj1 = "obj1";
        private static final String obj2 = "obj2";
    
        public static void main(String[] args) {
            ExecutorService es = Executors.newFixedThreadPool(2);
            es.submit(() -> {
                System.out.println("Lock1 running");
                while (true) {
                    synchronized (DeadLock.obj1) {
                        System.out.println("Lock1 lock obj1");
                        Thread.sleep(3000);//获取obj1后先等一会儿，让Lock2有足够的时间锁住obj2
                        synchronized (DeadLock.obj2) {
                            System.out.println("Lock1 lock obj2");
                        }}}});
    
            es.submit(() -> {
                System.out.println("Lock1 running");
                while (true) {
                    synchronized (DeadLock.obj2) {
                        System.out.println("Lock2 lock obj2");
                        Thread.sleep(3000);
                        synchronized (DeadLock.obj1) {
                            System.out.println("Lock2 lock obj1");
                        }}}});
        }
    }
    ```
1. 怎么唤醒一个阻塞的线程
    * 如果线程是因为调用了wait()、sleep()或者join()方法而导致的阻塞，可以中断线程，并且通过抛出InterruptedException来唤醒它；
    * 如果线程遇到了IO阻塞，无能为力，因为IO是操作系统实现的，Java代码并没有办法直接接触到操作系统。
1. 什么是多线程的上下文切换
    * CPU控制权由一个已经正在运行的线程切换到另外一个就绪并等待获取CPU执行权的线程的过程。   
1. 什么是自旋
    * 让等待锁的线程不要被阻塞，而是在synchronized的边界做忙循环，这就是自旋。
1. 什么是CAS
    * CAS，全称为Compare and Set，即比较-设置。
    * 假设有三个操作数：内存值V、旧的预期值A、要修改的值B，
    * 当且仅当预期值A和内存值V相同时，才会将内存值修改为B并返回true，
    * 否则什么都不做并返回false。当然CAS一定要volatile变量配合，这样才能保证每次拿到的变量是主内存中最新的那个值
1. 什么是AQS【待学习】
    * http://www.cnblogs.com/waterystone/p/4920797.html
    * 内容较多，以后再学
    * 简单说一下AQS，AQS全称为AbstractQueuedSynchronizer，翻译过来应该是抽象队列同步器。
    * 如果说java.util.concurrent的基础是CAS的话，那么AQS就是整个Java并发包的核心了，ReentrantLock、CountDownLatch、Semaphore等等都用到了它。
    * AQS实际上以双向队列的形式连接所有的Entry，比方说ReentrantLock，所有等待的线程都被放在一个Entry中并连成双向队列，前面一个线程使用ReentrantLock好了，则双向队列实际上的第一个Entry开始运行。
    * AQS定义了对双向队列所有的操作，而只开放了tryLock和tryRelease方法给开发者使用，开发者可以根据自己的实现重写tryLock和tryRelease方法，以实现自己的并发功能。
1. Semaphore有什么作用
    ```Java
    class SemaphoreTest {
        public static void main(String[] args) {
            ExecutorService exec = Executors.newCachedThreadPool();
            // 只能5个线程同时访问 
            final Semaphore semp = new Semaphore(5);
            for (int index = 0; index < 20; index++) {
                final int NO = index;
                exec.execute(() -> {
                    try {
                        // 获取许可 
                        semp.acquire();
                        System.out.println("Accessing: " + NO);
                        Thread.sleep((long) (Math.random() * 10000));
                        semp.release();
                    } catch (InterruptedException ignored) {
                    }
                });
            }
            exec.shutdown();
        }
    }
    ```
    * Java中的Semaphore是一种新的同步类，它是一个计数信号。
    * 从概念上讲，信号量维护了一个许可集合。如有必要，在许可可用前会阻塞每一个 acquire()，然后再获取该许可。每个 release()添加一个许可，从而可能释放一个正在阻塞的获取者。
    * 但是，不使用实际的许可对象，Semaphore只对可用许可的号码进行计数，并采取相应的行动。
    * 信号量常常用于多线程的代码中，比如数据库连接池。
1. 线程池的使用场景
    1. 高并发、任务执行时间短的业务：线程池线程数可以设置为CPU核数+1，减少线程上下文的切换
    2. 并发不高、任务执行时间长的业务要区分开看：
        * 假如是业务时间长集中在IO操作上，也就是IO密集型的任务，因为IO操作并不占用CPU，所以不要让所有的CPU闲下来，可以加大线程池中的线程数目，让CPU处理更多的业务
        * 假如是业务时间长集中在计算操作上，也就是计算密集型任务，这个就没办法了，和（1）一样吧，线程池中的线程数设置得少一些，减少线程上下文的切换
    3. 并发高、业务执行时间长：解决这种类型任务的关键不在于线程池而在于整体架构的设计，看看这些业务里面某些数据是否能做缓存是第一步，增加服务器是第二步，至于线程池的设置，设置参考（2）。最后，业务执行时间长的问题，也可能需要分析一下，看看能不能使用中间件对任务进行拆分和解耦。
1. Java中什么是竞态条件？ 
    * 计算的正确性取决于多个线程的交替执行时序时，就会发生竞态条件。
1. Java中如何停止一个线程？
    * 使用退出标志，使线程正常退出，也就是当run方法完成后线程终止。
    * 使用stop方法强行终止，但是不推荐这个方法，因为stop和suspend及resume一样都是过期作废的方法。
    * 使用interrupt方法中断线程。
1. 一个线程运行时发生异常会怎样？
    * 简单的说，如果异常没有被捕获该线程将会停止执行。
    * Thread.UncaughtExceptionHandler是用于处理未捕获异常造成线程突然中断情况的一个内嵌接口。当一个未捕获异常将造成线程中断的时候JVM会使用Thread.getUncaughtExceptionHandler()来查询线程的UncaughtExceptionHandler并将线程和异常作为参数传递给handler的uncaughtException()方法进行处理。

1. Java中interrupted 和 isInterrupted方法的区别？
    * interrupted() 和 isInterrupted()的主要区别是前者会将中断状态清除而后者不会。
    * Java多线程的中断机制是用内部标识来实现的，调用Thread.interrupt()来中断一个线程就会设置中断标识为true。
    * 当中断线程调用静态方法Thread.interrupted()来检查中断状态时，中断状态会被清零。
    * 而非静态方法isInterrupted()用来查询其它线程的中断状态且不会改变中断状态标识。
    * 简单的说就是任何抛出InterruptedException异常的方法都会将中断状态清零。无论如何，一个线程的中断状态有有可能被其它线程调用中断来改变。
1. 为什么你应该在循环中检查等待条件?
    * http://www.tengleitech.com/archives/107092
1.  Java中的同步集合与并发集合有什么区别？
    * http://www.tengleitech.com/archives/107098
1.  如何写代码来解决生产者消费者问题？
    * http://www.tengleitech.com/archives/107116
1.  如何避免死锁？
    * 死锁是指两个或两个以上的进程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去。这是一个严重的问题，因为死锁会让你的程序挂起无法完成任务，死锁的发生必须满足以下四个条件：
        * 互斥条件：一个资源每次只能被一个进程使用。
        * 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
        * 不剥夺条件：进程已获得的资源，在末使用完之前，不能强行剥夺。
        * 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。
    * 避免死锁最简单的方法就是阻止循环等待条件，将系统中所有的资源设置标志位、排序，规定所有的进程申请资源必须以一定的顺序（升序或降序）做操作来避免死锁。
1. 有三个线程T1，T2，T3，怎么确保它们按顺序执行？
    * http://www.tengleitech.com/archives/107158
1. Thread类中的yield方法有什么作用？
    * Yield方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃CPU占用而不能保证使其它线程一定能占用CPU，执行yield()的线程有可能在进入到暂停状态后马上又被执行。
1. 写出3条你遵循的多线程最佳实践
    * 给你的线程起个有意义的名字。  
    这样可以方便找bug或追踪。OrderProcessor, QuoteProcessor or TradeProcessor 这种名字比 Thread-1. Thread-2 and Thread-3 好多了，给线程起一个和它要完成的任务相关的名字，所有的主要框架甚至JDK都遵循这个最佳实践。
    * 避免锁定和缩小同步的范围  
    锁花费的代价高昂且上下文切换更耗费时间空间，试试最低限度的使用同步和锁，缩小临界区。因此相对于同步方法我更喜欢同步块，它给我拥有对锁的绝对控制权。
    * 多用同步类少用wait 和 notify  
    首先，CountDownLatch, Semaphore, CyclicBarrier 和 Exchanger 这些同步类简化了编码操作，而用wait和notify很难实现对复杂控制流的控制。其次，这些类是由最好的企业编写和维护在后续的JDK中它们还会不断优化和完善，使用这些更高等级的同步工具你的程序可以不费吹灰之力获得优化。
    * 多用并发集合少用同步集合  
    这是另外一个容易遵循且受益巨大的最佳实践，并发集合比同步集合的可扩展性更好，所以在并发编程时使用并发集合效果更好。如果下一次你需要用到map，你应该首先想到用ConcurrentHashMap。我的文章Java并发集合有更详细的说明。    
1.  Java中的fork join框架是什么？
    * fork join框架是JDK7中出现的一款高效的工具，Java开发人员可以通过它充分利用现代服务器上的多处理器。它是专门为了那些可以递归划分成许多子模块设计的，目的是将所有可用的处理能力用来提升程序的性能。fork join框架一个巨大的优势是它使用了工作窃取算法，可以完成更多任务的工作线程可以从其它线程中窃取任务来执行。
1. Java线程池中submit() 和 execute()方法有什么区别？
    * 两个方法都可以向线程池提交任务，
    * execute()方法的返回类型是void，它定义在Executor接口中, 
    * 而submit()方法可以返回持有计算结果的Future对象，它定义在ExecutorService接口中，它扩展了Executor接口，
    * 其它线程池类像ThreadPoolExecutor和ScheduledThreadPoolExecutor都有这些方法。

### 其他
1. 优化反射性能
    * 利用缓存
    * 对Field、Method、Constructor进行setAccessible(true);

11. 什么时候用接口、什么时候用抽象类 
    * 表达is-a关系的时候用抽象类
    * 表达like-a关系的时候用接口
    
12. List的实现类
    * LinkedList 
    * ArrayList 默认自动扩容1.5倍
    
13. Map的实现类
    * HashMap  
    先hash，hash冲突的用链表连起来（jdk1.8 当链表长度超过8时，链表变成红黑树）
    * TreeMap  
    红黑树
    * LinkedHashMap  
    默认按照插入排序，  
    accessOrder为true时，记录访问顺序，最近访问的放在最后
    * hashTable 与 concurrentHashMap 区别
        * HashTable每次同步执行的时候都要锁住整个结构
        * ConcurrentHashMap锁的方式是稍微细粒度的，ConcurrentHashMap里面的操作只锁当前数据所在的桶

1. Servlet的生命周期，是否是线程安全的？  
    1. Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：
        * Servlet 通过调用 init () 方法进行初始化。
        * Servlet 调用 service() 方法来处理客户端的请求。  
        service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。
        * Servlet 通过调用 destroy() 方法终止（结束）。
        * 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。
    2. 不是线程安全的，因为Servlet只会被实例化一次。如果Servlet中含有可变的域

1. Java表达式转型规则由低到高转换
    * 所有的byte,short,char型的值将被提升为int型；
    * 如果有一个操作数是long型，计算结果是long型；
    * 如果有一个操作数是float型，计算结果是float型；
    * 如果有一个操作数是double型，计算结果是double型；
    * 被fianl修饰的变量不会自动改变类型，当2个final修饰相操作时，结果会根据左边变量的类型而转化。

# 操作系统
1. 进程和线程的区别  
每个进程拥有自己的一套变量，而线程之间则共享数据的。
    
3. 进程的通信方式  【待理解】
    * 管道：管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。
    * 信号量：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。
    * 消息队列：消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。
    * 共享内存：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。
    * 套接字：套解口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同及其间的进程通信。

4. 缓冲区溢出
    * 缓冲区溢出是指当计算机向缓冲区填充数据时超出了缓冲区本身的容量，溢出的数据覆盖在合法数据上。  
    * 危害有以下两点：
        * 程序崩溃，导致拒绝额服务
        * 跳转并且执行一段恶意代码  
    * 造成缓冲区溢出的主要原因是程序中没有仔细检查用户输入。 

5. 死锁  
在两个或者多个并发进程中，如果每个进程持有某种资源而又等待其它进程释放它或它们现在保持着的资源，在未改变这种状态之前都不能向前推进，称这一组进程产生了死锁。通俗的讲就是两个或多个进程无限期的阻塞、相互等待的一种状态。  
死锁产生的四个条件（有一个条件不成立，则不会产生死锁）
    * 互斥条件：一个资源一次只能被一个进程使用
    * 请求与保持条件：一个进程因请求资源而阻塞时，对已获得资源保持不放
    * 不剥夺条件：进程获得的资源，在未完全使用完之前，不能强行剥夺
    * 循环等待条件：若干进程之间形成一种头尾相接的环形等待资源关系 
    
处理策略：鸵鸟策略、预防策略、避免策略、检测与恢复策略

6. 进程有哪几种状态？
    * 就绪状态：进程已获得除处理机以外的所需资源，等待分配处理机资源
    * 运行状态：占用处理机资源运行，处于此状态的进程数小于等于CPU数
    * 阻塞状态： 进程等待某种条件，在条件满足之前无法执行 

7. 分页和分段有什么区别？  
    * 页是信息的物理单位，分页是为实现离散分配方式，以消减内存的外零头，提高内存的利用率；或者说，分页仅仅是由于系统管理的需要，而不是用户的需要。
    * 段是信息的逻辑单位，它含有一组其意义相对完整的信息。分段的目的是为了能更好的满足用户的需要。
    * 页的大小固定且由系统确定，把逻辑地址划分为页号和页内地址两部分，是由机器硬件实现的，因而一个系统只能有一种大小的页面。段的长度却不固定，决定于用户所编写的程序，通常由编辑程序在对源程序进行编辑时，根据信息的性质来划分。
    * 分页的作业地址空间是一维的，即单一的线性空间，程序员只须利用一个记忆符，即可表示一地址。分段的作业地址空间是二维的，程序员在标识一个地址时，既需给出段名，又需给出段内地址。
    
8. 操作系统中进程调度策略
    * 先来先服务
    * 优先级
    * 短作业优先
    * 时间片轮转
    * 最高响应比优先算法(HRN)：FCFS可能造成短作业用户不满，SPF可能使得长作业用户不满，于是提出HRN，选择响应比最高的作业运行。响应比=1+作业等待时间/作业处理时间。
9. 进程同步机制
    【待补充】

5. 中断和轮询的特点
    * 对I/O设备的程序轮询的方式，是早期的计算机系统对I/O设备的一种管理方式。它定时对各种设备轮流询问一遍有无处理要求。轮流询问之后，有要求的，则加以处理。在处理I/O设备的要求之后，处理机返回继续工作。
    * 程序中断是指CPU在正常运行程序的过程中，由于预先安排或发生了各种随机的内部或外部事件，使CPU中断正在运行的程序，而转到为响应的服务程序去处理。
    * 轮询——效率低，等待时间很长，CPU利用率不高。
    * 中断——容易遗漏一些问题，CPU利用率高。

6. 临界区  
每个进程中访问临界资源的那段程序称为临界区，每次只准许一个进程进入临界区，进入后不允许其他进程进入。
    * 如果有若干进程要求进入空闲的临界区，一次仅允许一个进程进入；
    * 任何时候，处于临界区内的进程不可多于一个。如已有进程进入自己的临界区，则其它所有试图进入临界区的进程必须等待；
    * 进入临界区的进程要在有限时间内退出，以便其它进程能及时进入自己的临界区；
    * 如果进程不能进入自己的临界区，则应让出CPU，避免进程出现“忙等”现象。
1. 数据传输率（C）=记录位密度（D） x   线速度( V )
1. https://www.nowcoder.com/test/question/done?tid=9502589&qid=44781#summary

## Linux 
1. 常用命令  
    1. cd 切换路径
    2. ls 列出文件
        * -l ：列出长数据串，包含文件的属性与权限数据等  
        * -a ：列出全部的文件，连同隐藏文件（开头为.的文件）一起列出来（常用）  
        * -R ：连同子目录的内容一起列出（递归列出），等于该目录下的所有文件都会显示出来  
    3. grep【待补充】
    4. find【待补充】
    5. cp 复制
        * -a ：将文件的特性一起复制  
        * -p ：连同文件的属性一起复制，而非使用默认方式，与-a相似，常用于备份  
        * -i ：若目标文件已经存在时，在覆盖时会先询问操作的进行  
        * -r ：递归持续复制，用于目录的复制行为  
        * -u ：目标文件与源文件有差异时才会复制  
    6. mv 移动
        * -f ：force强制的意思，如果目标文件已经存在，不会询问而直接覆盖  
        * -i ：若目标文件已经存在，就会询问是否覆盖  
        * -u ：若目标文件已经存在，且比目标文件新，才会更新  
    7. rm 删除
        * -f ：就是force的意思，忽略不存在的文件，不会出现警告消息  
        * -i ：互动模式，在删除前会询问用户是否操作  
        * -r ：递归删除，最常用于目录删除，它是一个非常危险的参数  
    8. ps 查看进程
        * ps aux # 查看系统所有的进程数据  
        * ps ax # 查看不与terminal有关的所有进程  
        * ps -lA # 查看系统所有的进程数据  
        * ps axjf # 查看连同一部分进程树状态  
    9. kill 关闭
        * kill pid
    10. file 判断接在file命令后的文件的基本数据
    11. tar 打包
        * 压缩：tar -zcv -f filename.tar.gz 要被处理的文件或目录名称  
        * 查询：tar -ztv -f filename.tar.gz 
        * 解压：tar -zxv -f filename.tar.gz -C 欲解压缩的目录  
    12. cat 查看文本文件的内容
    13. time 用于测算一个命令（即程序）的执行时间
        * 在命令的前面加入一个time即可
    14. pwd 显示当前目录
    
【待补充】
2. Linux文件属性
【待补充】
1. Linux中stdout和stderr输出缓冲
    * 结论：stdout使用了行缓冲区，stderr没使用缓冲区
    * 仔细的说，stdout只有在换行的时候才输出
1. https://www.nowcoder.com/test/question/done?tid=9501873&qid=46328#summary
# 设计模式

### 常见的23种设计模式
0. 详解参考 [Java之美[从菜鸟到高手演变]之设计模式](http://blog.csdn.net/zhangerqing/article/details/8194653/?spm=5176.100239.blogcont69707.9.A2VX6A#comments) ，我只是对这篇文章进行了提炼，错误的简单更正，能让自己回忆起来。

1. 工厂方法模式
    * 普通工厂模式，用字符串产生实例
    * 多个工厂方法模式，直接调用相应的建造实例的方法
    * 静态工厂方法模式，多个工厂方法模式里的方法置为静态的，不需要创建工厂实例，直接调用即可
2. 抽象工厂模式
    * 为了解决程序扩展问题
    * 每个类对应一个工厂，然后这些工厂实现一个接口，使用时对相相应的工厂进行实例化，然后通过接口获取实例。
    ```java
    Provider provider = new SendMailFactory(); //Privider为所有工厂实现的接口
    Sender sender = provider.produce(); //produce方法获取该工厂想要产生的实例
    sender.Send(); 
    ```
3. 单例模式（Singleton）
    * 实现一，静态加载
    ```java
    class Singleton {
        private static Singleton instance = new Singleton();
        private Singleton (){}
        public static Singleton getInstance() {
            return instance;
        }
    }
    ```
    * 实现二，双重锁定
    ```java
    class Singleton {
        private static Singleton instance = null;
        private Singleton() {}
        public static Singleton getInstance() {
            if (instance == null) {
                synchronized (Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    ```
4. 建造者模式（Builder）
    * Builder：给出一个接口，以规范产品对象的各个组成成分的建造。规定要实现复杂对象的哪些部分的创建。（人的身体有哪些部分）
    * ConcreteBuilder：实现Builder接口，具体化复杂对象的各部分的创建。 在建造过程完成后，提供产品的实例。（某个人具体每部分的样子）
    * Director：调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建。（按照样子来创建一个人）
    * Product：要创建的复杂对象。（从Builder中接收这个人）
5. 原型模式（Prototype）
    * 用原型实例指定创建对象的种类，并通过拷贝这些原型创建新的对象
    * 原型类需要具备两个条件
        * 实现Cloneable接口
        * 重写Object类中的clone方法，如果是浅拷贝的话，直接super.clone()
    * 优点
        * 创建对象比直接new一个对象在性能上要好的多，因为Object类的clone方法是一个本地方法，它直接操作内存中的二进制流，特别是复制大对象时，性能的差别非常明显。
        * 简化对象的创建，使得创建对象就像我们在编辑文档时的复制粘贴一样简单。
6. 适配器模式（Adapter）
    * 适配器模式将一个类的接口转换成客户期望的另一个接口，让原本不兼容的接口可以合作无间。
    * 适配器对象实现原有接口
    * 适配器对象组合一个实现新接口的对象
    * 对适配器原有接口方法的调用被委托给新接口的实例的特定方法
    ```java
    public class Adapter implements OldInterface{ //实现旧接口  
        private NewInterface newInterface;//组合新接口  
        //在创建适配器对象时，必须传入一个新接口的实现类 
        public Adapter(NewInterface newInterface) {
            this.newInterface = newInterface;
        }
        //将对旧接口的调用适配到新接口 
        @Override
        public void oldFunction() {
            newInterface.newFunction();
        }
    }  
    ```
7. 装饰器模式(Decorator)
    * 简而言之，就是用一个装饰器，将需要装饰的类进行装饰，类似于AOP代理
    * Component：组件对象的接口，可以给这些对象动态的添加职责；
    * ConcreteComponent：具体的组件对象，实现了组件接口。该对象通常就是被装饰器装饰的原始对象，可以给这个对象添加职责；
    * Decorator：所有装饰器的父类，需要定义一个与组件接口一致的接口(主要是为了实现装饰器功能的复用，即具体的装饰器A可以装饰另外一个具体的装饰器B，因为装饰器类也是一个Component)，并持有一个Component对象，该对象其实就是被装饰的对象。如果不继承组件接口类，则只能为某个组件添加单一的功能，即装饰器对象不能在装饰其他的装饰器对象。
    * ConcreteDecorator：具体的装饰器类，实现具体要向被装饰对象添加的功能。用来装饰具体的组件对象或者另外一个具体的装饰器对象。
    ```java
    Component c1 = new ConcreteComponent(); //首先创建需要被装饰的原始对象(即要被装饰的对象)  
    Decorator decoratorA = new ConcreteDecoratorA(c1); //给对象透明的增加功能A并调用  
    decoratorA.operation();
    Decorator decoratorB = new ConcreteDecoratorB(c1); //给对象透明的增加功能B并调用  
    decoratorB.operation();
    Decorator decoratorBandA = new ConcreteDecoratorB(decoratorA);//装饰器也可以装饰具体的装饰对象，此时相当于给对象在增加A的功能基础上在添加功能B  
    decoratorBandA.operation();
     ```
8. 代理模式
    * 抽象角色：声明真实对象和代理对象的共同接口。    
    * 代理角色：代理对象角色内部含有对真实对象的引用，从而可以操作真实对象，同时代理对象提供与真实对象相同的接口以便在任何时刻都能代替真实对象。同时，代理对象可以在执行真实对象操作时，附加其他的操作，相当于对真实对象进行封装（也就是Decorator？）。
    * 真实角色：代理角色所代表的真实对象，是我们最终要引用的对象
9. 外观模式
    * 外观模式是为了解决类与类之间的依赖关系的，外观模式就是将他们的关系放在一个Facade[fə'sɑːd]类中，降低了类类之间的耦合度，该模式中没有涉及到接口。
    ```java
    //Facade
    public class Computer {
    //...cpu, memory, disk之间没有关系，如果没有Computer他们将互相严重依赖
        public void startup(){
            System.out.println("start the computer!");
            cpu.startup();
            memory.startup();
            disk.startup();
            System.out.println("start computer finished!");
        }
    }  
    ```
10. 桥接模式
    * 桥接的用意是：将抽象化与实现化解耦，使得二者可以独立变化
    * 常见的JDBC桥DriverManager就是桥接模式。
    * 设计一个桥，传入需要连接的对象，然后用桥来调用方法（那和实现一个拥有该方法的接口有什么区别呢？为了解决历史遗留问题？）
11. 组合模式(部分-整体模式)
    * 将对象组合成树形结构以表示“部分整体”的层次结构。组合模式使得用户对单个对象和使用具有一致性。
    * 如果你想要创建层次结构，并可以在其中以相同的方式对待所有元素，那么组合模式就是最理想的选择
12. 享元模式
    * 实现对象的共享，即共享池，当系统中对象多的时候可以减少内存的开销，通常与工厂模式一起使用。
    * 例如jdbc连接池
13. 策略模式（strategy）
    * 把一个类中经常改变或者将来可能改变的部分提取出来，作为一个接口，然后在类中包含这个对象的实例，这样类的实例在运行时就可以随意调用实现了这个接口的类的行为。
    * 比如定义一系列的算法,把每一个算法封装起来, 并且使它们可相互替换，使得算法可独立于使用它的客户而变化。
14. 模板方法模式
    * 父类中，有一个主方法，再定义1...n个方法，可以是抽象的，也可以是实际的方法
    * 定义一个类，继承该父类，重写方法，通过调用类，实现对子类的调用
    * 一般不需要变的方法可以为定义为final
15. 观察者模式
    * 当一个对象变化时，其它依赖该对象的对象都会收到通知，并且随着变化
    * 用一个类来保存依赖关系，并通知依赖者被依赖者的变化情况
16. 迭代子模式
    * 顺序访问聚集中的对象
17. 责任链模式
    * 有多个对象，每个对象持有对下一个对象的引用，这样就会形成一条链，请求在这条链上传递，直到某一对象决定处理该请求。
    * 发出者并不清楚到底最终那个对象会处理该请求
    * 责任链模式可以实现，在隐瞒客户端的情况下，对系统进行动态的调整。
18. 命令模式
    * 抽象命令（Command）：定义命令的接口，声明执行的方法。
    * 具体命令（ConcreteCommand）：具体命令，实现要执行的方法，它通常是“虚”的实现；通常会有接收者，并调用接收者的功能来完成命令要执行的操作。
    * 接收者（Receiver）：真正执行命令的对象。任何类都可能成为一个接收者，只要能实现命令要求实现的相应功能。
    * 调用者（Invoker）：要求命令对象执行请求，通常会持有命令对象，可以持有很多的命令对象。这个是客户端真正触发命令并要求命令执行相应操作的地方，也就是说相当于使用命令对象的入口。
    * 客户端（Client）：命令由客户端来创建，并设置命令的接收者。
19. 备忘录模式
    * 目的是保存一个对象的某个状态，以便在适当的时候恢复对象
    * 假设有原始类A，A中有各种属性，A可以决定需要备份的属性
    * 备忘录类B是用来存储A的一些内部状态
    * 类C就是一个用来存储备忘录的，且只能存储，不能修改等操作。
20. 状态模式
    * 通过改变状态来改变行为
21. 访问者模式【似懂非懂】
    * 封装某些作用于某种数据结构中各元素的操作，它可以在不改变数据结构的前提下定义作用于这些元素的新的操作。
    ```java
    class A {
        void method1() { System.out.println("我是A"); }
        void method2(B b) { b.showA(this); }
    }
    class B {
        void showA(A a) { a.method1(); }
        public static void main(String[] args) {
            A a = new A();
            a.method1();
            a.method2(new B());
        }
    }
    ```
22. 中介者模式
    * 定义一个中介对象来封装系列对象之间的交互。
    * 中介者使各个对象不需要显示地相互引用，从而使其耦合性松散，而且可以独立地改变他们之间的交互。
23. 解释器模式
    * 就是写个解释器，比如正则表达式解释器，四则运算解释器。
### 常见问题
1. 单例模式相较于静态类的优点
    * 静态类对接口不友好，Java8 才出现静态方法。
    * 单例可以被延迟初始化，静态类一般在第一次加载是初始化。之所以延迟加载，是因为有些类比较庞大，所以延迟加载有助于提升性能。不过用静态实现的单例模式没有这个优点。
    * 单例类可以被继承，他的方法可以被override。但是静态类内部方法都是static，无法被override。
    * 单例类比较灵活，毕竟从实现上只是一个普通的Java类，只要满足单例的基本需求，你可以在里面随心所欲的实现一些其它功能，但是静态类不行。

# 数学
### 概率论
1. https://www.nowcoder.com/test/question/done?tid=9501873&qid=46327#summary


# 小算法 或 智商题
只总结一些我接触的少，或者常见但是忘记了的算法 
1. Reservoir Sampling  
    【待补充】
2. 排序
    * 简单修改了一下[DualPivotQuicksort](https://github.com/llysrv/Interview/blob/master/src/sort/DualPivotQuicksort.java)的源码
    * 源码很强
    * 含有：
        * 插入排序
        * 普通快排
        * 双轴快排
        * Tim sort
    * 还有各种特判优化
    * 值得一默
        
3. 哈夫曼树

1. 老鼠喝可乐
    * 问题：有15瓶可乐，其中有一瓶是坏的，问需要多少只老鼠同时喝才能知道哪一瓶可乐是坏的
    * 答案：不懂。。我感觉是二进制问题，我猜的是4
1. 2、5、7改变开关
1. 两个人约6~7点见面，如果等超过15min就回家，问能见面的概率
# Python 3
Python相关内容，点击[这里](https://github.com/llysrv/Interview/tree/master/Python)