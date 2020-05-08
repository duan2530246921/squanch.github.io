# Hibernate
## 113.为什么要使用 hibernate？
---
1. 讲解什么是Hibernate。

Hibernate是一个开放源代码的对象关系映射（ORM)框架，它对JDBC进行了非常轻量级的对象封装，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。

2. 讲解Hibernate的工作原理。

    - 读取并解析配置文件。

    - 读取并解析映射文件。

    - 创建SessionFactory。

    - 打开Session。

    - 创建事务Transaction。

    - 持久化操作。

    - 提交事务。

    - 关闭Session。

    - 关闭SessionFactory。

3. 讲解使用Hibernate的优点。

    - Hibernate对JDBC访问数据库的代码做了封装，大大简化了数据访问层繁琐的重复性代码。

    - Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现，很大程度上简化了dao层编码工作。

    - Hibernate使用java反射机制，而不是字节码增强程序类实现透明性。

    - Hibernate的性能非常好，因为它是一个轻量级框架。映射的灵活性很出色。它支持很多关系型数据库，从一对一到多对多的各种复杂关系。

## 114.什么是 ORM 框架？
---

ORM（Object Relation Mapping）对象关系映射

即通过类与数据库表的映射关系，将对象持久化到数据库中，

常用的有: Hibernate(Nhibernate)，iBATIS，mybatis，EclipseLink，JFinal

## 115.hibernate 中如何在控制台查看打印的 sql 语句？
---
- 打印sql语句到控制台

首先，我使用的是application.properties配置文件，使用yml也可以达到同样的效果。

在网上查这个问题查了好久，基本上都是xml配置，在此不多说；

正确的properties配置项应该如下图所示：

在jpa下一级不直接是hibernate，而是properties。

```
spring.jpa.properties.hibernate.show_sql=true          //控制台是否打印
spring.jpa.properties.hibernate.format_sql=true        //格式化sql语句
spring.jpa.properties.hibernate.use_sql_comments=true  //指出是什么操作生成了该语句
```

- 打印sql语句中的参数值
经过上面的步骤，我们已经可以在控制台打印出格式化之后的sql语句，但是大多数情况下，我们还需要具体的sql参数值，这个时候我们就需要配置 日志配置文件。

博主使用的是slf4j的日志，配置文件用的是logback.xml，配置方式如下：

```
<logger name="org.hibernate.SQL" level="DEBUG"/>
<logger name="org.hibernate.engine.QueryParameters" level="DEBUG"/>
<logger name="org.hibernate.engine.query.HQLQueryPlan" level="DEBUG"/>
```

## 116.hibernate 有几种查询方式？
---
1. NativeSQL 是运用数据库本身提供的数据查询语言进行查询，这种方式查询效率高，与数据库耦合性高依赖于具体的数据库。因为不同的数据库厂商提供的查询语言会存在某些细微差别。

2. HQL 通过Hibernate提供的查询语言进行查询。Hibernate Query lanague

3. EJBQL(JPQL 1.0) 是EJB提供的查询语言

4. QBC(query by cretira)通过Cretira接口进行查询

5. QBE(query by Example) 通过Example编程接口进行查询

从功能强弱上排序：NativeSQL > HQL > EJBQL(JPQL 1.0) >QBC(query by cretira) >QBE(query by Example)

## 117.hibernate 实体类可以被定义为 final 吗？
---
是的，你可以将Hibernate的实体类定义为final类，但这种做法并不好。因为Hibernate会使用代理模式在延迟关联的情况下提高性能，如果你把实体类定义成final类之后，因为 Java不允许对final类进行扩展，所以Hibernate就无法再使用代理了，如此一来就限制了使用可以提升性能的手段。不过，如果你的持久化类实现了一个接口而且在该接口中声明了所有定义于实体类中的所有public的方法轮到话，你就能够避免出现前面所说的不利后果。

## 118.在 hibernate 中使用 Integer 和 int 做映射有什么区别？
---
hibernate的PO类中 经常会用到int 型得变量 这个时候如果使用基本类型的变量（int ） 如果数据库中对应的存储数据是 null 时使用PO类进行获取数据 会出现类型转换异常 如果使用的是对象类型（Integer）这个时候则不会报错。 

## 119.hibernate 是如何工作的？
---
一.工作原理：

1. 读取并解析配置
2. 读取并解析映射信息，创建Session Factory
3. 打开Session
4. 创建事务Transation
5. 持久化操作
6. 提交事务
7. 关闭Session
8. 关闭SesstionFactory

二.为什么要用：
1. 对JDBC访问数据库的代码做了封装，大大简化了数据访问层繁琐的重复性代码。
2. Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现。它很大程度的简化DAO层的编码工作
3. hibernate使用Java反射机制，而不是字节码增强程序来实现透明性。
4. hibernate的性能非常好，因为它是个轻量级框架。映射的灵活性很出色，它支持各种关系数据库，从一对一到多对多的各种复杂关系。

三.Hibernate是如何延迟加载?
   1. Hibernate2延迟加载实现： a)实体对象    b)集合（Collection）

   2. Hibernate3 提供了属性的延迟加载功能
      当Hibernate在查询数据的时候，数据并没有存在与内存中，当程序真正对数据的操作时，对象才存在与内存中，就实现了延迟加载，他节省了服务器的内存开销，从而提高了服务器的性能。 

## 120.get()和 load()的区别？
---
load()没有使用对象的其他属性的时候，没有SQL  延迟加载

get() :没有使用对象的其他属性的时候，也生成了SQL  立即加载

## 121.说一下 hibernate 的缓存机制？
---
Hibernate中的缓存分一级缓存和二级缓存。

一级缓存就是  Session 级别的缓存，在事务范围内有效是,内置的不能被卸载。二级缓存是 SesionFactory级别的缓存，从应用启动到应用结束有效。是可选的，默认没有二级缓存，需要手动开启。保存数据库后，缓存在内存中保存一份，如果更新了数据库就要同步更新。

什么样的数据适合存放到第二级缓存中？

1. 很少被修改的数据   帖子的最后回复时间

2. 经常被查询的数据   电商的地点

3. 不是很重要的数据，允许出现偶尔并发的数据

4. 不会被并发访问的数据

5. 常量数据

## 122.hibernate 对象有哪些状态？
---
hibernate对象三种状态的区分关键在于：

　　a）有没有id

　　b）id在数据库中有没有

　　c）在内存中有没有（session缓存）

三种状态：

　　a） transient ：内存中一个对象，没id，缓存中也没有

　　b）persistent：内存中有对象，缓存中有，数据库中有（id）

　　c）detachd：内存有对象，缓存没有，数据库有

## 123.在 hibernate 中 getCurrentSession 和 openSession 的区别是什么？
---
Hibernate openSession() 和 getCurrentSession的区别

getHiberanteTemplate 、getCurrentSession和OpenSession 

采用getCurrentSession()创建的Session会绑定到当前的线程中去、而采用OpenSession()则不会。

采用getCurrentSession()创建的Session在commit或rollback后会自动关闭，采用OpenSession()必须手动关闭。

采用getCurrentSession()需要在Hibernate.cfg.xml配置文件中加入如下配置：

如果是本地事物，及JDBC一个数据库：

```<propety name=”Hibernate.current_session_context_class”>thread</propety>```

如果是全局事物，及jta事物、多个数据库资源或事物资源：

```<propety name=”Hibernate.current_session_context_class”>jta</propety>```

使用spring的getHiberanteTemplate 就不需要考虑事务管理和session关闭的问题：

openSession创建session时autoCloseSessionEnabled参数为false，即在事物结束后不会自动关闭session，需要手动关闭，如果不关闭将导致session关联的数据库连接无法释放，最后资源耗尽而使程序当掉。              

getCurrentSession创建session时autoCloseSessionEnabled，flushBeforeCompletionEnabled都为true，并且session会同sessionFactory组成一个map以sessionFactory为主键绑定到当前线程。

getCurrentSession():从上下文（配置文件current_session_context_class: thread 使用Connection自动管理；jta(java transaction api) 由Application Server提供的分布式事务管理，Tomcat本身不具备此能力，JBoss、WebLogic具备）找，如果有，则用旧的，否则创建新的，事务提交自动Close；

getCurrentSession本地事务(本地事务:jdbc)时 要在配置文件里进行如下设置：

如果使用的是本地事务（jdbc事务）
```<property name="hibernate.current_session_context_class">thread</property>```
如果使用的是全局事务（jta事务）
```<property name="hibernate.current_session_context_class">jta</property>```

总之：

 getCurrentSession()   使用当前的session
 openSession()         重新建立一个新的session

 在一个应用程序中，如果DAO 层使用Spring 的hibernate 模板，通过Spring 来控制session 的生命周期，则首选getCurrentSession ()。

## 124.hibernate 实体类必须要有无参构造函数吗？为什么？
---
首先答案是肯定的。
原因

Hibernate框架会调用这个默认构造方法来构造实例对象，即Class类的newInstance方法 ，这个方法就是通过调用默认构造方法来创建实例对象的 。

当查询的时候返回的实体类是一个对象实例，是Hibernate动态通过反射生成的。反射的Class.forName(“className”).newInstance()需要对应的类提供一个无参构造方法，必须有个无参的构造方法将对象创建出来，单从Hibernate的角度讲 他是通过反射创建实体对象的 所以没有默认构造方法是不行的，另外Hibernate也可以通过有参的构造方法创建对象。
提醒一点

如果你没有提供任何构造方法，虚拟机会自动提供默认构造方法（无参构造器），但是如果你提供了其他有参数的构造方法的话，虚拟机就不再为你提供默认构造方法，这时必须手动把无参构造器写在代码里，否则new Xxxx()是会报错的，所以默认的构造方法不是必须的，只在有多个构造方法时才是必须的，这里“必须”指的是“必须手动写出来”。
