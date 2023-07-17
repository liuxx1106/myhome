一、什么是Mybatis?
   答: 1. Mybatis是一个半orm（对象映射）框架，它内部封装了JDBC，开发时只需要关注SQL的本身，不需要 加载驱动、创建连接、写statement的过程，程序员直接编写原生的sql,灵活性高。

        2. Mybatis可以使用XML 或注解来配置和映射原生信息，将POJO映射成数据库的记录(对象的属性字段)，避免了所有JDBC代码和手动设置参数以及获取结果集。

二、 Mybatis的优点有哪些？
      答:  1. 基于SQL编程，不会对数据库的现有设计和java应用程序造成任何影响，SQL写在XML文件里，解除了SQL与应用程序代码的耦合，方便统一管理; 提供XML标签(结果map)，支持动态编写SQL语句，并可重用。

        2. 与JDBC相比，减少了代码的冗余量，减少了50%的代码量，不需要手动开关连接。
    
        3. 很好与各种数据库兼容(因为Mybatis使用JDBC连接数据库，所以只要支持JDBC连接数据库的都支持Mybatis)。
    
        4.  能够与spring集成。
    
        5.  提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

三、 Mybatis的缺点有那些？
     答: 

        1. SQL语句的编写工作量较大，尤其是字段多的时候，对开发人员的编写SQL语句的功底有一定要求。
    
        2. SQL 依赖于数据库，导致数据库移植性差，SQL语句依赖于数据库。 

四、Mybatis的适用场合？
     答: 

        1.  Mybatis专注于SQL的本身，是一个足够灵活的DAO层方案。
    
        2. 对性能要求很高的项目，使用myabtis是一个不错的选择。

五、#{}与${}的区别是什么？
   答:   #{} 是预编译处理，${}是字符串替换。 

        1. Mybatis处理#{}时，会将sql#{}转换为?,然后使用PreparedStatement的set方法来赋值。
    
        2. 使用#{}能有效的预防SQL注入，提高系统的安全性。

六、 当实体类中的属性名与表中的字段名不一样，怎么办？
有两种方式：

        1. 使用as 别名的方式让字段的别名与属性名一致。
    
        2. 使用<resultMap>来映射字段名和实体类属性名的一一对应的关系 

<select id="getOrder" parameterType="int"
resultMap="orderresultmap">
select * from orders where order_id=#{id}
</select>
<resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
<!–用 id 属性来映射主键字段–>
<id property=”id” column=”order_id”>
<!–用 result 属性来映射非主键字段，property 为实体类属性名，column
为数据表中的属性–>
<result property = “orderno” column =”order_no”/>
<result property=”price” column=”order_price” />
</reslutMap>
七、 Mybatis 是如何进行分页的？分页插件的原理是什么？
        Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。可以在sql 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件 的拦截方法内拦截待执行的 sql，然后重写 sql，根据 dialect 方言，添加对应的物理分页语句和物理分页参数。

八、Mybatis是如何将SQL执行结果封装成目标对象并返回的，都有哪些映射形式？
  答: 

        1. ResultMap形式。 将SQL执行结果用<ResultMap>标签映射成目标对象。
    
        2.  使用SQL列的别名，将查询结果用as转换为列的别名书写成对象属性名。

九、怎么利用Mybatis实现批量数据的插入？
        首先创建一个简单的insert语句，然后结合java程序，将代码和xml结合实现批量插入。

<insert id=”insertname”>
insert into names (name) values (#{value})
</insert>
        对应的java程序利用sessionFactory去创建一个SqlSession

List <string> names = new ArrayList();
names.add(“zhangsan”);
names.add(“lisi”);
names.add(“wangwu”);
// 注意这里 executortype.batch
Sqlsession sqlsession =
        sessionfactory.opensession(executortype.batch);
try {
Namemapper mapper = sqlsession.getmapper(namemapper.class);
for (string name: names) {
    mapper.insertname(name);
}
    sqlsession.commit();
} catch (Exception e) {
      e.printStackTrace();
      sqlSession.rollback();
      throw e;
}finally {
    sqlsession.close();
}

        这个地方建议使用excutortype.batch, 如果在xml里使用foreach标签的话性能会明显下降 。

十、在mapper中如何传多个参数？
 答：   

   1. 使用#{0},#{1}来获取参数，0表示DAO层的第一个参数，1表示DAO层的第二个参数。

   2. 使用@Param注解来传参数，然后使用#{参数名}来获取参数。

   3.使用Map将多个参数封装成对象，然后传参。

十一、Mybatis有哪些动态的SQL? 执行原理？有哪些动态的SQL？
  答:

       1.  Mybatis的动态SQL可以在XML映射文件内，以标签的形式编写动态SQL，执行原理是根据表达式的值，完成逻辑判断并动态拼接sql的功能。
    
       2.  Mybatis 提供了 9 种动态 sql 标签：trim | where | set | foreach | if | choose | when | otherwise | bind。

十二、Mybatis的Xml映射文件中，不同的Xml映射文件，是否可以存在相同的id,为什么？
 答: 

      如果Xml映射文件配置了namespace命名空间，那么不用的Xml映射文件就可以存在相同的id,如果没有配置namespace,那么就不允许有相同的id,因为namespace+id 是作为 Map<String, MapperStatement>的 key 使用的，如果没有 namespace，就剩下 id，那么，id 重复会导致数据互相覆盖。

十三、如何获取自动生成的(主)键值?
答: 

        采用自增长策略，然后对象的属性进入到数据库后，会自动将键值设置到对象中，也就是insert方法执行完后键值会被设置到传入的参数对象中。
    
      举例:
    
      service：

 @Override
    public void insertUser() {
        User user=new User();
        user.setUsername("bingbing");
        user.setPassword("123456");
       Integer rows= userMapper.insertname(user);
       System.out.println("影响条数 :"+rows);
       System.out.println("id为:"+user.getId());

    }
        mapper.xml,需要使用属性useGenerateKeys="true",执行完insert方法后，会将插入到数据库的id自动设置到user对象中。如果不加这个，那么执行完insert方法后，取到的id为null

<insert id="insertname" useGeneratedKeys="true" keyProperty="id">
      insert into user(username,password) values (#{user.username},#{user.password})

</insert>
测试:



去掉 useGenerateKeys="true", 再重新访问:



十四、Mybatis实现一对一有几种方式？具体是怎么操作的？
        答: 有2种方式: 联合查询和嵌套查询。联合查询是几个表联合查询，只查询一次，通过在resultMap里配置association节点配置一对一的类就可以完成。 

        嵌套查询是先查一个表，然后根据这个表里的结果的外键Id(也可以是其他字段),去再另一个表里面查询数据，也是通过association配置，但另外一个表的查询是通过select属性来配置的。

十五、Mybatis 实现一对多的方式有哪几种？ 具体是怎么操作的？ 
        答:  有2种方式: 联合查询和嵌套查询。 联合查询是只查询一次，通过在resultMap里配置collection节点配置一对多的类就可以完成。嵌套查询是先查 一个表,根据这个表里面的 结果的外键 id,去再另外一个表里面查询数据,也是通过 配置 collection,但另外一个表的查询通过 select 节点配置。

十六、 Mybatis的一对一、一对多查询？
        答: 

        在resultMap标签里配置association节点实现一对一，配置collection节点实现一对多。

<mapper namespace="com.lcb.mapping.userMapper">

<!--association 一对一关联查询 -->
<select id="getClass" parameterType="int"
resultMap="ClassesResultMap">
select * from class c,teacher t where c.teacher_id=t.t_id and
c.c_id=#{id}
</select>
 

<resultMap type="com.lcb.user.Classes" id="ClassesResultMap">
<!-- 实体类的字段名和数据表的字段名映射 -->
<id property="id" column="c_id"/>
<result property="name" column="c_name"/>

<association property="teacher"
javaType="com.lcb.user.Teacher">
<id property="id" column="t_id"/>
<result property="name" column="t_name"/>
</association>

</resultMap>


<!--collection 一对多关联查询 -->
<select id="getClass2" parameterType="int"
resultMap="ClassesResultMap2">
select * from class c,teacher t,student s where c.teacher_id=t.t_id
and c.c_id=s.class_id and c.c_id=#{id}
</select>
 

<resultMap type="com.lcb.user.Classes" id="ClassesResultMap2">

<id property="id" column="c_id"/>
<result property="name" column="c_name"/>

<association property="teacher"
javaType="com.lcb.user.Teacher">
<id property="id" column="t_id"/>
<result property="name" column="t_name"/>
</association>

<collection property="student"
ofType="com.lcb.user.Student">
<id property="id" column="s_id"/>
<result property="name" column="s_name"/>
</collection>

</resultMap>


</mapper>

十七、Mybatis是否支持延迟加载？ 如果支持它的原理是什么？
答:   

        Mybatis仅支持association和collection对象的延迟加载，association指的是一对一, collection指的是一对多，在Mybatis的配置文件里，可以配置是否启用延迟加载lazyLoadingEnabled=true/false。
    
        它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦 截器方法，比如调用 a.getB().getName()，拦截器 invoke()方法发现 a.getB()是 null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来， 然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName() 方法的调用。这就是延迟加载的基本原理。

十八、 通常一个xml映射文件，都会写一个DAO接口与之对应，请问，这个DAO接口的工作原理是什么？ Dao接口里的方法，参数不同时，方法能重载嘛？ 
        DAO接口即Mapper接口。接口的全限定名，就是映射文件里面的namsepacce。接口的方法名就是映射文件里的mapper的statement的id值，接口方法内的参数即传递给sql的参数。 

        Mapper接口是没有实现类的，当调用接口方法时，接口全限定名+方法名拼接字符串作为key的值，可以唯一定位一个MapperStatement。在Mybatis中，每个<select>,<insert>,<delete>,<update>标签都会被解析成一个MapperStatement对象。
    
        Mapper 接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。Mapper 接口的工作原理是 JDK 动态代理，Mybatis 运行时会使用 JDK 动态代理为 Mapper 接口生成代理对象 proxy，代理对象会拦截接口方法，转而 执行MapperStatement 所代表的 sql，然后将 sql 执行结果返回。

十九、Mybatis加载mapper的方式有哪些?
        4 种方式。 package、url、resource、class。 其中package的优先级最高。

二十、Mapper接口的@Param注解传参是一定必须的嘛？
        当只有一个参数并且在<if>标签里的时候，必须要用@Param注解。

二十一、Mybatis的核心组件有哪些?
        SqlSessionFactory。 用于创建SqlSession的工厂类。

        SqlSession。 用于向数据库执行SQL语句。
    
        Mapper映射器。用于编写SQL，采用XML、注解均可实现。
————————————————
版权声明：本文为CSDN博主「Dream_it_possible！」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33036061/article/details/105209254