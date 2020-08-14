# Servlet介绍

- 1、Servlet（Server Applet），全称Java Servlet。==是用Java编写的服务器端程序，其主要功能在于交互式地浏览和修改数据，生成动态Web内容。==狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。

- 2、 Servlet运行于支持Java的应用服务器中。从实现上讲，Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来==扩展基于HTTP协议的Web服务器==。
- 3、Servlet工作模式：
  - 客户端发送请求至服务器。
  - 服务器启动大概并调用Servlet，Servlet根据客户端请求生成响应内容并将其传给服务器。
  - 服务器将响应并返回客户端。

# Servlet API

- javax.serv letinterface Servlet
  - 1.init()
  - 2.service()
  - 3.destroy()
  - 4.getServletConfig()
  - 5.getServletInfo()
- javax.servlet abstract GenericServlet（除了实现或继承Servlet接口中的五个方法外还提供了额外方法）
  - 1.getInitParameter()
  - 2.getServletName()
  - 3.getServletContext()
- javax.servlet.http abstract HttpServlet（重载service()方法）
  - 1.doGet()
  - 2.doPost()

# Servlet工作原理

(1) Servlet接口定义了Servlet与servlet容器之间的契约。这个契约是：Servlet容器将Servlet类载入内存，并产生
Servlet实例和调用它具体的方法。但是要注意的是，在一个应用程序中，每种Servlet类型只能有一个实例。
(2)用户请求致使Servlet容器调用Servlet的Service（）方法，并传入一个ServletRequest对象和一个
ServletResponse对象。ServletRequest对象和ServletResponse对象都是由Servlet容器（例如TomCat）封
装好的，并不需要程序员去实现，程序员可以直接使用这两个对象。
(3)ServletRequest中封装了当前的Http请求，因此，开发人员不必解析和操作原始的Http数据。ServletResponse
表示当前用户的Http响应，程序员只需直接操作ServletResponse对象就能把响应轻松的发回给用户。
(4)对于每一个应用程序，Servlet容器还会创建一个ServletContext对象。这个对象中封装了上下文（应用程序）
的环境详情。每个应用程序只有一个ServletContext。每个Servlet对象也都有一个封装Servlet配置的
ServletConfig对象。

# Servlet的生命周期

当客户端首次发送第一次请求后，由容器(web服务器(tomcat))去解析请求, 根据请求找到对应的servlet,判断该类的对象是否存在，不存在则创建servlet实例，调取init()方法 进行初始化操作,初始化完成后调取service()方法,由service()判断客户端的请求方式，如果是get，则执行doGet(),如果是post则执行doPost().处理方法完成后,作出相应结果给客户端.单次请求处理完毕。
当用户发送第二次以后的请求时,会判断对象是否存在,但是不再执行init()，而直接执行service方法,调取doGet()/doPost()方法。
当服务器关闭时调取destroy()方法进行销毁。
四个过程:
 (1)实例化 --先创建servlet实例
(2)初始化 --init()
(3)处理请求 ---service()
(4)服务终止 --destory()

# 请求

HttpServletRequest表示Http环境中的Servlet请求。它扩展于javax.servlet.ServletRequest接口)

## 常用方法

- 1、String getParameter(String name) 根据表单组件名称获取提交数据，返回值是String
  注：服务器在接收数据时使用字符串统一接收
- 2、String[ ] getParameterValues(String name) 获取表单组件对应多个值时的请求数据
- 3、void setCharacterEncoding(String charset) 指定每个请求的编码(针对post请求才起作用)
- 4、RequestDispatcher getRequestDispatcher(String path) --跳转页面

返回一个RequestDispatcher对象，该对象的forward( )方法用于转发请求

```java
示例：request.getRequestDispatcher("../success.jsp").forward(request,response);
```

- 5、存值 request.setAttribute("key",value);
- 6、取值 request.getAttribute("key");//取值后需要向下转型

# 响应

在Service API中，定义了一个HttpServletResponse接口，它继承自ServletResponse接口，专门用来封装HTTP响应消息。 在HttpServletResponse接口中定义了向客户端发送响应状态码，响应消息头，响应消息体的方法。

**常用方法：**

void addCookie(Cookie var1)：给这个响应添加一个cookie

void sendRedirect(String var1) ：发送一条响应码，将浏览器跳转到指定的位置

PrintWriter getWriter() ：获得字符流，通过字符流的write(String s)方法可以将字符串设置到response 缓冲区中，随后Tomcat会将response缓冲区中的内容组装成Http响应返回给浏览器端。

setContentType() ：设置响应内容的类型。

**重定向和转发的对比：**

重定向:response.sendRedirect()

 转发:request.getRequestDispatcher("../success.jsp").forward(request,response);

相同点:都用来跳转页面。

不同点:

- 重定向时地址栏会改变,request中存储的数据会丢失.转发时地址栏显示的是请求页面的地址,request数据可以保存。
- 转发属于一次请求一次响应,重定向属于两次请求(地址栏修改了两次)两次响应。

补充:使用out对象往页面中输出js或html,css

```java
out.print("<script type='text/javascript'>alert('登录失败');location='../login.jsp'</script>");
```

注:使用js跳转页面，也会丢失request中的数据

# 会话

**会话（session）的概念:**从打开浏览器到关闭浏览器,期间访问服务器就称为一次会话

**request & session区别和联系：**

- request存的值只能在单次请求中保存，保存的数据不能跨页面,当重定向时,request存的值会丢失。

- session的数据可以在多个页面中共享,即使重定向页面,数据不会丢失。

- session中可以包含n个request。

**常用方法：**

- void setAttribute(String key,Object value) ：以key/value的形式保存对象值,将数据存储在服务器端

- Object getAttribute(String key) ：通过key获取对象值

- void invalidate() ：设置session对象失效

- String getId() ：获取sessionid,当第一次登录成功后，session会产生一个唯一的id，浏览器之后访问时如果发现id值还是之前id，那么说明当前访问的属于同一个会话

- void setMaxInactiveInterval(int interval) ：设定session的非活动时间

  - 方式一：

  ```java
  session.setMaxInactiveInterval(10*60);//设置有效时间为10分钟
  ```

  - 方式二：

  ```java
  <session-config>
  <session-timeout>10</session-timeout>//单位:分钟
  </session-config>
  ```

- int getMaxInactiveInterval() ：获取session的有效非活动时间(以秒为单位)，默认的有效时间:30分钟.
- void removeAttribute(String key)：从session中删除指定名称(key)所对应的对象
- 小结 :让session失效的方式
  （1）invalidate() （2）removeAttribute("key") （3）直接关闭浏览器。
  示例:使用session验证用户是否登录
  补充:
  自动刷新到某页面:
  注:在head标签中添加该标签，单位:秒

# 获得初始化参数

request.setCharacterEncoding("utf-8");代码的耦合度太高，不便于后期维护修改。可以通过初始化参数实现。

**实现方式:**

**一、方式一**：

- 1、web.xml中先定义初始化参数

```java
<servlet>
<servlet-name></servlet-name>
<servlet-class></servlet-class>
<init-param>
<param-name>encoding</param-name>
<param-value>utf-8</param-value>
</init-param>
</servlet>
```

- 2、servlet中获得初始化参数，重写init()方法

```jav
public void init(ServletConfig config) throws ServletException {
encoding= config.getInitParameter("encoding");
}
```

注意:这种方式的初始化参数仅限于当前servlet中使用。

**二、方式二全局初始化参数**

- 1、定义，context-param是和servlet标签同级别

```java
<context-param>
<param-name>bianma</param-name>
<param-value>utf-8</param-value>
</context-param>
```



# servlet3.0

从Servlet3.0开始，配置Servlet支持注解方式，但还是保留了配置web.xml方式，所有使用Servlet有两种方式：

（1）Servlet类上使用@WebServlet注解进行配置

（2）web.xml文件中配置

![image-20200806164423354](C:\Users\jasmi\AppData\Roaming\Typora\typora-user-images\image-20200806164423354.png)

==注意==

- .loadOnStartup属性：标记容器是否在启动应用时就加载Servlet，默认不配置或数值为负数时表示客户端第一次请求Servlet时再加载；0或正数表示启动应用就加载，正数情况下，数值越小，加载该Servlet的优先级越高；

```java
@WebServlet(value="/test1",loadOnStartup=1)
```

- .name属性：可以指定也可以不指定，通过getServletName()可以获取到，若不指定，则为Servlet的完整类名，如：cn.edu.njit.servlet.UserServlet

- .urlPatterns/value属性： String[]类型，可以配置多个映射，如：urlPatterns={"/user/test", "/user/example"}
- .在使用注解方式时，需要注意：
  根元素中不能配置属性metadata-complete="true"，否则无法加载Servlet。metadata-complete属性表示通知
  Web容器是否寻找注解，默认不写或者设置false，容器会扫描注解，为Web应用程序构建有效的元数据；metadata-complete="true"，会在启动时不扫描注解（annotation）。如果不扫描注解的话，用注解进行的配置就无法生效，例如：@WebServlet
- .urlPatterns的常用规则：
  - /*或者/：拦截所有
  - *.do：拦截指定后缀
  - /user/test：拦截路径
  - /user/.do、/.do、test*.do都是非法的，启动时候会报错