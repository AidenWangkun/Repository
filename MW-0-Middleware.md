# 问题记录



# 待归档区

```
在Java开发中，常见的中间件包括但不限于以下几种:

1. Web容器:例如Tomcat、Jetty.Undertow等，用于运行Web应用程序的容器，支持Servlet和JSP规范;

2.应用服务器:例如JBoss、WebSphere、WebLogic等，功能更加强大，支持分布式应用、EJB等;

3.数据库中间件:例如MySQL Proxy、TDDL、Cobar等，用于实现数据库的读写分离、负载均衡、故障转移等;

4.缓存中间件:例如Redis、 Memcached、Ehcache等，用于提高数据访问速度和性能;

5.消息中间件:例如RabbitMQ、ActiveMQ、 Kafka等，用于实现异步消息传递、解耦和削峰;

6.日志中间件:例如ELK、Log4j、Slf4j等，用于记录应用程序的日志，并支持日志分析和查询;

7.搜索引擎:例如Solr、Elasticsearch 等，用于实现全文搜索和数据分析;

8. RPC框架:例如 Dubbo、gRPC、Thrift等，用于实现远程过程调用，简化服务调用的过程。

这些中间件能够有效地提高Java应用程序的性能、可靠性、可扩展性等方面的表现，因此在Java开发中被广泛地应用。
```





# Redis实现

# 简单动态字符串

Redis设计构建了一种名为简单动态字符的抽象类型（SDS：Simple Dynamic String），并将SDS作为Redis的默认字符串表示。

> C字符串只会作为字符串字面量（string literal）用在无需对字符串值进行修改的地方，比如打印日志。

当Redis需要一个可以被修改的字符串值时，Redis就会使用SDS来表示字符串值。

