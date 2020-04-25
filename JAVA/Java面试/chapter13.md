# Mybatis
## 125.mybatis 中 #{}和 ${}的区别是什么？
---
#{}是预编译处理，${}是字符串替换。

1. mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值。

2. mybatis在处理${}时，就是把${}替换成变量的值。

3. 使用#{}可以有效的防止SQL注入，提高系统安全性。原因在于：预编译机制。预编译完成之后，SQL的结构已经固定，即便用户输入非法参数，也不会对SQL的结构产生影响，从而避免了潜在的安全风险。

4. 预编译是提前对SQL语句进行预编译，而其后注入的参数将不会再进行SQL编译。我们知道，SQL注入是发生在编译的过程中，因为恶意注入了某些特殊字符，最后被编译成了恶意的执行操作。而预编译机制则可以很好的防止SQL注入。

## 126.mybatis 有几种分页方式？
---
- 数组分页

查询出全部数据，然后再list中截取需要的部分。

mybatis接口

```List<Student> queryStudentsByArray();```

xml配置文件

```
 <select id="queryStudentsByArray"  resultMap="studentmapper">
        select * from student
 </select>
```

service

接口

```List<Student> queryStudentsByArray(int currPage, int pageSize);```

实现接口

```
 @Override
    public List<Student> queryStudentsByArray(int currPage, int pageSize) {
        //查询全部数据
        List<Student> students = studentMapper.queryStudentsByArray();
        //从第几条数据开始
        int firstIndex = (currPage - 1) * pageSize;
        //到第几条数据结束
        int lastIndex = currPage * pageSize;
        return students.subList(firstIndex, lastIndex); //直接在list中截取
    }
```

controller

```
    @ResponseBody
    @RequestMapping("/student/array/{currPage}/{pageSize}")
    public List<Student> getStudentByArray(@PathVariable("currPage") int currPage, @PathVariable("pageSize") int pageSize) {
        List<Student> student = StuServiceIml.queryStudentsByArray(currPage, pageSize);
        return student;
    }
```
- sql分页

mybatis接口

```List<Student> queryStudentsBySql(Map<String,Object> data);```

xml文件

```
<select id="queryStudentsBySql" parameterType="map" resultMap="studentmapper">
        select * from student limit #{currIndex} , #{pageSize}
</select>
```

service

接口
```List<Student> queryStudentsBySql(int currPage, int pageSize);```
实现类

```
public List<Student> queryStudentsBySql(int currPage, int pageSize) {
        Map<String, Object> data = new HashedMap();
        data.put("currIndex", (currPage-1)*pageSize);
        data.put("pageSize", pageSize);
        return studentMapper.queryStudentsBySql(data);
    }
```

- 拦截器分页

创建拦截器，拦截mybatis接口方法id以ByPage结束的语句

```
package com.autumn.interceptor;

import org.apache.ibatis.executor.Executor;
import org.apache.ibatis.executor.parameter.ParameterHandler;
import org.apache.ibatis.executor.resultset.ResultSetHandler;
import org.apache.ibatis.executor.statement.StatementHandler;
import org.apache.ibatis.mapping.MappedStatement;
import org.apache.ibatis.plugin.*;
import org.apache.ibatis.reflection.MetaObject;
import org.apache.ibatis.reflection.SystemMetaObject;

import java.sql.Connection;
import java.util.Map;
import java.util.Properties;

/**
 * @Intercepts 说明是一个拦截器
 * @Signature 拦截器的签名
 * type 拦截的类型 四大对象之一( Executor,ResultSetHandler,ParameterHandler,StatementHandler)
 * method 拦截的方法
 * args 参数,高版本需要加个Integer.class参数,不然会报错
 */
@Intercepts({@Signature(type = StatementHandler.class, method = "prepare", args = {Connection.class})})
public class MyPageInterceptor implements Interceptor {

    //每页显示的条目数
    private int pageSize;
    //当前现实的页数
    private int currPage;
    //数据库类型
    private String dbType;


    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        //获取StatementHandler，默认是RoutingStatementHandler
        StatementHandler statementHandler = (StatementHandler) invocation.getTarget();
        //获取statementHandler包装类
        MetaObject MetaObjectHandler = SystemMetaObject.forObject(statementHandler);

        //分离代理对象链
        while (MetaObjectHandler.hasGetter("h")) {
            Object obj = MetaObjectHandler.getValue("h");
            MetaObjectHandler = SystemMetaObject.forObject(obj);
        }

        while (MetaObjectHandler.hasGetter("target")) {
            Object obj = MetaObjectHandler.getValue("target");
            MetaObjectHandler = SystemMetaObject.forObject(obj);
        }

        //获取连接对象
        //Connection connection = (Connection) invocation.getArgs()[0];


        //object.getValue("delegate");  获取StatementHandler的实现类

        //获取查询接口映射的相关信息
        MappedStatement mappedStatement = (MappedStatement) MetaObjectHandler.getValue("delegate.mappedStatement");
        String mapId = mappedStatement.getId();

        //statementHandler.getBoundSql().getParameterObject();

        //拦截以.ByPage结尾的请求，分页功能的统一实现
        if (mapId.matches(".+ByPage$")) {
            //获取进行数据库操作时管理参数的handler
            ParameterHandler parameterHandler = (ParameterHandler) MetaObjectHandler.getValue("delegate.parameterHandler");
            //获取请求时的参数
            Map<String, Object> paraObject = (Map<String, Object>) parameterHandler.getParameterObject();
            //也可以这样获取
            //paraObject = (Map<String, Object>) statementHandler.getBoundSql().getParameterObject();

            //参数名称和在service中设置到map中的名称一致
            currPage = (int) paraObject.get("currPage");
            pageSize = (int) paraObject.get("pageSize");

            String sql = (String) MetaObjectHandler.getValue("delegate.boundSql.sql");
            //也可以通过statementHandler直接获取
            //sql = statementHandler.getBoundSql().getSql();

            //构建分页功能的sql语句
            String limitSql;
            sql = sql.trim();
            limitSql = sql + " limit " + (currPage - 1) * pageSize + "," + pageSize;

            //将构建完成的分页sql语句赋值个体'delegate.boundSql.sql'，偷天换日
            MetaObjectHandler.setValue("delegate.boundSql.sql", limitSql);
        }
        //调用原对象的方法，进入责任链的下一级
        return invocation.proceed();
    }


    //获取代理对象
    @Override
    public Object plugin(Object o) {
        //生成object对象的动态代理对象
        return Plugin.wrap(o, this);
    }

    //设置代理对象的参数
    @Override
    public void setProperties(Properties properties) {
        //如果项目中分页的pageSize是统一的，也可以在这里统一配置和获取，这样就不用每次请求都传递pageSize参数了。参数是在配置拦截器时配置的。
        String limit1 = properties.getProperty("limit", "10");
        this.pageSize = Integer.valueOf(limit1);
        this.dbType = properties.getProperty("dbType", "mysql");
    }
}
```

配置文件SqlMapConfig.xml

```

<configuration>

    <plugins>
        <plugin interceptor="com.autumn.interceptor.MyPageInterceptor">
            <property name="limit" value="10"/>
            <property name="dbType" value="mysql"/>
        </plugin>
    </plugins>

</configuration>
```

mybatis配置

```
<!--接口-->
List<AccountExt> getAllBookByPage(@Param("currPage")Integer pageNo,@Param("pageSize")Integer pageSize);
<!--xml配置文件-->
  <sql id="getAllBooksql" >
    acc.id, acc.cateCode, cate_name, user_id,u.name as user_name, money, remark, time
  </sql>
  <select id="getAllBook" resultType="com.autumn.pojo.AccountExt" >
    select
    <include refid="getAllBooksql" />
    from account as acc
  </select>
```

service

```
    public List<AccountExt> getAllBookByPage(String pageNo,String pageSize) {
        return accountMapper.getAllBookByPage(Integer.parseInt(pageNo),Integer.parseInt(pageSize));
    }
```

controller

```

    @RequestMapping("/getAllBook")
    @ResponseBody
    public Page getAllBook(String pageNo,String pageSize,HttpServletRequest request,HttpServletResponse response){
        pageNo=pageNo==null?"1":pageNo;   //当前页码
        pageSize=pageSize==null?"5":pageSize;   //页面大小
        //获取当前页数据
        List<AccountExt> list = bookService.getAllBookByPage(pageNo,pageSize);
        //获取总数据大小
        int totals = bookService.getAllBook();
        //封装返回结果
        Page page = new Page();
        page.setTotal(totals+"");
        page.setRows(list);
        return page;
    }
```

Page实体类

```
package com.autumn.pojo;

import java.util.List;

/**
 * Created by Autumn on 2018/6/21.
 */
public class Page {
    private String pageNo = null;
    private String pageSize = null;
    private String total = null;
    private List rows = null;

    public String getTotal() {
        return total;
    }

    public void setTotal(String total) {
        this.total = total;
    }

    public List getRows() {
        return rows;
    }

    public void setRows(List rows) {
        this.rows = rows;
    }

    public String getPageNo() {
        return pageNo;
    }

    public void setPageNo(String pageNo) {
        this.pageNo = pageNo;
    }

    public String getPageSize() {
        return pageSize;
    }

    public void setPageSize(String pageSize) {
        this.pageSize = pageSize;
    }

}
```

前端

bootstrap-table接受数据格式

```
{
  "total": 3,
  "rows": [
    {
      "id": 0,
      "name": "Item 0",
      "price": "$0"
    },
    {
      "id": 1,
      "name": "Item 1",
      "price": "$1"
    }
  ]
}
```

boostrap-table用法

```
        var $table = $('#table');
        $table.bootstrapTable({
        url: "/${appName}/manager/bookController/getAllBook",
        method: 'post',
        contentType: "application/x-www-form-urlencoded",
        dataType: "json",
        pagination: true, //分页
        sidePagination: "server", //服务端处理分页
        pageList: [5, 10, 25],
        pageSize: 5,
        pageNumber:1,
        //toolbar:"#tb",
        singleSelect: false,
        queryParamsType : "limit",
        queryParams: function queryParams(params) {   //设置查询参数
          var param = {
            pageNo: params.offset/params.limit+1,  //offset为数据开始索引,转换为显示当前页
            pageSize: params.limit  //页面大小
          };
          console.info(params);   //查看参数是什么
          console.info(param);   //查看自定义的参数
          return param;
        },
        cache: false,
        //data-locale: "zh-CN", //表格汉化
        //search: true, //显示搜索框
        columns: [
                {
                    checkbox: true
                },
                {
                    title: '消费类型',
                    field: 'cate_name',
                    valign: 'middle'
                },
                {
                    title: '消费金额',
                    field: 'money',
                    valign: 'middle',
                    formatter:function(value,row,index){
                        if(!isNaN(value)){   //是数字
                            return value/100;
                        }
                    }
                },
                {
                    title: '备注',
                    field: 'remark',
                    valign: 'middle'
                },
                {
                    title: '消费时间',
                    field: 'time',
                    valign: 'middle'
                },
                {
                    title: '操作',
                    field: '',
                    formatter:function(value,row,index){
                        var f = '<a href="#" class="btn btn-gmtx-define1" onclick="delBook(\''+ row.id +'\')">删除</a> ';
                        return f;
                       }
                }
            ]
          });
      });
```

- RowBounds分页

数据量小时，RowBounds不失为一种好办法。但是数据量大时，实现拦截器就很有必要了。
mybatis接口加入RowBounds参数

```
public List<UserBean> queryUsersByPage(String userName, RowBounds rowBounds);

service

    @Override
    @Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.SUPPORTS)
    public List<RoleBean> queryRolesByPage(String roleName, int start, int limit) {
        return roleDao.queryRolesByPage(roleName, new RowBounds(start, limit));
    }

```

## 127.RowBounds 是一次性查询全部结果吗？为什么？
---
RowBounds是一次性查询全部结果

从RowBounds源码看出，RowBounds最大数据量为Integer.MAX_VALUE(2147483647),大概是20亿条

在实际开发不建议使用RowBounds。数据量达到一定程度，RowBounds所造成的内存压力比较大

## 128.mybatis 逻辑分页和物理分页的区别是什么？
---
1. 物理分页

物理分页就是数据库本身提供了分页方式，如MySQL的limit，oracle的rownum ，好处是效率高，不好的地方就是不同数据库有不同的搞法。

2. 逻辑分页

逻辑分页利用游标分页，好处是所有数据库都统一，坏处就是效率低。

3. 常用ORM框架采用的分页技术

- hibernate采用的是物理分页；

- MyBatis使用RowBounds实现的分页是逻辑分页,也就是先把数据记录全部查询出来,然在再根据offset和limit截断记录返回（数据量大的时候会造成内存溢出），不过可以用插件或其他方式能达到物理分页效果。

 mybatis的物理分页插件：

常见的两种： Mybatis-Paginator Mybatis-PageHelper

   为了在数据库层面上实现物理分页,又不改变原来MyBatis的函数逻辑,可以编写plugin截获MyBatis Executor的statementhandler,重写SQL来执行查询

分页结论：

1. 物理分页速度上并不一定快于逻辑分页，逻辑分页速度上也并不一定快于物理分页。
2. 物理分页总是优于逻辑分页：没有必要将属于数据库端的压力加诸到应用端来，就算速度上存在优势,然而其它性能上的优点足以弥补这个缺点。
3. 在分页工作前，有必要了解使用数据库本身的一些sql语句特点更好的分页

## 129.mybatis 是否支持延迟加载？延迟加载的原理是什么？
---
mybatis支持延迟加载
 
适用场景
一对一，多对一   立即加载
一对多，多对多   延迟加载

## 130.说一下 mybatis 的一级缓存和二级缓存？
---
Mybatis首先去缓存中查询结果集，如果没有则查询数据库，如果有则从缓存取出返回结果集就不走数据库。Mybatis内部存储缓存使用一个HashMap，key为hashCode+sqlId+Sql语句。value为从查询出来映射生成的java对象
Mybatis的二级缓存即查询缓存，它的作用域是一个mapper的namespace，即在同一个namespace中查询sql可以从缓存中获取数据。二级缓存是可以跨SqlSession的。

## 131.mybatis 和 hibernate 的区别有哪些？
---
1. 两者相同点
Hibernate和Mybatis的二级缓存除了采用系统默认的缓存机制外，都可以通过实现你自己的缓存或为其他第三方缓存方案，创建适配器来完全覆盖缓存行为。

2. 两者不同点
Hibernate的二级缓存配置在SessionFactory生成的配置文件中进行详细配置，然后再在具体的表-对象映射中配置是那种缓存。而MyBatis在使用二级缓存时需要特别小心。如果不能完全确定数据更新操作的波及范围，避免Cache的盲目使用。否则，脏数据的出现会给系统的正常运行带来很大的隐患。

3. 举个形象的比喻
MyBatis：机械工具，使用方便，拿来就用，但工作还是要自己来作，不过工具是活的，怎么使由我决定。（小巧、方便、高效、简单、直接、半自动）

Hibernate：智能机器人，但研发它（学习、熟练度）的成本很高，工作都可以摆脱他了，但仅限于它能做的事。（强大、方便、高效、复杂、绕弯子、全自动）

## 132.mybatis 有哪些执行器（Executor）？
---
- SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

- ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map内，供下一次使用。简言之，就是重复使用Statement对象。

- BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

作用范围：Executor的这些特点，都严格限制在SqlSession生命周期范围内。

## 133.mybatis 分页插件的实现原理是什么？
---
分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。
## 134.mybatis 如何编写一个自定义插件？
---
MyBatis 自定义插件针对 MyBatis 四大对象（Executor,StatementHandler,ParamentHandler,ResultSetHandler）进行拦截。

- Executor  ：拦截内部执行器，它负责调用StatementHandler 操作数据库，并把结果集通过 ResultSetHandler 进行自动映射，另外它还处理了二级缓存的操作。
- StatementHandler  ：拦截 SQL 语法构建的处理，它是MyBatis 直接和数据库执行 SQL脚本的对象，另外它也实现了 MyBatis一级缓存
- ParamenterHandler  ：拦截参数的处理。
- ResultSetHandler  ：拦截结果集的处理。