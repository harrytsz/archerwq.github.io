---
layout: post
title: "Java Logging: slf4j + logback"
date: 2015-10-09
banner_image: 
tags: [Java]
---

Java日志方案有很多，包括：java.util.logging、Apache的commons-logging和log4j、slf4j以及logback. 一个大型项目会用到众多第三方jar包，这些jar包可能会用到上述各种日志方案，如何在新的项目中使用slf4j+logback的组合，让所有其他jar包的日志也输出到logback，并避免冲突和异常？

<!--more-->

> SLF4J is a simple facade for logging systems allowing the end-user to plug-in the desired logging system at deployment time.  

> Logback is intended as a successor to the popular log4j project. It was designed by Ceki Gülcü, log4j’s founder. It builds upon a decade of experience gained in designing industrial-strength logging systems. The resulting product, i.e. logback, is faster and has a smaller footprint than all existing logging systems, sometimes by a wide margin. Just as importantly, logback offers unique and rather useful features missing in other logging systems.  

下面这张图能够很好地说明slf4j和各个日志框架集成的原理：
{% include image_caption.html imageurl="/images/posts/slf4j_integration.png" caption="sl4j integration" %}

slf4j-api.jar里的org.slf4j.LoggerFactory会调org.slf4j.impl.StaticLoggerBinder，这个StaticLoggerBinder在logback-classic.jar里实现，这样就把slf4j的日志绑定到logback了。这时，如果classpath里同时还有slf4j-log4j12.jar那么会报multiple SLF4j bindings错误，因为slf4j-log4j12.jar里也有StaticLoggerBinder实现。

```
SLF4J: Class path contains multiple SLF4J bindings. 
SLF4J: Found binding in [jar:file:/D:/mvn_repository/ch/qos/logback/logback-classic/1.1.2/logback-classic-1.1.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]  
SLF4J: Found binding in [jar:file:/D:/mvn_repository/org/slf4j/slf4j-log4j12/1.7.6/slf4j-log4j12-1.7.6.jar!/org/slf4j/impl/StaticLoggerBinder.class]  
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.  
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder] 
```

应用里只能把slf4j绑定到一个最终日志实现方案，可以是logback,log4j,comming-logging或java.util.logging等，本篇主要介绍slf4j绑定logback, 上面这个图就是绑定到logback.

如果应用引用其他的jar是使用log4j,comming-logging或java.util.logging记日志的，要把这些日志也路由到logback上来，首先把这些日志调用路由到slf4j，slf4j再到logback.具体实现是用jcl-over-slf4j.jar替换commons-logging,用log4j-over-slf4j.jar替换log4j.jar, 用jul-to-slf4j.jar替换java.util.logging. 原理也很简单，log4j-over-slf4j.jar里有log4j.jar同名的类，所以有log4j-over-slf4j.jar了就要把log4j.jar删了。但jul-to-slf4j.jar的原理比较特殊一些，因为java.util.logging是jdk自带的，没有办法替换，想了解具体怎么做的可以看下实现代码代码。

maven pom.xml配置:

```xml
<dependency>  
    <groupId>org.slf4j</groupId>  
    <artifactId>slf4j-api</artifactId>  
    <version>1.7.6</version>  
</dependency>  
<dependency>  
    <groupId>ch.qos.logback</groupId>  
    <artifactId>logback-classic</artifactId>  
    <version>1.1.2</version>  
</dependency>  
<dependency>  
    <groupId>ch.qos.logback</groupId>  
    <artifactId>logback-core</artifactId>  
    <version>1.1.2</version>  
</dependency>  
<dependency>  
    <groupId>org.slf4j</groupId>  
    <artifactId>log4j-over-slf4j</artifactId>  
    <version>1.7.6</version>  
</dependency>  
<dependency>  
    <groupId>org.slf4j</groupId>  
    <artifactId>jcl-over-slf4j</artifactId>  
    <version>1.7.6</version>  
</dependency>  
<dependency>  
    <groupId>org.slf4j</groupId>  
    <artifactId>jul-to-slf4j</artifactId>  
    <version>1.7.6</version>  
</dependency>  
```

logback的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>  
  
<configuration>  
    <!-- Send debug messages to System.out -->  
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">  
        <!-- By default, encoders are assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder -->  
        <encoder>  
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{5} - %msg%n</pattern>  
        </encoder>  
    </appender>  
  
    <!-- Send debug messages to a file -->  
    <appender name="FILE"  
        class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <file>D:/logs/java_toolbox_lbk.log</file>  
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">  
            <Pattern>%d{yyyy-MM-dd_HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</Pattern>  
        </encoder>  
        <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">  
            <FileNamePattern>D:/logs/java_toolbox_lbk.%i.log.zip</FileNamePattern>  
            <MinIndex>1</MinIndex>  
            <MaxIndex>10</MaxIndex>  
        </rollingPolicy>  
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">  
            <MaxFileSize>2MB</MaxFileSize>  
        </triggeringPolicy>  
    </appender>  
  
    <logger name="com.weibo.keeplooking.logging" level="INFO" additivity="false">  
        <appender-ref ref="STDOUT" />  
        <appender-ref ref="FILE" />  
    </logger>  
  
    <!-- By default, the level of the root level is set to DEBUG -->  
    <root level="DEBUG">  
        <appender-ref ref="STDOUT" />  
    </root>  
  
</configuration>  
```

测试代码：

```java
import org.junit.Test;  
import org.slf4j.bridge.SLF4JBridgeHandler;  
  
public class VariousLogging {  
  
    private static final java.util.logging.Logger jucLogger = java.util.logging.Logger.getLogger(VariousLogging.class  
            .getName());  
    private static final org.apache.log4j.Logger log4jLogger = org.apache.log4j.Logger.getLogger(VariousLogging.class);  
    private static final org.apache.commons.logging.Log commonLogger = org.apache.commons.logging.LogFactory  
            .getLog(VariousLogging.class);  
    private static final org.slf4j.Logger slf4jLogger = org.slf4j.LoggerFactory.getLogger(VariousLogging.class);  
  
    static {  
        // optionally remove existing handlers attached to j.u.l root logger  
        SLF4JBridgeHandler.removeHandlersForRootLogger(); // (since SLF4J 1.6.5)  
  
        // add SLF4JBridgeHandler to j.u.l's root logger, should be done once during  
        // the initialization phase of your application  
        SLF4JBridgeHandler.install();  
    }  
  
    private void logWithJuc() {  
        jucLogger.info("java util logging...");  
    }  
  
    private void logWithLog4j() {  
        log4jLogger.info("log4j...");  
    }  
  
    private void logWithCommonLogging() {  
        commonLogger.info("common logging...");  
    }  
  
    private void logWithSlf4j() {  
        slf4jLogger.info("slf4j...");  
    }  
  
    @Test  
    public void testLogging() {  
        logWithJuc();  
        logWithLog4j();  
        logWithCommonLogging();  
        logWithSlf4j();  
    }  
  
}  
```

输出结果：

> 23:16:49.378 [main] INFO  c.w.k.l.VariousLogging - java util logging...  
> 23:16:49.382 [main] INFO  c.w.k.l.VariousLogging - log4j...  
> 23:16:49.382 [main] INFO  c.w.k.l.VariousLogging - common logging...  
> 23:16:49.382 [main] INFO  c.w.k.l.VariousLogging - slf4j...  
