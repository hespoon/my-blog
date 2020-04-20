---
title: JSP 学习笔记
date: 2020-04-20 09:22:39
tags:
---
# JSP Learn

JSP 技术以 Java 语言作为脚本语言。

使用 JSP 标签在 HTML 网页中插入 Java 代码。

JSP 标签通常以 `<%` 开头，以 `%>` 结尾。

JSP 通过网页表单获取用户输入数据、访问数据库及其他数据源，然后动态地创建网页。

JSP 标签有多种功能，比如访问数据库、记录用户选择信息、访问 JavaBeans 组件等，还可以在不同的网页中传递控制信息和共享信息。

JSP 直接在 HTML 网页中动态嵌入元素。

服务器调用的是已经编译好的 JSP 文件。

JSP 基于 Java Servlet API。

JSP 可以与处理业务逻辑的 Servlet 一起使用。

网络服务器需要一个 JSP 引擎，也就是一个容器来处理 JSP 界面，容器负责截获 JSP 页面的请求。

Apache 内嵌了 JSP 容器。

JSP 容器与 Web 服务器协同合作，为 JSP 的正常运行提供必要的运行环境和其他服务，并且能够正确识别专属于 JSP 网页的特殊元素。

## JSP 处理

Web 服务器使用 JSP 创建网页的过程

1. 客户端发送 HTTP 请求给服务器。
2. Web 服务器识别出这是一个对 JSP 网页的请求，并将该请求传递给 JSP 引擎。
3. JSP 引擎从磁盘中载入 JSP 文件，然后将它们转换为 Servlet。这种转换只是简单的将所有模版文本改用 println() 语句，并且将所有的 JSP 元素转换为 Java 代码。
4. JSP 引擎将 Servlet 编译成可执行类，并且将原始请求传递给 Servlet 引擎。
5. Web 服务器的某组件会调用 Servlet 引擎，然后载入并执行 Servlet 类。在执行过程中，Servlet 会产生 HTML 格式的输出并将其内嵌于 HTTP response 中上交给 Web 服务器。
6. Web 服务器以静态 HTML 网页的形式将 HTTP response 返回到客户端。
7. 最终，客户端处理 HTTP response 中动态产生的 HTML 网页，就好像处理静态网页。

一般情况下，JSP 引擎会检查 JSP 文件对应的 Servlet 是否已经存在，并且检查 JSP 文件的修改日期是否早于 Servlet。如果 JSP 文件的修改日期早于对应的 Servlet，那么 JSP 容器就可以确定 Servlet 文件有效并直接处理该 Servlet 不再执行 JSP 到 Servlet 的转换，加快执行流程。

JSP 网页就是用另一种方式编写 Servlet 而不用称为 Java 编程高手。

除了解释阶段外，JSP 网页几乎可以被当做一个普通的 Servlet 来对待。

## JSP 生命周期

1. 编译阶段

   解析 JSP 文件，将 JSP 文件转换为 Servlet，编译 Servlet

2. 初始化阶段

   加载与 JSP 对应的 Servlet 类，创建其实例，并调用它的初始化方法。

   JSP 容器会在为请求提供任何服务前调用 jspInit() 方法。

   如果要执行自定义的 JSP 初始化任务，复写 jspInit() 方法即可。

   ```JAVA
   public void jspInit(){
     // 初始化代码
   }
   ```

   

3. 执行阶段

   调用与 JSP 对应的 Servlet 实例的服务方法。

   该阶段执行一切与请求相关的交互行为，直到 Servlet 实例被销毁。

   当 JSP 网页初始化完成后，JSP 引擎会调用 _jspService() 方法。

   _jspService() 方法需要一个 HttpServletRequest 对象和一个 HttpServletResponse 对象作为它的参数。

   ```JAVA
   void _jspService(HttpServletRequest request,
                    HttpServletResponse response)
   {
      // 服务端处理代码
   }
   ```

   _jspService() 方法在每个 request 中被调用一次并且负责产生与之相对应的 response，并且还负责产生所有 7 个 HTTP 方法的回应。

4. 销毁阶段

   调用与 JSP 对应的 Servlet 实例的销毁方法，然后销毁 Servlet 实例。

   销毁阶段描述了当以个 JSP 网页从容器中被移除是所发生的一切。

   销毁时会调用 jspDestroy() 方法。

   当需要执行自定义的销毁任务时，复写 jspDestory() 方法即可。

   ```JAVA
   public void jspDestroy()
   {
      // 清理代码
   }
   ```

## JSP 语法

### 脚本程序

脚本程序可以包含任意量的 Java 语句、变量、方法或表达式。

语法格式如下：

```JSP
<% code %>
```

在 JSP 文件头部添加一下代码以正确显示中文：

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

### JSP 声明

在 JSP 文件中，必须先声明再使用。

一个声明语句可以包含一个或多个变量和方法。

```JSP
<%! declaration;[declaration;]+...%>
// 示例
<%! int i=0;%>
<!% Circle a=new Circle(2.0);%>
```

### JSP 表达式

一个 JSP 表达式中包含的脚本语言的表达式的值会被先转换为 String，然后插入到表达式出现的地方。

由于表达式的值会被转换为 String，因此可以在一个文本行中使用表达式而不用管它是否是 HTML标签。

JSP 表达式中的脚本语言的表达式不能使用分号来表示结束。

```JSP
<%= expression %>
// 示例
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
    <head>
        <meat charset="utf-8"></meat>
    </head>
    <body>
        <p>
            今天的日期是：<%=(new java.util.Date()).toLocaleString()%>
        </p>
    </body>
</html>
```

输出

```
今天的日期是：2020-4-17 10:28:21
```

### JSP 注释

```JSP
<%-- 注释 --%>
```

JSP 注释的内容不会被发送到浏览器甚至不会被编译。

### JSP 指令

JSP 指令用于设置与整个 JSP 页面相关的属性。

```JSP
<%@ directive attribute="value" %>
```

共有三种指令标签

| 指令                                         | 描述                                                                                |
| -------------------------------------------- | ----------------------------------------------------------------------------------- |
| <%@ page attribute=“value”%>                 | 定义页面的依赖属性，比如脚本语言，error 页面、缓存需求等                            |
| <%@ include file=“文件相对 url 地址”%>       | 包含其他文件                                                                        |
| <%@ taglib uri=“uri” prefix=”prefixOfTag” %> | 引入标签库的定义，可以是自定义标签。uri 指明标签库的位置，prefix 指明标签库的前缀。 |

page 指令为容器提供当前页面的使用说明。一个 JSP 页面可以包含多个 page 指令。

include 指令用于包含文件。被包含的文件可以使 JSP 文件、HTML 文件或文本文件。被包含的文件就像是该 JSP 文件的一部分，会被同时编译执行。JSP 编译器默认在当前路径下寻找被包含文件。

JSP API 允许用户自定义标签，一个自定义标签库就是自定义标签的集合。Taglib 指令引入一个自定义标签集合的定义，包括库路径、自定义标签。

### JSP 行为

JSP 行为标签使用 XML 语法结构来控制 servlet 引擎。JSP 动作元素在请求处理阶段起作用。它能够动态插入一个文件，重用 JavaBean 组件，引导用户去另一个页面，为 Java 插件产生相关的 HTML 等等。

行为标签只有一种语法格式，它严格遵守 XML 标准：

```JSP
<jsp:action_name attribute="value" />
```

每个动作元素都有两个属性：id 和 scope

id 属性是动作元素的唯一标识，可以在 JSP 页面引用。动作元素创建的 id 值可以通过 PageContext 来调用。

scope 属性用于识别动作元素的声明周期。scope 属性有四个可能的值：page、request、session、application

### JSP 隐含对象

JSP 支持九个自动定义的变量，江湖人称隐含对象，可以直接使用。

JSP 隐式对象是 JSP 容器为每个页面提供的 Java 对象，开发者可以直接使用而不用显示声明。隐式对象也被称为预定义变量。

### 控制流语句

JSP 提供对 Java 语言的全面支持。可以在 JSP 程序中使用 Java API 甚至建立 Java 代码块，包括判断语句和循环语句等。

**request 对象**

request 对象是 javax.servlet.http.HttpServletRequest 类的实例。每当客户端请求一个 JSP 页面时，JSP 引擎就会制造一个新的 request 对象来代表这个请求。

request 对象提供了一系列方法来获取 HTTP 头信息，cookies，HTTP 方法等。

**response 对象**

response 对象是 javax.servlet.http.HttpServletResponse 类的实例。当服务器创建 request 对象时会同时创建用于响应这个客户端的 response 对象。

response 对象定义了处理 HTTP 头模块的接口。通过这个对象，开发者可以添加新的 cookies，时间戳，HTTP 状态码等。

**out 对象**

out 对象是 javax.servlet.jsp.JspWrite 类的实例，用来向 response 对象中写入内容。

最初的 JspWrite 类对象根据页面是否有缓存来进行不同的实例化操作。可以在 page 指令中使用 buffered=‘false’ 属性来轻松关闭缓存。

JspWriter 类包含了大部分 java.io.PrintWriter 类中的方法。同时，JspWriter 新增了一些专为处理缓存而设计的方法。JspWriter 类会抛出 IOExceptions 异常，而 PrintWriter 不会。

out.flush() 刷新输出流

out.println(data Type dt) 输出 Type 类型的值然后换行

out.print(data Type dt) 输出 Type 类型的值

**session 对象**

session 对象是 javax.servlet.http.HttpSession 类的实例。和 Java Servlets 中的 session 对象有一样的行为。

**application 对象**

application 对象是 javax.servlet.ServletContext 类的实例。

这个对象在 JSP 页面的整个生命周期中都代表着这个 JSP 页面。这个对象在 JSP 页面初始化时创建，随着 jspDestory() 方法的调用而被移除。

**config 对象**

config 对象是 javax.servlet.ServletConfig 类的实例，直接包装了 servlet 的 Servletconfig 类的对象。

这个对象允许开发者访问 Servlet 或者 JSP 引擎的初始化参数。

**pageContext 对象**

pageContext 对象是 javax.jsp.PageContext 类的实例，用来代表整个 JSP 页面。

这个对象主要用来访问页面信息，同时过滤掉大部分实现细节。

这个对象存储了 request 对象和 response 对象的引用。application 对象，config 对象，session 对象，out 对象可以通过访问这个对象的属性来导出。

pageContext 对象也包含了传给 JSP 页面的指令信息，包括缓存信息，ErrorPage URL，页面 scope 等。

**page 对象**

这个对象就是页面实例的引用，可以被看作整个 JSP 页面的代表。

page 对象就是 this 对象的同义词。

**exception 对象**

exception 对象包装了从当前页面中抛出的异常信息。它通常被用来产生对出错条件的适当响应。

### 判断语句

**if…else**

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%! int day = 3; %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h3>IF...ELSE 实例</h3>
<% if (day == 1 | day == 7) { %>
      <p>今天是周末</p>
<% } else { %>
      <p>今天不是周末</p>
<% } %>
</body> 
</html> 
```

输出

```
IF...ELSE 实例
今天不是周末
```

**switch…case**

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%! int day = 3; %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h3>SWITCH...CASE 实例</h3>
<% 
switch(day) {
case 0:
   out.println("星期天");
   break;
case 1:
   out.println("星期一");
   break;
case 2:
   out.println("星期二");
   break;
case 3:
   out.println("星期三");
   break;
case 4:
   out.println("星期四");
   break;
case 5:
   out.println("星期五");
   break;
default:
   out.println("星期六");
}
%>
</body> 
</html> 
```

输出

```
SWITCH...CASE 实例

星期三
```

### 循环语句

**for**

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%! int fontSize; %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h3>For 循环实例</h3>
<%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
   <font color="green" size="<%= fontSize %>">
    菜鸟教程
   </font><br />
<%}%>
</body> 
</html> 
```

输出

![JSP-for.jpg](http://ww1.sinaimg.cn/large/006XJF4Oly1gdwksclipkj30h805wt8z.jpg)

**while**

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%! int fontSize=0; %> 
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h3>While 循环实例</h3>
<%while ( fontSize <= 3){ %>
   <font color="green" size="<%= fontSize %>">
    菜鸟教程
   </font><br />
<%fontSize++;%>
<%}%>
</body> 
</html> 
```

输出

![JSP-while.jpg](http://ww1.sinaimg.cn/large/006XJF4Oly1gdwkt3qccdj30ju06saag.jpg)

### JSP 运算符

JSP 支持所有 Java 的逻辑和算数运算符。

### JSP 字面量

* boolean：true 和 false
* int：与 Java 中一样
* float：与 Java 中一样
* string：以单引号或双引号开始或结束
* Null：null

## JSP 客户端请求

每当客户端请求一个页面时，JSP 引擎就会产生一个新的 javax.servlet.http.HttpServletRequest 类的实例来代表这个请求。实例名称为 request。request 对象提供了一系列方法来获取 HTTP 信息头，包括表单数据，cooikes，HTTP 方法等。

## JSP 服务器响应

Response 响应对象主要将 JSP 容器处理后的结果传回到客户端。可以通过 response 变量设置 HTTP 的状态和向客户端发送数据，如 Cooike、HTTP 文件头信息等。

一个典型的相应看起来就像下面这样：

```HTTP
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (空行)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

response 对象使 javax.servlet.http.HttpServletResponse 类的一个实例。服务器对每一个客户端的请求都会创建一个 response 对象，代表对客户端的响应。

response 对象定义了处理 HTTP 信息头的接口。通过使用这个对象，开发者们可以添加新的 cooike 或时间戳、HTTP 状态码等。

## JSP 表单处理

浏览器中使用 GET 和 POST 方法向服务器提交数据。

### Get 方法

GET 方法将请求的编码信息添加到网址后面，网址与编码信息通过 `?` 分割。

```
http://www.runoob.com/hello?key1=value1&key2=value2
```

GET 方法是浏览器默认传递参数的方法，一些敏感信息，如密码等不应使用 GET 方法。

使用 GET 时，传输数据的大小有限制，最大为 1024 字节。

### POST 方法

POST 提交数据是隐式的，一些敏感数据可以使用 POST 方法传递。

### JSP 读取表单数据

* getParameter()，使用 request.getParameter() 方法来获取表单参数的值。
* getParameterValues()，获得如 checkbox 类的数据，接收数组变量。
* getParameterNames()，该方法可以取得所有变量的名称，返回一个 Enumeration。
* getInputStream()，调用此方法读取来自客户端的二进制数据流。 

## JSP 过滤器

JSP 中的过滤器是 Java 类。

过滤器可以动态地拦截请求和响应，以变换或使用包含在请求或响应中的信息。

可以将过滤器附加到 JSP 文件和 HTML 页面。

过滤器可以在客户端的请求访问后端资源之前，拦截这些请求。

过滤器可以在服务器的响应发送回客户端之前，处理这些响应。

规范推荐有一下几种过滤器：

* 身份验证过滤器
* 数据压缩过滤器
* 加密过滤器
* 触发资源访问事件过滤器
* 图像转换过滤器
* 日志记录和审核过滤器
* MIME-TYPE 链过滤器
* 标记化过滤器
* XSL/T 过滤器

过滤器通过 Web 部署描述符（web.xml）中的 XML 标签来声明，然后映射到应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为部署描述符中声明的每一个过滤器创建一个实例。

Fliter 的执行顺序与在 web.xml 配置文件中的配置顺序一致，一般把 Filter 配置在所有 Servlet 之前。

![过滤器的过滤过程.png](http://ww1.sinaimg.cn/large/006XJF4Oly1gdyvspbqyfj30lj0ctt9m.jpg)

**Servlet 过滤器方法**

过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类。

## JSP Cooike 处理

Cooike 是存储在客户机的文本文件，它们保存了大量轨迹信息。

服务器通常有三个步骤来识别回头客：

1. 服务器脚本发送一系列 cooike 到浏览器。比如名字，年龄，ID 号码等。
2. 浏览器在本地存储这些信息，以备不时之需。
3. 当下一次浏览器发送任何请求至服务器时，它会同时将这些 cooike 信息发送给服务器，然后服务器使用这些信息来识别用户。

### Cooike 剖析

在 JSP 中，设置一个 cooike 需要发送如下信息头给客户端：

```
HTTP/1.1 200 OK
Date: Fri, 04 Feb 2015 21:03:38 GMT
Server: Apache/1.3.9 (UNIX) PHP/4.0b3
Set-Cookie: name=runoob; expires=Friday, 04-Feb-17 22:03:38 GMT; 
                 path=/; domain=runoob.com
Connection: close
Content-Type: text/html
```

如上所示，一个 Cooike 包含一个键值对，一个 GMT 时间（过期时间），一个路径，一个域名。键值对会被编码为 URL。

JSP 脚本通过 request 对象中的 getCooikes() 方法来访问这些 Cooike，这个方法会返回一个 Cooike 对象数组。Cooike 对象提供了很多方法来访问或修改 Cooike 对象的数据。

### 使用 JSP 设置 Cooike

1. 创建一个 cooike 对象

   调用 cooike 的构造函数，使用一个 cooike 名称和值作为参数，它们都是字符串。

   ```java
   Cooike cooike=new cooike("key","value");
   ```

   **名称和值中不能包含空格或者如下字符：**

   ```java
   [] () = , " / ? @ : ;
   ```

2. 设置有效期

   使用 setMaxAge() 函数设置 cooike 的有效期，以秒为单位。

   ```java
   cooike.setMaxAge(60*60*24);
   ```

3. 将 cooike 发送至 HTTP 响应头中

   调用 response.addCooike() 函数向 HTTP 响应头中添加 Cooike

   ```jsp
   response.addCooike(cooike);
   ```

### 使用 JSP 读取 Cooike

调用 request.getCooikes() 方法获得一个 javax.servlet.http.Cooike 对象的数组，然后遍历这个数组，使用 getName() 方法和 getValue() 方法来获取每一个 Cooike 的名称和值。

### 使用 JSP 删除 cooike

1. 获取一个已经存在的 cooike 然后存储在 Cooike 对象中
2. 将这个 Cooike 的有效值设置为 0
3. 将这个 Cooike 重新添加到响应头中

## JSP Session

JSP 利用 servlet 提供的 HttpSession 接口来识别一个用户，存储这个用户的所有访问信息。

## JSP 文件上传

JSP 可以与 HTML form 标签一起使用，来允许用户上传文件到服务器。

## JSP 日期处理

JSP 可以使用所有的 Java API。

JSP 可以使用 Java 的 Date 类处理时间。

## JSP 页面重定向

当需要将文档移动到一个新的位置时，就需要使用 JSP重定向。

最简单的重定向方法是使用 response 对象的 sendRedirect() 方法：

```java
public void response.sendRedirect(String location) throws IOException
```

这个方法将状态码和新的页面位置作为响应发回给浏览器。

使用 setStatus() 和 setHeader() 方法可以达到同样的效果。

```java
....
String site="http://www.aa.com";
response.setStatus(response.SC_MOVED_TEMPORARILY);
response.setHeader("Loaction",site);
....
```

## JSP 点击量统计

实现一个页面点击量计数器，可以利用 JSP 的隐式对象和相关方法来实现。

## JSP 自动刷新

JSP 提供了一种机制可以定时的自动刷新页面。

刷新一个页面最简单的方法就是使用 response 对象的 setIntHeader() 方法：

```Java
public void setIntHeader(String header,int headerValue);
```

这个方法通知浏览器在给定的时间后刷新，时间以秒为单位。

## JSP 标准标签库 JSTL

JSP 标准标签库是一个 JSP 标签集合，它封装了 JSP 应用的通用核心功能。

根据 JSTL 标签所提供的功能，可以将其分为 5 个类别：

1. 核心标签

   最常用的 JSTL 标签

2. 格式化标签

   格式化标签用来格式化并输出文本、日期、时间、数字。

3. SQL 标签

   JSTL SQL 标签库提供了与关系型数据库进行交互的标签。

4. XML 标签

   提供了创建和操作 XML 文档的标签。

5. JSTL 标签

   包含了一系列标准函数，大部分是通用的字符串处理函数。

使用任何库，必须在每个 JSP 文件中的头部包含 `<taglib>` 标签。

## JSP 连接数据库

JSP 使用 SQL 标签与数据库进行交互。

## JSP JavaBean

JavaBean 是特殊的 Java 类，使用 Java 语言书写，并且遵守 JavaBean API 规范。

JavaBean 有如下特性：

1. 提供一个默认的无参构造函数
2. 需要被序列化并且实现了 Serializable 接口
3. 可能有一系列可读写属性
4. 可能有一系列的 getter 或 setter 方法

### JavaBean 属性

一个 JavaBean 对象的属性应该是可以被访问的，这个属性可以是任意合法的 Java 数据类型，包括自定义的 Java 类。

JavaBean 对象的属性通过 get 和 set 方法来访问。

### 访问 JavaBean

`<jsp:useBean>` 标签可以在 JSP 中声明一个 JavaBean 然后使用。声明后，JavaBean 对象就成了脚本变量，可以通过脚本元素或其他自定义标签来访问。

## JSP 自定义标签

自定义标签是用户定义的 JSP 语言元素。

JSP 标签扩展可以允许开发人员创建新的标签并且可以直接插入到一个 JSP 页面中。

## JSP 表达式语言

JSP 表达式语言（EL）使得访问存储在 JavaBean 中的数据变得非常简单。JSP EL 既可以用来创建算数表达式，也可以用来创建逻辑表达式。

JSP EL 表达式内可以使用整型数、浮点数、字符串、常量 ture、false 和 null。

### 简单的语法

典型的，当开发者需要在 JSP 标签中指定一个属性时，只需要简单地使用字符串即可：

```JSP
<jsp:setProperty name="box" property="perimeter" value="100"/>
```

JSP EL 允许开发者指定一个表达式来表示属性值。一个简单的表达式语法如下：

```JSP
${expr}
```

其中，expr 指的是表达式。

在 JSP EL 中，通用的操作符是 `.` 和 `{}`。这两个操作符允许开发者通过内嵌的 JSP 对象访问各种各样的 JavaBean 属性。 

比如，上面的 `<jsp:setProperty>` 标签可以使用表达式语言改写为如下形式：

```JSP
<jsp:setProperty name="box" property="perimeter" value="${2*box.width+2*box.height}"/>
```

当 JSP 编译器在属性中见到 `${}` 格式后，它会产生代码来计算这个表达式，并且产生一个替代品来替代表达式的值。

也可以在标签的模板文本中使用表达式语言。比如，在 `<jsp:text>` 标签主题中使用表达式：

```JSP
<jsp:text>
Box Perimeter is: ${2*box.width+2*box.height}
</jsp:text>
```

可以使用 page 指令设置 isELIgnored 属性的值为 true 来忽略 EL 表达式：

```JSP
<%@ page isELIgnored ="true"%>
```

### EL 中的基础操作符

EL 表达式支持大部分 Java 所提供的算术和逻辑操作符。

### JSP EL 中的函数

JSP EL 允许开发者在表达式中使用函数。这些函数必须被定义在自定义标签库中。

函数使用语法如下：

```JSP
${ns:func(param1,param2,...)}
```

ns 指的是命名空间，func 指的是函数的名称，param1 指的是第一个参数，以此类推。

比如有 fn:length 函数在 JSTL 库中定义，则可以像下面这样来获取一个字符串的长度：

```JSP
${fn:length("Get my length")}
```

### JSP EL 隐含对象

可以在表达式中直接使用这些隐含对象，就像使用变量一样。

## JSP 异常处理

JSP 代码中通常有一下几类异常：

* 检查型异常

  检查型异常就是一个典型的用户错误或者一个程序员无法预见的错误。比如，一个文件将要被打开，但是无法找到这个文件，则一个异常被抛出。这些异常不能在编译期被简单的忽略。

* 运行时异常

  一个运行时异常可能已经被程序员避免，这种异常在编译期将会被忽略。

* 错误

  错误不是异常，它超出了用户或者程序员的控制范围。错误通常会在代码中被忽略。

JSP 提供了可选项来为每个 JSP 页面指定错误页面。无论何时页面抛出了异常，JSP 容器都会自动的调用错误页面。

## JSP 调试

JSP 和 Servlet 程序趋向于牵涉到大量客户端和服务器之间的交互，这很有可能会产生错误，并且很难重现出错的环境。

### 使用 System.out.println()

System.out.println() 可以很方便的标记一段代码是否被执行。

System 为 Java 核心对象，它可以在任何地方使用而不用引入额外的类。使用范围包括 Servlets、JSP、RMI、EJB’s、Beans、类和独立应用。

与在断点调试相比，用 System.out 进行输出不会对应用程序的运行流造成中大影响。

### 使用 JDB Logger

J2SE 日志框架可为任何运行在 JVM 中的类提供日志记录服务。因此，我们可以利用这个框架来记录任何信息。

使用 Log4J 框架将消息记录在不同的文件中，这些消息基于严重程度和重要性来进行分类。

### 调试工具

NetBeans 是树形结构，是开源的 Java 综合开发环境，支持开发独立的 Java 应用程序和网络应用程序，同时支持 JSP 调试。

### 使用 JDB Debugger

在 JSP 和 Servlet 中使用 jdb 命令来进行调试，就像调试普通的应用程序一样。

### 使用注释

程序中的注释在很多方面都对程序的调试起到一定的帮助作用。

### 客户端和服务器的头模块

直接查看 HTTP 请求和响应。

## JSP 国际化

* 国际化（i18n)

  表明一个页面根据访问者的语言或国家来呈现不同的翻译版本

* 本地化（l10n）

  向网站添加资源，以使它适应不同的地区和文化

* 区域

  通常是一个语言标志和国家标志通过下划线连接起来。比如 en_US 代表美国英语。

JSP 容器能够根据 request 的 locale 属性来提供正确的页面版本。

```JSP
java.util.Locale request.getLocale()
```

