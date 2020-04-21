# Java Web
## 64.jsp 和 servlet 有什么区别？
---
- Servlet：
 Servlet 是一种服务器端的Java应用程序，具有独立于平台和协议的特性，可以生成动态的Web页面。它担当客户请求（Web浏览器或其他HTTP客户程序）与服务器响应（HTTP服务器上的数据库或应用程序）的中间层。 Servlet是位于Web服务器内部的服务器端的Java应用程序，与传统的从命令行启动的Java应用程序不同，Servlet由Web服务器进行加载，该Web服务器必须包含支持Servlet的Java虚拟机。
- Jsp：
 JSP 全名为Java Server Pages，中文名叫java服务器页面，其根本是一个简化的Servlet设计。JSP技术使用Java编程语言编写类XML的tags和scriptlets，来封装产生动态网页的处理逻辑。网页还能通过tags和scriptlets访问存在于服务端的资源的应用逻辑。JSP将网页逻辑与网页设计的显示分离，支持可重用的基于组件的设计，使基于Web的应用程序的开发变得迅速和容易。 JSP(JavaServer Pages)是一种动态页面技术，它的主要目的是将表示逻辑从Servlet中分离出来。
- 相同点
jsp经编译后就变成了servlet，jsp本质就是servlet，jvm只能识别java的类，不能识别jsp代码，web容器将jsp的代码编译成jvm能够识别的java类。
- 不同点
JSP侧重视图，Sevlet主要用于控制逻辑。
Servlet中没有内置对象 。
JSP中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到。

## 65.jsp 有哪些内置对象？作用分别是什么？
---
**JSP共有以下9个内置的对象：**
- request 用户端请求，此请求会包含来自GET/POST请求的参数
- response 网页传回用户端的回应
- pageContext 网页的属性是在这里管理
- session 与请求有关的会话期
- application servlet 正在执行的内容
- out 用来传送回应的输出
- config servlet的构架部件
- page JSP网页本身
- exception 针对错误网页，未捕捉的例外
- request表示HttpServletRequest对象。它包含了有关浏览器请求的信息，并且提供了几个用于获取cookie, header,和session数据的有用的方法。
- response表示HttpServletResponse对象，并提供了几个用于设置送回浏览器的响应的方法（如cookies,头信息等）
- out对象是javax.jsp.JspWriter的一个实例，并提供了几个方法使你能用于向浏览器回送输出结果。
- pageContext表示一个javax.servlet.jsp.PageContext对象。它是用于方便存取各种范围的名字空间、servlet相关的对象的API，并且包装了通用的servlet相关功能的方法。
- session表示一个请求的javax.servlet.http.HttpSession对象。Session可以存贮用户的状态信息
- applicaton 表示一个javax.servle.ServletContext对象。这有助于查找有关servlet引擎和servlet环境的信息
- config表示一个javax.servlet.ServletConfig对象。该对象用于存取servlet实例的初始化参数。
- page表示从该页面产生的一个servlet实例

**JSP共有以下6种基本动作**
- jsp:include：在页面被请求的时候引入一个文件。
- jsp:useBean：寻找或者实例化一个JavaBean。
- jsp:setProperty：设置JavaBean的属性。
- jsp:getProperty：输出某个JavaBean的属性。
- jsp:forward：把请求转到一个新的页面。
- jsp:plugin：根据浏览器类型为Java插件生成OBJECT或EMBED标记

## 66.说一下 jsp 的 4 种作用域？
---
作用域|名称|描述
:--:|:--:|:--:
page|当前页面作用域|相当于 Java 关键字中 this。在这个作用域中存放的属性值，只能在当前页面中取出。
request|请求作用域|范围是从请求创建到请求消亡这段时间，一个请求可以涉及的多个页面。<jsp:forward>和<jsp:include>跳转到其他页面，也在作用域范围。
session|会话作用域|范围是一段客户端和服务端持续连接的时间，用户在会话有效期内多次请求所涉及的页面。seesion会话器，服务端为第一次建立连接的客户端分配一段有效期内的属性内存空间。
application|全局作用域|范围是服务端Web应用启动到停止，整个Web应用中所有请求所涉及的页面。当服务器开启时，会创建一个公共内存区域，任何客户端都可以在这个公共内存区域存取值。

## 67.session 和 cookie 有什么区别？
---
1. 存储位置不同
>cookie的数据信息存放在客户端浏览器上。
>session的数据信息存放在服务器上。
2. 存储容量不同
>单个cookie保存的数据<=4KB，一个站点最多保存20个Cookie。
>对于session来说并没有上限，但出于对服务器端的性能考虑，session内不要存放过多的东西，并且设置session删除机制。
3. 存储方式不同
>cookie中只能保管ASCII字符串，并需要通过编码方式存储为Unicode字符或者二进制数据。
>session中能够存储任何类型的数据，包括且不限于string，integer，list，map等。
4. 隐私策略不同
>cookie对客户端是可见的，别有用心的人可以分析存放在本地的cookie并进行cookie欺骗，所以它是不安全的。
>session存储在服务器上，对客户端是透明对，不存在敏感信息泄漏的风险。
5. 有效期上不同
>开发可以通过设置cookie的属性，达到使cookie长期有效的效果。
>session依赖于名为JSESSIONID的cookie，而cookie JSESSIONID的过期时间默认为-1，只需关闭窗口该session就会失效，因而session不能达到长期有效的效果。
6. 服务器压力不同
>cookie保管在客户端，不占用服务器资源。对于并发用户十分多的网站，cookie是很好的选择。
>session是保管在服务器端的，每个用户都会产生一个session。假如并发访问的用户十分多，会产生十分多的session，耗费大量的内存。
7. 浏览器支持不同
>假如客户端浏览器不支持cookie：
>cookie是需要客户端浏览器支持的，假如客户端禁用了cookie，或者不支持cookie，则会话跟踪会失效。关于WAP上的应用，常规的cookie就派不上用场了。
>运用session需要使用URL地址重写的方式。一切用到session程序的URL都要进行URL地址重写，否则session会话跟踪还会失效。
>假如客户端支持cookie：
>cookie既能够设为本浏览器窗口以及子窗口内有效，也能够设为一切窗口内有效。
>session只能在本窗口以及子窗口内有效。
8. 跨域支持上不同
>cookie支持跨域名访问。
>session不支持跨域名访问。

## 68.说一下 session 的工作原理？
---
**session是什么**

首先，我们需要知道session是什么。有比较专业的人将session称之为会话控制。说实在的，如果这么说的话，我也不清楚session到底算是什么。
其实session是一个存在服务器上的类似于一个散列表格的文件。里面存有我们需要的信息，在我们需要用的时候可以从里面取出来。类似于一个大号的map吧，里面的键存储的是用户的sessionid，用户向服务器发送请求的时候会带上这个sessionid。这时就可以从中取出对应的值了。

**session有什么用**

说起session的作用，简单的举个例子：我们在登录某些网站的时候，输入了用户名密码，登录以后再打开新的页面时，自动显示的是已登录的状态，不需要再次重新登录。这里就是session功能的一个小小的体现。那么，刚才这个小小的应用发生了什么呢？
<img src="/_markdownimg/session1.png" alt="图片名称" align=center />

如图所示：在用户1和用户2登录的时候，我们的服务器在他们登录成功后，在session表中为他们每个用户分配了一个sessionid并且存下了一个对应的信息。当用户第二次访问该服务器的时候，会将sessionid在request请求中携带者发送过去。这时我们的服务器就可以根据sessionid确定用户存储的数据，然后进行使用。如图所示：
<img src="/_markdownimg/session2.png" alt="图片名称" align=center />

**session的生命周期**

当session超过一定时间（一般为30分钟）没有被访问时，服务器就会认为这个session对应的客户端已经停止活动，然后将这个session删除。用以节省空间。
当用户关闭浏览器时，sessionId的信息会丢失，虽然服务器session还在，依然无法访问到session中的数据

## 69.如果客户端禁止 cookie 能实现 session 还能用吗？
---
一般默认情况下，在会话中，服务器存储 session 的 sessionid 是通过 cookie 存到浏览器里。如果浏览器禁用了 cookie，浏览器请求服务器无法携带 sessionid，服务器无法识别请求中的用户身份，session失效。但是可以通过其他方法在禁用 cookie 的情况下，可以继续使用session。
1. 通过url重写，把 sessionid 作为参数追加的原 url 中，后续的浏览器与服务器交互中携带 sessionid 参数。
2. 服务器的返回数据中包含 sessionid，浏览器发送请求时，携带 sessionid 参数。
3. 通过 Http 协议其他 header 字段，服务器每次返回时设置该 header 字段信息，浏览器中 js 读取该 header 字段，请求服务器时，js设置携带该 header 字段。

## 70.spring mvc 和 struts 的区别是什么？
---
1. Struts2是类级别的拦截， 一个类对应一个request上下文，SpringMVC是方法级别的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应,所以说从架构本身上SpringMVC就容易实现restful url,而struts2的架构实现起来要费劲，因为Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。
2. 由上边原因，SpringMVC的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，方法之间不共享变量，而Struts2搞的就比较乱，虽然方法之间也是独立的，但其所有Action变量是共享的，这不会影响程序运行，却给我们编码 读程序时带来麻烦，每次来了请求就创建一个Action，一个Action对象对应一个request上下文。
3. 由于Struts2需要针对每个request进行封装，把request，session等servlet生命周期的变量封装成一个一个Map，供给每个Action使用，并保证线程安全，所以在原则上，是比较耗费内存的。
4.  拦截器实现机制上，Struts2有以自己的interceptor机制，SpringMVC用的是独立的AOP方式，这样导致Struts2的配置文件量还是比SpringMVC大。
5. SpringMVC的入口是servlet，而Struts2是filter（这里要指出，filter和servlet是不同的。以前认为filter是servlet的一种特殊），这就导致了二者的机制不同，这里就牵涉到servlet和filter的区别了。
6. SpringMVC集成了Ajax，使用非常方便，只需一个注解@ResponseBody就可以实现，然后直接返回响应文本即可，而Struts2拦截器集成了Ajax，在Action中处理时一般必须安装插件或者自己写代码集成进去，使用起来也相对不方便。
7. SpringMVC验证支持JSR303，处理起来相对更加灵活方便，而Struts2验证比较繁琐，感觉太烦乱。
8. Spring MVC和Spring是无缝的。从这个项目的管理和安全上也比Struts2高（当然Struts2也可以通过不同的目录结构和相关配置做到SpringMVC一样的效果，但是需要xml配置的地方不少）。
9. 设计思想上，Struts2更加符合OOP的编程思想， SpringMVC就比较谨慎，在servlet上扩展。
10. SpringMVC开发效率和性能高于Struts2。
11. SpringMVC可以认为已经100%零配置。

## 71.如何避免 sql 注入？
---
- 严格限制 Web 应用的数据库的操作权限，给连接数据库的用户提供满足需要的最低权限，最大限度的减少注入攻击对数据库的危害
- 校验参数的数据格式是否合法（可以使用正则或特殊字符的判断）
- 对进入数据库的特殊字符进行转义处理，或编码转换
- 预编译 SQL（Java 中使用 PreparedStatement），参数化查询方式，避免 SQL 拼接
- 发布前，利用工具进行 SQL 注入检测
- 报错信息不要包含 SQL 信息输出到 Web 页面

## 72.什么是 XSS 攻击，如何避免？
---
**概念：**
XSS（Cross Site Scripting），即跨站脚本攻击，是一种常见于web应用程序中的计算机安全漏洞.XSS通过在用户端注入恶意的可运行脚本，若服务器端对用户输入不进行处理，直接将用户输入输出到浏览器，然后浏览器将会执行用户注入的脚本。

**举例：**
有一个input输入框，需要用户输入名字，用户却输入一个恶意脚本，<script>alert(document.cookie)</script>，
或者用户输入一个HTML，在标签中嵌入恶意脚本，如src，href，css style等。如：<IMG SRC="javascript:alert('XSS');">;
<BODY BACKGROUND="javascript:alert('XSS')">

**防御：**
其恶意脚本都是来自用户的输入。因此，可以使用过滤用户输入的方法对恶意脚本进行过滤。
1. 获取用户输入，不用.innerHTML，用innerText。
2. 对用户输入进行过滤，如 HTMLEncode 函数实现应该至少进行 & < > " ' / 等符号转义成 &amp &lt &gt &quot &#x27 &#x2F；

## 73.什么是 CSRF 攻击，如何避免？
---
>CSRF：Cross Site Request Forgery（跨站点请求伪造）。
>CSRF 攻击者在用户已经登录目标网站之后，诱使用户访问一个攻击页面，利用目标网站对用户的信任，以用户身份在攻击页面对目标网站发起伪造用户操作的请求，达到攻击目的。

**CSRF 攻击实例**
避免方法：
- CSRF 漏洞进行检测的工具，如 CSRFTester、CSRF Request Builder...
- 验证 HTTP Referer 字段
- 添加并验证 token
- 添加自定义 http 请求头
- 敏感操作添加验证码
- 使用 post 请求
