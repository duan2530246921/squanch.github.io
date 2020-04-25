# Spring/Spring MVC
## 90.为什么要使用 spring？
---
spring 是一个开源的轻量级 JavaBean 容器框架。使用 JavaBean 代替 EJB ，并提供了丰富的企业应用功能，降低应用开发的复杂性。
- 轻量：非入侵性的、所依赖的东西少、资源占用少、部署简单，不同功能选择不同的 jar 组合
- 容器：工厂模式实现对 JavaBean 进行管理，通过控制反转（IOC）将应用程序的配置和依赖性与应用代码分开
- 松耦合：通过 xml 配置或注解即可完成 bean 的依赖注入
- AOP：通过 xml 配置 或注解即可加入面向切面编程的能力，完成切面功能，如：日志，事务...的统一处理
- 方便集成：通过配置和简单的对象注入即可集成其他框架，如 Mybatis、Hibernate、Shiro...
- 丰富的功能：JDBC 层抽象、事务管理、MVC、Java Mail、任务调度、JMX、JMS、JNDI、EJB、动态语言、远程访问、Web Service... 

## 91.解释一下什么是 aop？
---
aop 是面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。 简单来说就是统一处理某一“切面”（类）的问题的编程思想，比如统一处理日志、异常等。

## 92.解释一下什么是 ioc？
---
ioc：Inversionof Control（中文：控制反转）是 spring 的核心，对于 spring 框架来说，就是由 spring 来负责控制对象的生命周期和对象间的关系。 简单来说，控制指的是当前对象对内部成员的控制权；控制反转指的是，这种控制权不由当前对象管理了，由其他（类,第三方容器）来管理。

## 93.spring 有哪些主要模块？
---
1. Spring Core
框架的最基础部分，提供 IoC 容器，对 bean 进行管理。

2. Spring Context
基于 bean，提供上下文信息，扩展出JNDI、EJB、电子邮件、国际化、校验和调度等功能。

3. Spring DAO
提供了JDBC的抽象层，它可消除冗长的JDBC编码和解析数据库厂商特有的错误代码，还提供了声明性事务管理方法。

4. Spring ORM
提供了常用的“对象/关系”映射APIs的集成层。 其中包括JPA、JDO、Hibernate、MyBatis 等。

5. Spring AOP
提供了符合AOP Alliance规范的面向方面的编程实现。

6. Spring Web
提供了基础的 Web 开发的上下文信息，可与其他 web 进行集成。

7. Spring Web MVC
提供了 Web 应用的 Model-View-Controller 全功能实现。

## 94.spring 常用的注入方式有哪些？
---
1、xml中配置
- bean 的申明、注册
```<bean>``` 节点注册 bean
```<bean>``` 节点的 factory-bean 参数指工厂 bean，factory-method 参数指定工厂方法
- bean 的注入
```<property>``` 节点使用 set 方式注入
```<constructor-arg>``` 节点使用 构造方法注入

实测代码

maven pom 文件

```
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-beans</artifactId>
    	<version>4.2.4.RELEASE</version>
    </dependency>
     
    <dependency>
    	<groupId>org.springframework</groupId>
    	<artifactId>spring-context</artifactId>
    	<version>4.2.4.RELEASE</version>
    </dependency>
```

A、 ```<bean> + <property>```，set方法注入

```
class Bowl

    package constxiong.interview.inject;
     
    public class Bowl {
     
    	public void putRice() {
    		System.out.println("盛饭...");
    	}
     
    }
```

```
class Person

    package constxiong.interview.inject;
     
    public class Person {
     
    	private Bowl bowl;
    	
    	public void eat() {
    		bowl.putRice();
    		System.out.println("开始吃饭...");
    	}
     
    	public void setBowl(Bowl bowl) {
    		this.bowl = bowl;
    	}
    	
    }
```

spring 配置文件

```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans 
    		http://www.springframework.org/schema/beans/spring-beans.xsd
    	    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
            
    	<bean id="bowl" class="constxiong.interview.inject.Bowl" />
    	
    	<bean id="person" class="constxiong.interview.inject.Person">
    		<property name="bowl" ref="bowl"></property>
    	</bean>
    	
    </beans>
```

测试类

```
    package constxiong.interview.inject;
     
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
     
    public class InjectTest {
     
    	public static void main(String[] args) {
    		ApplicationContext context = new ClassPathXmlApplicationContext("spring_inject.xml");
    		Person person = (Person)context.getBean("person");
    		person.eat();
    	}
    }
``` 

B、修改为 配置文件和class Person，```<bean> + <constructor-arg>``` 节点使用 构造方法注入

```
class Person

    package constxiong.interview.inject;
     
    public class Person {
     
    	private Bowl bowl;
    	
    	public Person(Bowl bowl) {
    		this.bowl = bowl;
    	}
    	
    	public void eat() {
    		bowl.putRice();
    		System.out.println("开始吃饭...");
    	}
    	
    }
```

spring 配置文件

```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans 
    		http://www.springframework.org/schema/beans/spring-beans.xsd
    	    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
            
    	<bean id="bowl" class="constxiong.interview.inject.Bowl" />
    	
    	<bean id="person" class="constxiong.interview.inject.Person">
    		<constructor-arg name="bowl" ref="bowl"></constructor-arg>
    	</bean>
    	
    </beans>
```
 

C、```<bean>``` 节点 factory-method 参数指定静态工厂方法

工厂类，静态工厂方法

```
    package constxiong.interview.inject;
     
    public class BowlFactory {
     
    	public static final Bowl getBowl() {
    		return new Bowl();
    	}
    	
    }
```

spring 配置文件

```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans 
    		http://www.springframework.org/schema/beans/spring-beans.xsd
    	    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
            
    	<bean id="bowl" class="constxiong.interview.inject.BowlFactory" factory-method="getBowl"/>
    	
    	<bean id="person" class="constxiong.interview.inject.Person">
    		<constructor-arg name="bowl" ref="bowl"></constructor-arg>
    	</bean>
    	
    </beans>
```

D、非静态工厂方法，需要指定工厂 bean 和工厂方法

工厂类，非静态工厂方法

```
    package constxiong.interview.inject;
     
    public class BowlFactory {
     
    	public Bowl getBowl() {
    		return new Bowl();
    	}
    	
    }
```

配置文件

```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans 
    		http://www.springframework.org/schema/beans/spring-beans.xsd
    	    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
        
        <bean id="bowlFactory" class="constxiong.interview.inject.BowlFactory"></bean>   
    	<bean id="bowl" factory-bean="bowlFactory" factory-method="getBowl"/>
    	
    	<bean id="person" class="constxiong.interview.inject.Person">
    		<constructor-arg name="bowl" ref="bowl"></constructor-arg>
    	</bean>
    	
    </beans>
```


2. 注解
- bean 的申明、注册
@Component //注册所有bean
@Controller //注册控制层的bean
@Service //注册服务层的bean
@Repository //注册dao层的bean
- bean 的注入
@Autowired 作用于 构造方法、字段、方法，常用于成员变量字段之上。
@Autowired + @Qualifier 注入，指定 bean 的名称
@Resource JDK 自带注解注入，可以指定 bean 的名称和类型等

测试代码

E、spring 配置文件，设置注解扫描目录

```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xsi:schemaLocation="
    		http://www.springframework.org/schema/beans 
    		http://www.springframework.org/schema/beans/spring-beans.xsd
    	    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
            
    	<context:component-scan base-package="constxiong.interview" />
    	
    </beans>
```

```
class Bowl

    package constxiong.interview.inject;
     
    import org.springframework.stereotype.Component;
    //import org.springframework.stereotype.Controller;
    //import org.springframework.stereotype.Repository;
    //import org.springframework.stereotype.Service;
     
    @Component //注册所有bean
    //@Controller //注册控制层的bean
    //@Service //注册服务层的bean
    //@Repository //注册dao层的bean
    public class Bowl {
     
    	public void putRice() {
    		System.out.println("盛饭...");
    	}
     
    }
```

```
class Person

    package constxiong.interview.inject;
     
    //import javax.annotation.Resource;
    //
    import org.springframework.beans.factory.annotation.Autowired;
    //import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.stereotype.Component;
     
    @Component //注册所有bean
    //@Controller //注册控制层的bean
    //@Service //注册服务层的bean
    //@Repository //注册dao层的bean
    public class Person {
     
    	@Autowired
    //	@Qualifier("bowl")
    //	@Resource(name="bowl")
    	private Bowl bowl;
     
    	public void eat() {
    		bowl.putRice();
    		System.out.println("开始吃饭...");
    	}
    	
    }
```

测试类同上
A、B、C、D、E 测试结果都ok

```
    盛饭...
    开始吃饭...
```

## 95.spring 中的 bean 是线程安全的吗？
---
spring 管理的 bean 的线程安全跟 bean 的创建作用域和 bean 所在的使用环境是否存在竞态条件有关，spring 并不能保证 bean 的线程安全。

## 96.spring 支持几种 bean 的作用域？
---
- singleton：单例模式，在整个Spring IoC容器中，使用 singleton 定义的 bean 只有一个实例
- prototype：原型模式，每次通过容器的getbean方法获取 prototype 定义的 bean 时，都产生一个新的 bean 实例
只有在 Web 应用中使用Spring时，request、session、global-session 作用域才有效
- request：对于每次 HTTP 请求，使用 request 定义的 bean 都将产生一个新实例，即每次 HTTP 请求将会产生不同的 bean 实例。
- session：同一个 Session 共享一个 bean 实例。
- global-session：同 session 作用域不同的是，所有的Session共享一个Bean实例。

## 97.spring 自动装配 bean 有哪些方式？
---
spring 配置文件中 ```<bean>``` 节点的 autowire 参数可以控制 bean 自动装配的方式
- default - 默认的方式和 "no" 方式一样
- no - 不自动装配，需要使用 <ref />节点或参数
- byName - 根据名称进行装配
- byType - 根据类型进行装配
- constructor - 根据构造函数进行装配

## 98.spring 事务实现方式有哪些？
---
1. 编程式事务管理对基于 POJO 的应用来说是唯一选择。我们需要在代码中调用beginTransaction()、commit()、rollback()等事务管理相关的方法，这就是编程式事务管理。
2. 基于 TransactionProxyFactoryBean的声明式事务管理
3. 基于 @Transactional 的声明式事务管理
4. 基于Aspectj AOP配置事务

## 99.说一下 spring 的事务隔离？
---
1. read uncommited：是最低的事务隔离级别，它允许另外一个事务可以看到这个事务未提交的数据。
2. read commited：保证一个事物提交后才能被另外一个事务读取。另外一个事务不能读取该事物未提交的数据。
3. repeatable read：这种事务隔离级别可以防止脏读，不可重复读。但是可能会出现幻象读。它除了保证一个事务不能被另外一个事务读取未提交的数据之外还避免了以下情况产生（不可重复读）。
4. serializable：这是花费最高代价但最可靠的事务隔离级别。事务被处理为顺序执行。除了防止脏读，不可重复读之外，还避免了幻象读
5. 脏读、不可重复读、幻象读概念说明：
a.脏读：指当一个事务正字访问数据，并且对数据进行了修改，而这种数据还没有提交到数据库中，这时，另外一个事务也访问这个数据，然后使用了这个数据。因为这个数据还没有提交那么另外一个事务读取到的这个数据我们称之为脏数据。依据脏数据所做的操作肯能是不正确的。
b.不可重复读：指在一个事务内，多次读同一数据。在这个事务还没有执行结束，另外一个事务也访问该同一数据，那么在第一个事务中的两次读取数据之间，由于第二个事务的修改第一个事务两次读到的数据可能是不一样的，这样就发生了在一个事物内两次连续读到的数据是不一样的，这种情况被称为是不可重复读。
c.幻象读：一个事务先后读取一个范围的记录，但两次读取的纪录数不同，我们称之为幻象读（两次执行同一条 select 语句会出现不同的结果，第二次读会增加一数据行，并没有说这两次执行是在同一个事务中）

## 100.说一下 spring mvc 运行流程？
---
1. 用户向服务器发送请求，请求被 Spring 前端控制 Servelt DispatcherServlet 捕获(捕获)

2. DispatcherServlet对请求  URL进行解析，得到请求资源标识符（URI）。然后根据该  URI，调用 HandlerMapping获得该Handler配置的所有相关的对象（包括  Handler对象以及   Handler对象对应的拦截器），最后以 HandlerExecutionChain对象的形式返回；(查找   handler)

3. DispatcherServlet  根据获得的 Handler，选择一个合适的  HandlerAdapter。  提取Request 中的模型数据，填充 Handler 入参，开始执行 Handler（Controller), Handler执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象(执行  handler)

4. DispatcherServlet  根据返回的 ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver) (选择  ViewResolver)

5. 通过 ViewResolver 结合 Model 和 View，来渲染视图,DispatcherServlet 将渲染结果返回给客户端。（渲染返回）

快速记忆技巧：

核心控制器捕获请求、查找Handler、执行Handler、选择ViewResolver,通过ViewResolver渲染视图并返回

## 101.spring mvc 有哪些组件？
---
- 前端控制器（DispatcherServlet） 
- 处理器映射器（HandlerMapping） 
- 处理器适配器（HandlerAdapter） 
- 拦截器（HandlerInterceptor）
- 语言环境处理器（LocaleResolver）
- 主题解析器（ThemeResolver）
- 视图解析器（ViewResolver） 
- 文件上传处理器（MultipartResolver）
- 异常处理器（HandlerExceptionResolver） 
- 数据转换（DataBinder）
- 消息转换器（HttpMessageConverter）
- 请求转视图翻译器（RequestToViewNameTranslator）
- 页面跳转参数管理器（FlashMapManager）
- 处理程序执行链（HandlerExecutionChain） 

## 102.@RequestMapping 的作用是什么？
---
@RequestMapping 是一个注解，用来标识 http 请求地址与 Controller 类的方法之间的映射。

可作用于类和方法上，方法匹配的完整是路径是 Controller 类上 @RequestMapping 注解的 value 值加上方法上的 @RequestMapping 注解的 value 值。

## 103.@Autowired 的作用是什么？
---
@Autowired 是一个注释，它可以对类成员变量、方法及构造函数进行标注，让 spring 完成 bean 自动装配的工作。
@Autowired 默认是按照类去匹配，配合 @Qualifier 指定按照名称去装配 bean。