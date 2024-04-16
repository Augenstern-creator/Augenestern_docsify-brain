# 💪MyBatis

## 1、谈谈你对MyBatis的理解

- Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，加载驱动、创建连接、创建statement等繁杂的过程，开发者开发时只需要关注如何编写SQL语句，可以严格控制sql执行性能，灵活度高。
- 作为一个半ORM框架，MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集
- 通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）。
- 由于MyBatis专注于SQL本身，灵活度高，所以比较适合对性能的要求很高，或者需求变化较多的项目，如互联网项目

优点：

1. 基于 SQL 语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL 写在 XML 里，解除 SQL 与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态 SQL 语句，并可重用
2. 与 JDBC 相比，减少了代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接
3. 很好的与各种数据库兼容（因为 MyBatis 使用 JDBC 来连接数据库，所以只要 JDBC 支持的数据库 MyBatis 都支持）
4. 提供映射标签，支持对象与数据库的 ORM 字段关系映射；提供对象关系映射标签，支持对象关系组件维护

缺点：

1. SQL 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 SQL 语句的功底有一定要求
2. SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库





## 1、#{} 和 ${} 的区别是什么

- `${}`是 Properties 文件中的变量占位符，它可以用于标签属性值和 sql 内部，属于静态文本替换，比如${driver}会被静态替换为com.mysql.jdbc. Driver

- `#{}` 是 sql 的参数占位符，MyBatis 会将 sql 中的`#{}`替换为? 号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的? 号占位符设置参数值

- `#{}` 可以有效的防止SQL注入，提高系统安全性，`${}` 不能防止SQL注入

  



## 2、xml映射文件中，除了常见的 select、insert、update、delete标签之外，还有哪些标签

还有很多其他的标签， `<resultMap>` 、 `<parameterMap>` 、 `<sql>` 、 `<include>` 、 `<selectKey>` ，加上动态 sql 的 9 个标签， `trim|where|set|foreach|if|choose|when|otherwise|bind` 等，其中 `<sql>` 为 sql 片段标签，通过 `<include>` 标签引入 sql 片段， `<selectKey>` 为不支持自增的主键生成策略标签





## 3、通常一个xml映射文件，都会写一个Dao接口与之对应，那么这个Dao接口的工作原理是什么？Dao接口里面的方法、参数不同时，方法能重载吗

最佳实践中，通常一个 xml 映射文件，都会写一个 Dao 接口与之对应。Dao 接口就是人们常说的 `Mapper` 接口，接口的全限名，就是映射文件中的 namespace 的值，接口的方法名，就是映射文件中 `MappedStatement` 的 id 值，接口方法内的参数，就是传递给 sql 的参数。

`Mapper` 接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为 key 值，可唯一定位一个 `MappedStatement` ，举例： `com.mybatis3.mappers. StudentDao.findStudentById` ，可以唯一找到 namespace 为 `com.mybatis3.mappers. StudentDao` 下面 `id = findStudentById` 的 `MappedStatement` 。在 MyBatis 中，每一个 `<select>` 、 `<insert>` 、 `<update>` 、 `<delete>` 标签，都会被解析为一个 `MappedStatement` 对象。

**Mybatis 的 Dao 接口可以有多个重载方法，但是多个接口对应的映射必须只有一个，否则启动会报错。**也就是Dao 接口里的方法可以重载，但是 Mybatis 的 xml 里面的 ID 不允许重复。



Dao接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Dao接口生成代理proxy对象，代理对象proxy会拦截接口方法，转而执行MappedStatement所代表的sql，然后将sql执行结果返回。



## 4、在Mapper中如何传递多个参数

1. 若Dao层函数有多个参数，那么其对应的xml中，#{0}代表接收的是Dao层中的第一个参数，#{1}代表Dao中的第二个参数，以此类推。
2. 使用@Param注解：在Dao层的参数中前加@Param注解,注解内的参数名为传递到Mapper中的参数名
3. 多个参数封装成Map，以HashMap的形式传递到Mapper中



## 5、Mybatis动态sql有什么用？执行原理是什么？有哪些动态sql

Mybatis动态sql可以在xml映射文件内，以标签的形式编写动态sql，执行原理是根据表达式的值完成逻辑判断，并动态拼接sql的功能。

Mybatis提供了9种动态sql标签：trim、where、set、foreach、if、choose、when、otherwise、bind











## 6、xml映射文件中，不同的xml映射文件id是否可以重复

不同的xml映射文件，如果配置了namespace，那么id可以重复；如果没有配置namespace，那么id不能重复；



## 7、MyBatis实现一对一有几种方式？具体是怎么操作的

有联合查询和嵌套查询两种方式。

联合查询是几个表联合查询，通过在resultMap里面配置association节点配置一对一的类就可以完成

嵌套查询是先查一个表，根据这个表里面的结果的外键id，再去另外一个表里面查询数据，也是通过association配置，但另外一个表的查询是通过select配置的



## 8、Mybatis实现一对多有几种方式？具体是怎么操作的

有联合查询和嵌套查询两种方式。

联合查询是几个表联合查询，只查询一次，通过在resultMap里面的collection节点配置一对多的类就可以完成

嵌套查询是先查一个表，根据这个表里面的结果的外键id，再去另外一个表里面查询数据，也是通过collection，但另外一个表的查询是通过select配置的。



## 9、Mybatis的一级、二级缓存

Mybatis的缓存其实就是把之前查到的数据存入内存（map）,下次如果还是查相同的东西，就可以直接从缓存中取，从而提高效率。

1. 一级缓存：基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存
2. 二级缓存：二级缓存需要手动开启和配置，它是基于 namespace 级别的缓存。默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态)，可在它的映射文件中配置`<cache/>` 标签







## 10、MyBatis是如何进行分页的？分页插件的原理是什么

MyBatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。可以在 SQL 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用 MyBatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 SQL，然后重写 SQL，根据 dialect 方言，添加对应的物理分页语句和物理分页参数



## 11、MyBatis有几种分页方式

1. 借助数组进行分页（逻辑分页）
2. 借助Sql语句进行分页（物理分页）
3. 拦截器分页 (物理分页) 通过拦截器给sql语句末尾加上limit语句来查询，一劳永逸最优
4. RowBounds实现分页（逻辑分页）：PageHelper是mybatis的通用分页插件，通过mybatis的拦截器实现分页功能，拦截sql查询请求，添加分页语句，最终实现分页查询功能。



## 12、MyBatis逻辑分页和物理分页的区别是什么

- 物理分页速度上并不一定快于逻辑分页，逻辑分页速度上也并不一定快于物理分页
- 物理分页总是优于逻辑分页：没有必要将属于数据库端的压力加到应用端来，就算速度上存在优势，然而其它性能上的优点足以弥补这个缺点



## 13、MyBatis是否支持延迟加载？

Mybatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在MyBatis配置文件中，可以配置是否启用延迟加载

```xml
lazyLoadingEnabled=true|false
```





## 14、说说MyBatis的工作原理

![](Java面试(五).assets/1.png)

1. 读取 MyBatis 配置文件：`mybatis-config.xml` 为 MyBatis 的全局配置文件，包含了 MyBatis 行为的设置和属性信息，例如数据库连接信息和映射文件

2. 加载映射文件mapper.xml。映射文件即 SQL 映射文件，该文件中配置了操作数据库的 SQL 语句，需要在 MyBatis 配置文件 mybatis-config.xml 中加载。mybatis-config.xml 文件可以加载多个映射文件，每个文件对应数据库中的一张表。
3. 构造会话工厂：构建会话工厂 SqlSessionFactory
4. 创建会话对象：由会话工厂 SqlSessionFactory 创建 SqlSession 对象，该对象中包含了执行 SQL 语句的所有方法
5. Executor 执行器：MyBatis 底层定义了一个 Executor 接口来操作数据库，它将根据 SqlSession 传递的参数动态地生成需要执行的 SQL 语句，同时负责查询缓存的维护
6. MappedStatement 对象：在 Executor 接口的执行方法中有一个 MappedStatement 类型的参数，该参数是对映射信息的封装，用于存储要映射的 SQL 语句的 id、参数等信息。



## 15、模糊查询like语句该怎么写

1. `'%${question}'`可能引起SQL注入，不推荐
2. `"%"#{question}"%"` 注意：因为#{...}解析成sql语句时候，会在变量外侧自动加单引号'  '，所以这里 % 需要使用双引号"  "，不能使用单引号 '  '，不然会查不到任何结果
3. `concat('%'),#{question},'%'` ，使用 concat() 函数，推荐
4. 使用bind标签

```xml
<select id="listUserLikeUsername" resultType="com.jourwon.pojo.User">
　　<bind name="pattern" value="'%' + username + '%'" />
　　select id,sex,age,username,password from person where username LIKE #{pattern}
</select>
```







## 16、为什么需要预编译

SQL 预编译指的是数据库驱动在发送 SQL 语句和参数给数据库之前对 SQL 语句进行编译，这样 数据库 执行 SQL 时，就不需要重新编译。

JDBC 中使用对象 PreparedStatement 来抽象预编译语句，使用预编译。预编译阶段可以优化 SQL 的执行。预编译之后的 SQL 多数情况下可以直接执行，数据库 不需要再次编译，Mybatis在默认情况下，将对所有的 SQL 进行预编译。

> 扩展：preparedstatement和statement的区别

statement和preparedstatement都是用来执行SQL语句的接口

- statement是用来执行静态SQL语句的接口，它每次执行都会将SQL语句发送到数据库服务器进行解析和执行
- preparedstatement是用来执行动态SQL语句的接口，它先将SQL语句发送到数据库服务器进行预处理，然后再执行。因为SQL语句已经预处理过了，所以执行效率更高。此外，preparedstatement还可以防止SQL注入攻击。



## 17、当实体类中的属性名和表中的字段名不一样，怎么办

1. 通过在查询的SQL语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```xml
<select id="getOrder" parameterType="int" resultType="com.jourwon.pojo.Order">
      select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
</select>
```

2. 通过`<resultMap>`来映射字段名和实体类属性名的一一对应关系。
   - 用 id 属性来映射主键字段
   - 用 result 属性来映射非主键字段，property 为实体类属性名，column 为数据库表中的属性

```xml
<select id="getOrder" parameterType="int" resultMap="orderResultMap">
select * from orders where order_id=#{id}
</select>

<resultMap type="com.jourwon.pojo.Order" id="orderResultMap">
  <!–用id属性来映射主键字段–>
   <id property="id" column="order_id">

  <!–用result属性来映射非主键字段，property为实体类属性名，column为数据库表中的属性–>
   <result property ="orderno" column ="order_no"/>
   <result property="price" column="order_price" />
</reslutMap>
```



## 18、什么是MyBatis的接口绑定，有哪些实现方式、

接口绑定，就是在MyBatis中任意定义接口，然后把接口里面的方法和SQL语句绑定，我们调用接口方法的时候，最终会执行绑定的SQL语句。

接口绑定有两种实现方式，当Sql语句比较简单时候，可以使用注解绑定，当SQL语句比较复杂时候，一般用xml绑定的比较多。

- 通过注解绑定，就是在接口的方法上面加上 @Select、@Update等注解，里面包含Sql语句来实现接口绑定
- 通过在xml里面写SQL语句来实现绑定， 在这种情况下，要指定xml映射文件里面的namespace必须为接口的全路径名，同时接口的方法名和SQL语句的id一一对应



## 19、使用MyBatis的mapper接口调用时有哪些要求

- Mapper.xml文件中的namespace即是mapper接口的全限定类名
- Mapper接口方法名和mapper.xml中定义的sql语句id一一对应
- Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql语句的parameterType的类型相同
- Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql语句的resultType的类型相同



































































































