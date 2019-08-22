---
title: JavaEE中servlet，filter，listener解析
categories: Java
date: 2017-07-24 21:59:43
---
web.xml 的加载顺序是：context-param -> listener -> filter -> servlet ，而同个类型之间的实际程序调用的时候的顺序是根据对应的 mapping 的顺序进行调用的。
# Filter #
它使用户可以改变一个 request和修改一个response. Filter 不是一个servlet,它不能产生一个response,它能够在一个request到达servlet之前预处理request,也可以在离开 servlet时处理response.换种说法,filter其实是一个”servlet chaining”(servlet 链).我们可以利用它来转换编码，权限验证等功能。
1. 在 servlet 之前调用；
2. 在 servlet 调用之前检查 request;
3. 根据需要修改 request 和 response;
4. 在 servlet 调用之后截获，继续修改相应数据;

在实现 filter 中 doFilter 方法时必须调用 chain.doFilter(req,resp) 表示调用下一个 filter 或者最终请求。

# Servlet #
Servlet是使用Java Servlet 应用程序设计接口（API）及相关类和方法的 Java 程序。除了 java Servlet API，Servlet 还可以使用用以扩展和添加到 API 的 Java 类软件包。Servlet 在启用 Java 的 Web 服务器上或应用服务器上运行并扩展了该服务器的能力。Java servlet对于Web服务器就好象Java applet对于Web浏览器。Servlet装入Web服务器并在Web服务器内执行，而applet装入Web浏览器并在Web浏览器内执行。Java Servlet API 定义了一个servlet 和Java使能的服务器之间的一个标准接口，这使得Servlets具有跨服务器平台的特性。

### 它的拦截规则 ###
当一个请求发送到servlet容器的时候，容器先会将请求的url减去当前应用上下文的路径作为servlet的映射url，比如我访问的是http://localhost/test/aaa.html，我的应用上下文是test，容器会将http://localhost/test去掉，剩下的/aaa.html部分拿来做servlet的映射匹配。这个映射匹配过程是有顺序的，而且当有一个servlet匹配成功以后，就不会去理会剩下的servlet了（filter不同，后文会提到）。其匹配规则和顺序如下：

1. 精确路径匹配。例子：比如servletA 的url-pattern为 /test，servletB的url-pattern为 /* ，这个时候，如果我访问的url为http://localhost/test ，这个时候容器就会先 进行精确路径匹配，发现/test正好被servletA精确匹配，那么就去调用servletA，也不会去理会其他的servlet了。
2. 最长路径匹配。例子：servletA的url-pattern为/test/*，而servletB的url-pattern为/test/a/*，此时访问http://localhost/test/a时，容器会选择路径最长的servlet来匹配，也就是这里的servletB。
3. 扩展匹配，如果url最后一段包含扩展，容器将会根据扩展选择合适的servlet。例子：servletA的url-pattern：*.action
4. 如果前面三条规则都没有找到一个servlet，容器会根据url选择对应的请求资源。如果应用定义了一个default servlet，则容器会将请求丢给default servlet

# Listener #
它是基于观察者模式设计的。目前 Servlet 中提供了 5 种两类事件的观察者接口，我们实现它们，然后在 web.xml 里配置好就可以使用监听器了，它们分别是：4 个 EventListeners 类型的，ServletContextAttributeListener、ServletRequestAttributeListener、ServletRequestListener、HttpSessionAttributeListener 和 2 个 LifecycleListeners 类型的，ServletContextListener、HttpSessionListener。作为 Servlet 的监听器，它可以监听客户端的请求，服务端的操作等。比如监听在线用户数量：当增加一个 session 时，就触发 sessionCreated 方法，将人数加 1。

