### JSP内置对象

> Servlet中内置对象:request 、response、session、application、out(PrintWriter)
>
> Jsp本质是Servlet，包含九个内置对象
>

| 对象名          | 类型                                   | 说明                        |
| --------------- | -------------------------------------- | --------------------------- |
| request         | javax.servlet.http.HttpServletRequest  |                             |
| response        | javax.servlet.http.HttpServletResponse |                             |
| session         | javax.servlet.http.HttpSession         | 由session=“true”开关        |
| application     | javax.servlet.ServletContext           |                             |
| config          | javax.servlet.ServletConfig            |                             |
| exception       | java.lang.Throwable                    | 由isErrorPage=“false”开关   |
| **out**         | javax.servlet.jsp.JspWriter            | javax.servlet.jsp.JspWriter |
| **pageContext** | javax.servlet.jsp.PageContext          |                             |
| page            | java.lang.Object当前对象this           | 当前servlet实例             |

#### request

> request 对象是javax.servlet.http.HttpServletRequest类型的对象，代表客户端的请求信息，主要用于获取客户端的参数和流。
>
> 作用域 ：request(用户请求期）

| 序号 | 方法名                                   | 作用                                    |
| :--- | ---------------------------------------- | --------------------------------------- |
| 1    | object getAttribute(String name)         | 返回指定属性的属性值                    |
| 2    | Enumeration getAttributeNames()          | 返回所有可用属性名的枚举                |
| 3    | String getCharacterEncoding()            | 返回字符编码方式                        |
| 4    | int getContentLength()                   | 返回请求体的长度（以字节数）            |
| 5    | String getContentType()                  | 得到请求体的MIME类型                    |
| 6    | ServletInputStream getInputStream()      | 得到请求体中一行的二进制流              |
| 7    | String getParameter(String name)         | 返回name指定参数的参数值                |
| 8    | Enumeration getParameterNames()          | 返回可用参数名的枚举                    |
| 9    | String[] getParameterValues(String name) | 返回包含参数name的所有值的数组          |
| 10   | String getProtocol()                     | 返回请求用的协议类型及版本号            |
| 11   | String getScheme()                       | 返回请求用的计划名,如:http.https及ftp等 |
| 12   | String getServerName()                   | 返回接受请求的服务器主机名              |
| 13   | int getServerPort()                      | 返回服务器接受此请求所用的端口号        |
| 14   | BufferedReader getReader()               | 返回解码过了的请求体                    |
| 15   | String getRemoteAddr()                   | 返回发送此请求的客户端IP地址            |
| 16   | String getRemoteHost()                   | 返回发送此请求的客户端主机名            |
| 17   | void setAttribute(String key,Object obj) | 设置属性的属性值                        |
| 18   | String getRealPath(String path)          | 返回一虚拟路径的真实路径                |

#### response

> response 对象和request是一对相应的内置对象，代表对客户端的响应，需要向客户端发送文字时直接使用。它是HttpServletResponse类的实例。
>
> 作用域 ：page（页面执行期）

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
| 1    | String getCharacterEncoding()                                | 返回响应用的是何种字符编码         |
| 2    | ServletOutputStream getOutputStream()                        | 返回响应的一个二进制输出流         |
| 3    | PrintWriter getWriter()                                      | 返回可以向客户端输出字符的一个对象 |
| 4    | void setContentLength(int len)                               | 设置响应头长度                     |
| 5    | void setContentType(String type)                             | 设置响应的MIME类型                 |
| 6    | sendRedirect([Java](http://lib.csdn.net/base/javaee).lang.String location) | 重新定向客户端的请求               |

#### session

> session 对象是由服务器自动创建的与请求相关的对象，服务器为每个用户都生成一个session对象，用于保存该用户的信息，跟踪用户的操作状态。session内部使用Map来保存数据，即key-value对。它是HttpSession类的实例。
>
> 作用域  ：session(会话期—）

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1| long getCreationTime()| 返回SESSION创建时间 |
|2 |public String getId() |返回SESSION创建时JSP引擎为它设的惟一ID号 |
|3 |long getLastAccessedTime() |返回此SESSION里客户端最近一次请求时间 |
|4 |int getMaxInactiveInterval() |返回两次请求间隔多长时间此SESSION被取消(ms) |
|5 |String[] getValueNames() |返回一个包含此SESSION中所有可用属性的数组 |
|6 |void invalidate()| 取消SESSION，使SESSION不可用 |
|7 |boolean isNew() |返回服务器创建的一个SESSION,客户端是否已经加入|
|8 |void removeValue(String name) |删除SESSION中指定的属性|
|9 |void setMaxInactiveInterval()|设置两次请求间隔多长时间此SESSION被取消(ms)|

#### application

> 实现了用户间数据的共享，可存放全局变量。可将信息保存在服务器中，直到服务器关闭，否则application对象中保存的信息会整个应用中都有。服务器的启动和关闭决定了application对象的生命。
>
> 它是javax.servlet.ServletContex类的实例。
>
> 作用域 ：application

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1|Object getAttribute(String name)|返回给定名的属性值|
|2|Enumeration getAttributeNames()|返回所有可用属性名的枚举|
|3| void setAttribute(String name,Object obj)|设定属性的属性值|
|4|void removeAttribute(String name)| 删除一属性及其属性值|
|5|String getServerInfo() |返回JSP(SERVLET)引擎名及版本号|
|6|String getRealPath(String path)| 返回一虚拟路径的真实路径|
|7|ServletContext getContext(String uripath)|返回指定WebApplication的application对象|
|8|int getMajorVersion() |返回服务器支持的Servlet API的最大版本号|
|9|int getMinorVersion()| 返回服务器支持的Servlet API的最小版本号|
|10|String getMimeType(String file)|返回指定文件的MIME类型|
|11|URL getResource(String path)|返回指定资源(文件及目录)的URL路径|
|12|InputStream getResourceAsStream(String path)|返回指定资源的输入流|
|13|**RequestDispatcher getRequestDispatcher(String uripath)**|返回指定资源的RequestDispatcher对象|
|14|Servlet getServlet(String name)|返回指定名的Servlet|
|15|Enumeration getServlets()|返回所有Servlet的枚举|
|16|Enumeration getServletNames()|返回所有Servlet名的枚举|
|17|void log(String msg)|把指定消息写入Servlet的日志文件|
|18|void log(Exception exception,String msg)|把指定异常的栈轨迹及错误消息写入Servlet的日志文件|
|19|void log(String msg,Throwable throwable)|把栈轨迹及给出的Throwable异常的说明信息 写入Servlet的日志文件|

#### config

> config 对象是javax.servlet.ServletConfig类的实例对象。
>
> 主要作用是取得服务器的配置信息。通过 pageConext对象的 getServletConfig() 方法可以获取一个config对象。当一个Servlet 初始化时，容器把某些信息通过 config对象传递给这个 Servlet。
>
> 开发者可以在web.xml 文件中为应用程序环境中的Servlet程序和JSP页面提供初始化参数。
>
> config对象是在一个Servlet初始化时，JSP引擎向它传递信息用的，此信息包括Servlet初始化时所要用到的参数（通过属性名和属性值构成）以及服务器的有关信息（通过传递一个ServletContext对象。
>
> 作用域 ：Page

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1 |ServletContext getServletContext() |返回含有服务器相关信息的ServletContext对象 |
|2| String getInitParameter(String name) |返回初始化参数的值 |
|3 |Enumeration getInitParameterNames()|返回Servlet初始化所需所有参数的枚举|

#### exception

> exception对象：是一个例外对象，当一个页面在运行过程中发生了例外，就产生这个对象。如果一个JSP页面要应用此对象，就必须把isErrorPage设为true，否则无法编译。他实际上是java.lang.Throwable的对象。
>
> 作用域 ：page

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1 |String getMessage()| 返回描述异常的消息 |
|2 |String toString() |返回关于异常的简短描述消息 |
|3| void printStackTrace() |显示异常及其栈轨迹 |
|4 |Throwable FillInStackTrace() |重写异常的执行栈轨迹|

#### out

> out 对象用于Web浏览器内输出信息，负责管理对客户端的输出。并且管理应用服务器上的输出缓冲区。在使用out对象输出数据时，可以对数据缓冲区进行操作，及时清理缓冲区中的残留数据。out对象是JspWriter类的实例,是向客户端输出内容常用的对象 。
>
> 作用域 ：page（页面执行期）

**jsp的out和getWriter()方法的区别**

```java
out.print(48);
response.getWriter().print("OK");
//浏览器顶部显示ok 不在html内
//48在下面
1 out是JspWriter类型，getWriter()是PrintWriter类型
2 out输出到缓冲区中，没有写到response中，getWriter()直接写到response中。
3 out一般用在jsp中，getWriter()用在Servlet中
```
| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1| void clear() |清除缓冲区的内容 |
|2| void clearBuffer()| 清除缓冲区的当前内容|
|3|void flush()| 清空流 |
|4|int getBufferSize()| 返回缓冲区以字节数的大小，如不设缓冲区则为0|
|5|int getRemaining()|返回缓冲区还剩余多少可用|
|6|boolean isAutoFlush()| 返回缓冲区满时，是自动清空还是抛出异常|
|7|void close()|关闭输出流 |

#### pageContext

> pageContext 对象的作用是取得任何范围的参数，通过它可以获取 JSP页面的out、request、reponse、session、application 等对象。
>
> pageContext对象的创建和初始化都是由容器来完成的，在JSP页面中可以直接使用 pageContext对象。
>
> 他相当于页面中所有功能的集大成者，它的本类名也叫pageContext。
>
> 作用域 ：Pageconfig对象


| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1| JspWriter getOut() |返回当前客户端响应被使用JspWriter(out)|
|2 |HttpSession getSession() |返回当前页中的HttpSession对象(session) |
|3 |Object getPage() |返回当前页的Object对象(page)|
|4 |ServletRequest getRequest()|返回当前页的ServletRequest对象(request)|
|5 |ServletResponse getResponse() |返回当前页的ServletResponse(response)|
|6 |Exception getException()| 返回当前页的Exception对象(exception) |
|7 |ServletConfig getServletConfig()| 返回当前页的ServletConfig对象(config) |
|8 |ServletContext getServletContext()| 返回当前页的ServletContext(application) |
|9 |void setAttribute(String name,Object attribute)| 设置属性及属性值 |
|10| void setAttribute(String name,Object obj,int scope) |在指定范围内设置属性及属性值 |
|11| public Object getAttribute(String name) |取属性的值 |
|12| Object getAttribute(String name,int scope) |在指定范围内取属性的值|
|13| public Object findAttribute(String name) |寻找一属性,返回起属性值或NULL|
|14| void removeAttribute(String name)| 删除某属性 |
|15| void removeAttribute(String name,int scope)| 在指定范围删除某属性 |
|16 |int getAttributeScope(String name) |返回某属性的作用范围 |
|17| Enumeration getAttributeNamesInScope(int scope)| 返回指定范围内可用的属性名枚举 |
|18| void release() |释放pageContext所占用的资源 |
|19| void forward(String relativeUrlPath) |使当前页面重导到另一页面 |
|20| void include(String relativeUrlPath) |在当前位置包含另一文件 |

```java
    void setAttribute(String name,Object o);
    Object getAttribute(String name);
    void removeAttribute(String name);

操作其它域对象的方法
    void setAttribute(String name,Object o,int Scope);
    Object getAttribute(String name,int Scope);
    void removeAttribute(String name,int Scope);
Scope作用域，值如下:
    PageContext.PAGE_SCOPE
    PageContext.REQUEST_SCOPE
    PageContext.SESSION_SCOPE
    PageContext.APPLICATION_SCOPE

findAttribute(Stringname)自动从pageContext,request ,session ,application依次查找，
                         找到了就取值，结束查找  （作用域的范围由小到大）
```

```java
getException //方法返回exception隐式对象 
getPage //方法返回page隐式对象
getRequest //方法返回request隐式对象 
getResponse //方法返回response隐式对象 
getServletConfig //方法返回config隐式对象
getServletContext //方法返回application隐式对象
getSession //方法返回session隐式对象 
getOut //方法返回out隐式对象
```

```json
pageContext.forward(“2.jsp”);//转发  request.getRequestDispatcher().forward();
pageContext.include(“2.jsp”);//动态包含
```

#### page

> page对象就是指向当前JSP页面本身，有点象类中的this指针，它是java.lang.Object类的实例 。“page” 对象代表了正在运行的由JSP文件产生的类对象。
>
>  作用域  ：page（页面执行期）

| 序号 | 方法名                                                       | 作用                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
|1| class getClass |返回此Object的类|
|2| int hashCode()             |返回此Object的hash码|
|3|boolean equals(Object obj)|判断此Object是否与指定的Object对象相等|
|4|void copy(Object obj)|把此Object拷贝到指定的Object对象中|
|5|Object clone()|克隆此Object对象|
|6|String toString()|把此Object对象转换成String类的对象|
|7|void notify()|唤醒一个等待的线程|
|8|void notifyAll()|唤醒所有等待的线程|
|9|void wait(int timeout)|使一个线程处于等待直到timeout结束或被唤醒 |
|10|void wait() |使一个线程处于等待直到被唤醒|
|11|void enterMonitor()|对Object加锁|
|12|void exitMonitor()|对Object开锁|

```
JSP的四大作用域：1.page 2.request 3.session 4.application。    

其中page作用域最小，application作用域最大。

存储在application对象中的属性可以被同一个Web应用程序中的所有Servlet和Jsp页面访问（属性作用范围最大）

存储在session对象中的属性可以被同一个会话的所有Servlet和Jsp页面访问。

存储在request对象中的属性可以被同一个请求的所有Servlet和Jsp页面访问。

存储在pageContext对象中的属性仅可以被当前Jsp页面的当前响应过程中调用的各个组件访问，例如，正在响应当前请求的Jsp页面和它调用的各个自定义标签类。
```

