### 微众面试题

[(54条消息) 微众银行--java面试题一_微众银行面试_bp粉的博客-CSDN博客](https://blog.csdn.net/m0_67392182/article/details/126599774)

#### 微众与传统银行的区别

- 渠道侧重不同，传统银行主要是基于线下门店，主要依赖于线下柜台服务和客户经理吸纳客户，而微众银行则是主要在于线上吸收资金和发放贷款。主要是依赖于it系统和云计算。
- 征信系统不同，传统银行主要是线下审核，贷款对象主要是有充沛现金流有偿还能力的富人。而微众基于社交大数据的信用体系，可以更为方便灵活的为普通老百姓和微小企业提供服务。

#### 1. CAS 底层原理

> 使用了unsafe方法。unsafe是java中提供的一个可以直接操作内存的类，通过反射获取它的实例。compareandswap(比较并交换)：参数内存值，旧的预期值，要修改的新值。修改前先比较内存值与旧值，若相等则执行修改操作并返回true,若不相等则返回false.
>
> 变量需要配合volitail使用，保证可见性；
>
> 利用该方法，我们可以使用while循环去实现一个自旋锁，不停的重试直到成功才返回结果。
>
> 底层，C语言实现的cmpxchg的方法，多核情况下，这个方法里面也是加了lock指令，目的是可以保证cpu的一个核心在操作期间独占一片内存区域。

#### 2. **如何避免脏读**

> 脏读：脏读指的是读到了其他事务未提交的数据，未提交意味着这些数据可能会回滚，也就是可能最终不会存到数据库中，也就是不存在的数据。读到了并不一定最终存在的数据，这就是脏读。
>
> 幻读：例如事务 A 对一个表中的数据进行了修改，**这种修改涉及到表中的全部数据行**。此时，突然事务 B 插入了一条数据并提交了，当事务 A 提交了修改数据操作之后，再次读取全部数据，**结果发现还有一条数据未更新，给人感觉好像产生了幻觉一样**。这就是幻读！
>
> 不可重复读：不可重复读指的是在一个事务内，最开始读到的数据和事务结束前的任意时刻读到的同一批数据出现不一致的情况。

- 事务的隔离级别设置为rr(可重复读)
- 使用排他锁select for update

#### 3. http1.1和http1.0的区别

[聊聊 HTTP 1.0 和 HTTP 1.1 的区 | CS指南 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/474004335)

1. **连接方式** : HTTP 1.0 为短连接，HTTP 1.1 支持长连接。
2. **状态响应码** : HTTP/1.1 中新加入了大量的状态码，光是错误响应状态码就新增了 24 种。比如说，`100 (Continue)`——在请求大资源前的预热请求，`206 (Partial Content)`——范围请求的标识码，`409 (Conflict)`——请求与当前资源的规定冲突，`410 (Gone)`——资源已被永久转移，而且没有任何已知的转发地址。
3. **缓存处理** : 在 HTTP1.0 中主要使用 header 里的 If-Modified-Since,Expires 来做为缓存判断的标准，HTTP1.1 则引入了更多的缓存控制策略例如 Entity tag，If-Unmodified-Since, If-Match, If-None-Match 等更多可供选择的缓存头来控制缓存策略。
4. **带宽优化及网络连接的使用** :HTTP1.0 中，存在一些浪费带宽的现象，例如客户端只是需要某个对象的一部分，而服务器却将整个对象送过来了，并且不支持断点续传功能，HTTP1.1 则在请求头引入了 range 头域，它允许只请求资源的某个部分，即返回码是 206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。
5. **Host 头处理** : HTTP/1.1 在请求头中加入了`Host`字段。

#### 4. 怎么判断一个字符串中的大小写字母，不用系统自带函数

- 根据字符的ascii码值。大写字母在65-90之间，小写字母在97-122之间
- 系统函数，isUpperCase和isLowerCase

#### 5. 数据库索引

#### 6. 表内链接和外连接

[Mysql内连接和外连接的区别 - 小刘的早餐店 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lis2/p/16067341.html)

内连接：指连接结果仅包含符合连接条件的行，参与连接的两个表都应该符合连接条件。

外连接：连接结果不仅包含符合连接条件的行同时也包含自身不符合条件的行。包括左外连接、右外连接和全外连接。

左外连接：左边表数据行全部保留，右边表保留符合连接条件的行。

右外连接：右边表数据行全部保留，左边表保留符合连接条件的行。

全外连接：左外连接 union 右外连接，Mysql 中暂不支持。

#### 7. java类的修饰符，成员变量，方法的修饰符区别

# [java中的类修饰符、成员变量修饰符、方法修饰符。](https://www.cnblogs.com/lixiaolun/p/4311727.html)

https://www.cnblogs.com/lixiaolun/p/4311727.html

***类修饰符：***

public（访问控制符），将一个类声明为公共类，他可以被任何对象访问，一个程序的主类必须是公共类。

abstract，将一个类声明为抽象类，没有实现的方法，需要子类提供方法实现。

final，将一个类生命为最终（即非继承类），表示他不能被其他类继承。

friendly，默认的修饰符，只有在相同包中的对象才能使用这样的类。

  

***成员变量修饰符：***

public（公共访问控制符），指定该变量为公共的，他可以被任何对象的方法访问。

private（私有访问控制符）指定该变量只允许自己的类的方法访问，其他任何类（包括子类）中的方法均不能访问。

protected（保护访问控制符）指定该变量可以别被自己的类和子类访问。在子类中可以覆盖此变量。

friendly ，在同一个包中的类可以访问，其他包中的类不能访问。

final，最终修饰符，指定此变量的值不能变。

static（静态修饰符）指定变量被所有对象共享，即所有实例都可以使用该变量。变量属于这个类。

transient（过度修饰符）指定该变量是系统保留，暂无特别作用的临时性变量。

volatile（易失修饰符）指定该变量可以同时被几个线程控制和修改。

  

***方法修饰符***：

public（公共控制符）

private（私有控制符）指定此方法只能有自己类等方法访问，其他的类不能访问（包括子类）

protected（保护访问控制符）指定该方法可以被它的类和子类进行访问。

final，指定该方法不能被重载。

static，指定不需要实例化就可以激活的一个方法。

synchronize，同步修饰符，在多个线程中，该修饰符用于在运行前，对他所属的方法加锁，以防止其他线程的访问，运行结束后解锁。

native，本地修饰符。指定此方法的方法体是用其他语言在程序外部编写的。

#### 8. 多态的提现？重写和重载的区别

 https://blog.csdn.net/caiyiii/article/details/82659267

方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。

(1)方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。

(2)方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。

(3)方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。

#### 9. 抽象类和接口区别，一个类如果继承了父类，还能实现接口么

1. [(54条消息) 抽象类和接口有什么区别_抽象类和接口的区别_gongxiao1993的博客-CSDN博客](https://blog.csdn.net/gongxiao1993/article/details/82055007?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-4-82055007-blog-121665234.235^v34^pc_relevant_increate_t0_download_v2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-4-82055007-blog-121665234.235^v34^pc_relevant_increate_t0_download_v2&utm_relevant_index=7)

### 抽象类和接口的对比

| **参数**           | **抽象类**                                                   | **接口**                                                     |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 默认的方法实现     | 它可以有默认的方法实现                                       | 接口完全是抽象的。它根本不存在方法的实现                     |
| 实现               | 子类使用**extends**关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。 | 子类使用关键字**implements**来实现接口。它需要提供接口中所有声明的方法的实现 |
| 构造器             | 抽象类可以有构造器                                           | 接口不能有构造器                                             |
| 与正常Java类的区别 | 除了你不能实例化抽象类之外，它和普通Java类没有任何区别       | 接口是完全不同的类型                                         |
| 访问修饰符         | 抽象方法可以有**public**、**protected**和**default**这些修饰符 | 接口方法默认修饰符是**public**。你不可以使用其它修饰符。     |
| main方法           | 抽象方法可以有main方法并且我们可以运行它                     | 接口没有main方法，因此我们不能运行它。（java8以后接口可以有default和static方法，所以可以运行main方法） |
| 多继承             | 抽象方法可以继承一个类和实现多个接口                         | 接口只可以继承一个或多个其它接口                             |
| 速度               | 它比接口速度要快                                             | 接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。   |
| 添加新方法         | 如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。 | 如果你往接口中添加方法，那么你必须改变实现该接口的类。       |

### 什么时候使用抽象类和接口

- 如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类吧。
- 如果你想实现多重继承，那么你必须使用接口。由于**Java不支持多继承**，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。
- 如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。

**什么时候使用抽象类？**

抽象类让你可以定义一些默认行为并促使子类提供任意特殊化行为。

例如：Spring的依赖注入就使得代码实现了集合框架中的接口原则和抽象实现。



#### 10. String类有哪些方法

[(54条消息) String类的常用方法都有哪些？_string类常用的方法_敲一天咯的博客-CSDN博客](https://blog.csdn.net/WYN5201314588/article/details/125801439)

（一）常见String类的获取功能
1.length()方法  获取字符串长度

> String str = "abcdef";
> System.out.println(str.length());//输出结果：6

2.charAt(int index)方法  传递一个下标参数，返回字符串对应位置的字符

> String str = "abc";
> System.out.println(str.charAt(1));//输出结果：b

3.indexOf()方法  传递某个字符，返回在字符串中的第一个位置

> String str = "abcabc";
> System.out.println(str.indexOf('a'));//输出结果：0

4.subString(int start)方法  默认是取到字符串末尾

> String str = "abcdef";
> System.out.println(str.subString(2));//输出结果：cdef

5.subString(int start,int end)方法 注：范围左闭右开，不包含下标为end的那位

> String str = "abcdefgh";
> System.out.println(str.subString(2,5));//输出结果：cde

（二）常见String类的判断功能
1.equals()方法 判断字符串内容是否相同，区分大小写。

> String str1 = "abc";
> String str2 = "ABC";
> System.out.println(str1.equals(str2));//输出结果：false

2.contains()方法 判断该字符串是否包含传递过来的字符串。

> String str1 = "我是猪";
> String str2 = "我是";
> String str3 = "是猪";
> System.out.println(str1.contains(str2));//输出结果：true
> System.out.println(str1.contains(str3));//输出结果：true

3.startsWith()方法 判断字符串是否以传进来的字符串开头。

endsWith()方法 判断字符串是否以传进来的字符串结尾。

> String str1 = " 我是猪";
> String str2 = " ";
> System.out.println(str1.startsWith(str2));//true
> System.out.printl(str1.endsWith(str2));//false

4.isEmpty()方法 判断字符串是否为空。

> String str = "";
> String str1 = "a";
> System.out.println(str.isEmpty());//true
> System.out.println(str1.isEmpty());//false
> （三）常见String类的转换功能
> 1.getBytes()方法 返回值类型 byte[]

    使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。

2.toCharArray()方法 将String类型的字符串转化为char字符数组

> Strint str = "我是你爹";
> char[] c = str.toCharArray();
> System.out.println(c[0]);//输出结果：我
> System.out.println(c[1]);//输出结果：是
> System.out.println(c[2]);//输出结果：你
> System.out.println(c[3]);//输出结果：爹

3.valueOf(char[] data)方法 将传递的字符数组，组合成字符串。

> char[] c = {'a','b','c','d'};
> String str = new String();
> str = str.valueOf(c);
> System.out.println(str);//输出结果：abcd

4.toLowerCase()方法 把字符串转成小写toUpperCase()方法 把字符串转成大写

5.comcat()方法 字符串拼接

> String str1 = "abc";
> String str2 = "def";
> String str3 = str1.concat(str2);
> System.out.println(str3);//输出结果：abcdef

（四）常见String类的其他常用功能
1.replace()方法 用传递的字符或字符串，替代指定的字符或者字符串。

2.trim()方法 去除两端空格

> String str = "  哈哈哈  ";
> System.out.println(str);//输出结果：  哈哈哈  ；
> str = str.trim();
> System.out.println(str);//输出结果：哈哈哈；
> 3.compareTo()方法

//源码：
public int compareTo(String anotherString) {
        int len1 = value.length;
        int len2 = anotherString.value.length;
        int lim = Math.min(len1, len2);
        char v1[] = value;
        char v2[] = anotherString.value;

        int k = 0;
        while (k < lim) {
            char c1 = v1[k];
            char c2 = v2[k];
            if (c1 != c2) {
                return c1 - c2;
            }
            k++;
        }
        return len1 - len2;
}
    解析：如果字符串与传递字符串的长度不等，那么返回就是两个字符串的长度差值；如果两个字符串长度相等，那么返回的就是，两个字符串长度最小值位置的字符ASCII码的差值。

> String str1 = "abc";
> String str2 = "ab";
> String str3 = "abd";
> String str4 = "abcdef";
> int a = str1.compareTo(str2);
> System.out.println(a);//输出结果：1
> int b = str1.compareTo(str3);s
> System.out.println(b);//输出结果：-1
> int c = str1.compareTo(str4);
> System.out.println(c);//输出结果：-3

#### 11. 在文件上传下载模块中，怎么实现一堆文件存储

[(54条消息) Java-断点续传(分片上传)_java断点上传_胡安民的博客-CSDN博客](https://huanmin.blog.csdn.net/article/details/124663444?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-1-124663444-blog-129854420.235^v34^pc_relevant_increate_t0_download_v2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-1-124663444-blog-129854420.235^v34^pc_relevant_increate_t0_download_v2&utm_relevant_index=2)

https://zhuanlan.zhihu.com/p/68798977

#### 12. 有没有Linux开发经验

#### 限流算法：

常见的限流算法有计数限流，固定窗口限流，滑动窗口限流，漏桶算法限流和令牌桶算法限流。

1. 滑动窗口算法

> [(55条消息) sentinel滑动时间窗口算法学习_时间滑动窗口算法_小小少年_的博客-CSDN博客](https://blog.csdn.net/CPLASF_/article/details/123889527?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-123889527-blog-124085421.235^v35^pc_relevant_increate_t0_download_v2&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

1. 令牌桶算法

> 令牌桶算法是限流算法的一种，其原理是系统会以一个恒定的速度往桶里放入固定数量的令牌，而如果请求需要被处理，则需要先从桶里获取对应令牌，当桶里没有令牌可取时，则拒绝服务。

#### springmvc

https://blog.csdn.net/m0_59281987/article/details/127992938

#### mybatis

https://blog.csdn.net/qq_33036061/article/details/105209254

#### synchronized

synchronized关键字，作用，修饰啥，底层原理。

是jvm提供的一个关键字，作用是在并发编程中，保证线程安全，即被它修饰的方法或代码块在同一时间只能被一个线程操作。

修饰静态方法，给当前类的class枷锁

修饰实例方法，给当前实例对象枷锁

修饰代码块，给指定类枷锁

> **底层原理**
>
> 这是使用同步代码块被标志的地方就是刚刚提到的对象头，它会关联一个monitor对象，也就是括号括起来的对象。
>
> 1、monitorenter，如果当前monitor的进入数为0时，线程就会进入monitor，并且把进入数+1，那么该线程就是monitor的拥有者(owner)。
>
> 2、如果该线程已经是monitor的拥有者，又重新进入，就会把进入数再次+1。也就是可重入的。
>
> 3、monitorexit，执行monitorexit的线程必须是monitor的拥有者，指令执行后，monitor的进入数减1，如果减1后进入数为0，则该线程会退出monitor。其他被阻塞的线程就可以尝试去获取monitor的所有权。
>
> monitorexit指令出现了两次，第1次为同步正常退出释放锁；第2次为发生异步退出释放锁；
> 总的来说，synchronized的底层原理是通过monitor对象来完成的。
> ————————————————
> 版权声明：本文为CSDN博主「959y」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/qq_43141726/article/details/119523441

#### AQS底层源码



AQS(AbstractQueuedSynchronizer)是Java中用于实现锁和同步器的抽象类。它提供了一个基本的同步框架，可以用来构建各种类型的同步器，如ReentrantLock、Semaphore、CountDownLatch等。

AQS的核心原理是通过一个双向链表来管理线程的等待和唤醒。当一个线程请求获取锁时，如果锁已经被其他线程占用，那么该线程就会被加入到等待队列中。当锁被释放时，AQS会从等待队列中取出一个线程并将其唤醒，使其可以继续执行。

AQS使用了一个叫做state变量来记录当前锁的状态。state变量有两个值：0表示未锁定，1表示已锁定。当一个线程请求获取锁时，AQS会首先检查state是否为0,如果是，则将state设置为1,并将线程加入到等待队列中；否则，直接返回。当一个线程持有锁时，如果该线程调用了unlock()方法，那么AQS会检查state是否为1,如果是，则将state设置为0,并从等待队列中取出一个线程并将其唤醒；否则，直接返回。

AQS还提供了一些其他的方法，如acquire(int arg)和release(int arg),它们分别用于获取锁和释放锁。acquire()方法会尝试获取锁，如果失败则会进入等待状态；如果成功，则会将state设置为1,并返回。release()方法会释放锁，并将state设置为0。

总之，AQS通过双向链表和state变量来管理线程的等待和唤醒，实现了Java中的锁和同步器的基本原理。

原文链接：https://blog.csdn.net/m0_46566541/article/details/119141525

#### 一个线程等待其他其他线程完成之后再启动如何实现？除了countDownLatch还有啥实现方式？

countdownlatch()。

join()。

#### 原子类是如何实现的？

cas机制。（当前值，偏移位置，预期值，当前对象）

#### spring

- ioc

>  Spring IOC的实现过程和原理如下：
>
> 1. 创建Bean实例：当应用程序需要一个Bean时，Spring容器会根据Bean定义创建一个Bean实例。
> 2. Bean属性注入：Spring容器使用反射机制，将创建的Bean实例的属性值注入到调用者中指定的位置。
> 3. 依赖注入：如果一个Bean需要另一个Bean来完成其功能，Spring容器将自动注入该Bean实例。
> 4. AOP切面：Spring IOC还可以与AOP(面向切面编程)技术结合使用，通过AOP技术实现日志记录、权限控制等功能。
> 5. 生命周期管理：Spring IOC可以管理Bean的生命周期，包括初始化、启动、关闭等阶段，并提供相应的回调接口。
>
> Spring IOC的实现原理是基于反射机制和Java的动态代理技术。当应用程序需要一个Bean时，Spring容器会根据Bean定义创建一个Bean实例，并使用反射机制将该实例的属性值注入到调用者中指定的位置。同时，如果一个Bean需要另一个Bean来完成其功能，Spring容器将自动注入该Bean实例。此外，Spring IOC还提供了依赖注入和AOP等功能，使得应用程序更加灵活和易于维护。

- aop

> Spring AOP的原理：
>
> 1. 切面(Aspect):切面是AOP的核心概念，它是一个包含一系列横切关注点的模块化代码块。切面可以被应用在多个目标对象上，例如方法调用、异常处理等。
>
> 2. 连接点(Joinpoint):连接点是指在程序执行过程中，被切面所关注的具体操作点。例如，一个方法调用、一个异常处理语句等。
>
> 3. 通知(Advice):通知是在连接点处执行的一段代码，它可以修改连接点的执行结果或者增加额外的行为。
>
> 4. 织入(Weaving):织入是将切面中的通知插入到连接点之前的步骤中的过程。Spring IOC容器可以通过代理模式实现织入。
>
> 5. AOP框架：Spring AOP框架是基于动态代理技术的，它可以在运行时动态地生成代理对象，并将切面与代理对象关联起来。当代理对象被调用时，AOP框架会自动执行切面中的通知。
>
> 6. AOP的优点：AOP可以提高代码的复用性、可维护性和可扩展性，同时也可以减少代码的重复和冗余。
>
> 使用场景：
>
> 1.日志记录：通过AOP可以在方法执行前后添加日志记录，方便开发人员进行调试和追踪。
>
> 2.权限控制：通过AOP可以在方法执行前进行身份验证和授权，保证系统的安全性。
>
> 3.事务管理：通过AOP可以在方法执行前后进行事务管理，确保数据的一致性和完整性。
>
> 4.性能优化：通过AOP可以在方法执行前后进行性能监控和优化，提高系统的性能和响应速度。
>
> 5.数据缓存：通过AOP可以在方法执行前后进行数据缓存，避免重复计算和提高系统效率。

[Spring IOC和AOP (baidu.com)](https://baijiahao.baidu.com/s?id=1712077766477430253&wfr=spider&for=pc)

#### 1. mybatis的mapper文件为什么可以实现注入？

[(55条消息) MyBatis的Mapper接口自动注册原理_mybatis的mapper接口怎么注入到service层_Alexon Xu的博客-CSDN博客](https://blog.csdn.net/shuoyueqishilove/article/details/127365241)

#### 2. RestController注解

@RestController=@Controller + @ResponseBody。

1.1 @Controller

在一个类上加上这个注解，表示这个类是一个控制器。

1.2 @ResponseBody

在请求方法上加上这个注解，控制器就会将请求响应的数据以指定的格式写入到Response的body中去。

1.3 所以说@RestController就是注册一个控制器，并且将响应的数据以指定的格式写入到Response的body中。

2. 附加

2.1 @RestController注解可以使用在类或者方法上

#### 3. jvm相关知识，问在实际中有没有jvm调优经历

![1684076066529](D:\PF\myhome\面试题\assets\1684076066529.png)

jvm内存参数：

- xmx 最大内存 
- xms最小内存
- xmn新生代
- xx:survive   伊甸园：幸村区

***.class文件存在堆中，类的原始信息存在方法区***

#### 3.1 三色标记

![1684078928448](D:\PF\myhome\面试题\assets\1684078928448.png)

#### 3.2 垃圾回收器

![1684079382390](D:\PF\myhome\面试题\assets\1684079382390.png)

#### 4. 会哪些设计模式？和jvm的一些调优有没有结合起来使用？



#### 5.游标的实现方式



#### 6.批量处理用的哪个函数，如果自己写怎么做

[(55条消息) java 批量处理_Java模拟数据量过大时批量处理数据的两种实现方法_eventually7的博客-CSDN博客](https://blog.csdn.net/weixin_35859336/article/details/114416479)

#### 7.自己实现一个注解comment

 // 定义

> @Target(ElementType.METHOD) // 表示该注解被用于什么位置，比如filed,method
> @Retention(RetentionPolicy.RUNTIME) // 表示该注解的神明周期。
> public @interface Authentications {
>     String[] role() default {};
> }
> // 使用
> 一般加在controller上，传入自己想要的参数

// 处理一

> @Pointcut(value = "@annotation(authentication)")  //设置切点
> @Around("authenticationService(authentication)")  // 获取切点

// 处理二

> 从perhand处拦截获取方法上的注解
> AccessLimit accessLimit = handlerMethod.getMethodAnnotation(AccessLimit.class);

#### 8.Redis自己实现锁

- setnx 
- redlock

[如何用Redis实现分布式锁_redis分布式锁_GeorgiaStar的博客-CSDN博客](https://blog.csdn.net/fuzhongmin05/article/details/119251590)

#### 9.Jvm超过一定空间实现自动dump文件



#### 10.Volatile原理

[一篇文章彻底搞懂volatile关键字底层实现原理 (qq.com)](https://mp.weixin.qq.com/s?__biz=MzAxNTE2NjEyMw==&mid=2247483734&idx=1&sn=8509d4aa61d08550c2b7668314da3582&chksm=9b897e92acfef7846cd7dcac24d46c05ef0fabe5187befa0216b986f232d681c948b9deba26e&scene=27)

#### 11.注解是如何生效的

任何注解类型都默认继承自java.lang.annotation包下的Annotation接口,表明这是一个注解类型，这是编译器自动帮我们完成的。但是手动继承Annotation没有这个效果,即不会把它当成注解类型。甚至Annotation接口本身也并不意味着它是注解类型。可以简单的理解为:我们可以也只可以通过@interface的方式来定义注解类型,这个注解类型默认会实现Annotation接口。

注解通过设置可以一直保留到运行期,此时VM通过反射的方式读取注解信息。由上面的介绍可知,注解是解释代码的代码,它必须存在于特定的代码元素之上，可以是类，可以是方法，可以是字段等等。
 为了更好的在运行时解析这些代码元素上的注解,java在反射包下为它们提供了一个接口java.lang.reflect.AnnotatedElement

作者：dylan丶QAQ

链接：https://www.jianshu.com/p/4a6305a31244

来源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 12.查看一个进程有多少线程

通过ps或top查找进程id

```objectivec
cat /proc/{进程id}/status
```

输出里有：

threads： 2

#### 13.查看一个进程打开了哪些文件

lsof -c 进程名 | head

lsof -p pid

lsof -u uid

#### 14.Mybatis配置了哪些内容

[(55条消息) Mybatis配置解析_可乐还是甜的好的博客-CSDN博客](https://blog.csdn.net/yangguang_98/article/details/121621046)