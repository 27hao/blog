---
title: Java ��־
categories: Java
date: 2018-06-14 20:09:43
---

#### һ��Log4

Log4j��Apache��һ����Դ��Ŀ�������������������������־��Ϣ.����Ҫ��Ϊ�����֣�һ��appender���������־�ķ�ʽ������logger���Ǿ�����־�����

**1.Appender**

�������У�Log4j�ṩ��appender�����¼��֣�
����

* org.apache.log4j.ConsoleAppender�����������̨��
* org.apache.log4j.FileAppender��������ļ���
* org.apache.log4j.DailyRollingFileAppender��ÿ�����һ����־�ļ���
* org.apache.log4j.RollingFileAppender���ļ�����ָ����С��ʱ�����һ���µ��ļ���
* org.apache.log4j.WriterAppender������־��Ϣ������ʽ���͵�����ָ���ĵط���

���磺

```
<appender name=" ConsoleAppenderA"class="org.apache.log4j.ConsoleAppender">
        <param name="target" value="System.out"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern"value="%-5p %x %c{2} -%m%n"/>
        </layout>
</appender>

<appender name="BUSINESSERRORAPPENDER" class="com.alibaba.common.logging.spi.log4j.DailyRollingFileAppender">
        <param name="file" value="${luna_loggingRoot}/usr/common/business-error.log"/>
        <param name="append" value="true"/>
        <param name="encoding" value="GBK"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%d %-5p %c{2} - %m%n"/>
        </layout>
    </appender>
    
     <appender name="cacheFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <File>${LOG_HOME}/redis-cache.log</File>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/redis-cache-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>100MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <encoder>
            <Pattern>%date{ISO8601} %-5level [%thread] %logger{32} - %message%n</Pattern>
        </encoder>
    </appender>
```

**���У�Log4j�ṩ��layout�����¼��֣�**

* org.apache.log4j.HTMLLayout����HTML�����ʽ���֣�
* org.apache.log4j.PatternLayout����������ָ������ģʽ��
* org.apache.log4j.SimpleLayout��������־��Ϣ�ļ������Ϣ�ַ�����
* org.apache.log4j.TTCCLayout��������־������ʱ�䡢�̡߳����ȵ���Ϣ��



**��ʽ����־��ϢLog4J��������C�����е�printf�����Ĵ�ӡ��ʽ��ʽ����־��Ϣ����ӡ�������£�**

* %m ���������ָ������Ϣ
* %p ������ȼ�����DEBUG��INFO��WARN��ERROR��FATAL
* %r �����Ӧ�������������log��Ϣ�ķѵĺ�����
* %c �����������Ŀ��ͨ�������������ȫ��
* %t �����������־�¼����߳���
* %n ���һ���س����з���Windowsƽ̨Ϊ��rn����Unixƽ̨Ϊ��n��
* %d �����־ʱ�������ڻ�ʱ�䣬Ĭ�ϸ�ʽΪISO8601��Ҳ���������ָ����ʽ�����磺%d{yyyy MMM ddHH:mm:ss,SSS}��������ƣ�2002��10��18�� 22��10��28��921
* %l �����־�¼��ķ���λ�ã�������Ŀ�����������̣߳��Լ��ڴ����е�������

**2.Logger**

Log4j����һ������־��

```
<root>
   <level value="$luna_loggingLevel"/>
   <appender-ref ref="PROJECT"/>
   <appender-ref ref="EXCEPTION_LOG"/>
</root>
```


Ĭ��ʱϵͳ���������зǸ�logger����̳и���־����logger�����level���ԾͻḲ�Ǽ̳���־�����������־��)��level���ԡ���appender����ӣ�����logger�����õ�appender���ϼ̳���־���ϵ�appender��

```
<logger name="com.alibaba.service.VelocityService">
        <level value="INFO"/>
        <appender-ref ref="ConsoleAppenderA"/>
</logger>
```

���ݼ̳�ԭ��õ����logger��level��INFO��appender��ConsoleAppenderA��PROJECT��EXCEPTION_LOG��
Ҳ����ʹlogger���̳�����logger��ʹ��additivity="false"

```
<logger name="com.alibaba.china.pylon.enhancedmit.domain.Handler" additivity="false">
	    <level value="info"/>
	    <appender-ref ref="enhancedMITAppender"/>
</logger>
```

���У�level ����־��¼�����ȼ�����ΪOFF��FATAL��ERROR��WARN��INFO��DEBUG��ALL�����Զ���ļ���

Log4j���õ��ĸ��������ȼ��Ӹߵ��ͷֱ���ERROR��WARN��INFO��DEBUG��ͨ�������ﶨ��ļ��������Կ��Ƶ�Ӧ�ó�������Ӧ�������־��Ϣ�Ŀ��ء�**���������ﶨ����INFO����ֻ�е��ڼ������������ĲŻᴦ����DEBUG�������־��Ϣ�����ᱻ��ӡ������**

ALL����ӡ���е���־��OFF���ر����е���־����� 
appender-ref���Լ�ʹ��logger����֮ǰ���ù���appender��


**�ڳ�������ô����logger���������أ�**

```
Logger logger =Logger.getLogger(VelocityService.class.getName());��
��
Logger logger =LogFactory.getLog(VelocityService.class);
logger.debug("Justtesting a log message with priority set to DEBUG");
logger.info("Just testinga log message with priority set to INFO");
logger.warn("Just testinga log message with priority set to WARN");
logger.error("Justtesting a log message with priority set to ERROR");
logger.fatal("Justtesting a log message with priority set to FATAL");
```
 
���⣬logger��name��ǰ׺Ĭ��Ҳ�м̳��ԣ�����

```
<logger name="com.alibaba.service" additivity="false">
     <level value="INFO"/>
     <appender-ref ref="ConsoleAppenderA"/>
</logger>

<logger name="com.alibaba.service.VelocityService">
    <level value="INFO"/>
    <appender-ref ref="FileAppenderA"/>
 </logger>
 ````
 
���ݼ̳�ԭ����Ϊcom.alibaba.service.VelocityService��logger��appender��FileAppenderA��ConsoleAppenderA��level��ΪINFO��


#### ����SLF4J

Log4J ��Simple Logging Facade for Java��ʹ���ձ飬���ǿ�ܽ��أ������˺ܶ����õİ������SLF4J�����ܶࡣSLF4J�ܺõؽ�����API��ʵ�֣����磬�����ǿ��ʹ��SLF4J��API���������������������˼���ľɵ�Log4J.properties�ļ���

���磺

```
Logger.debug("Hello " + name);
```

�����ַ���ƴ�ӵ����⣨ע������������ƴ���ַ������ٸ��ݵ�ǰ�����Ƿ����debug�����Ƿ����������־����ʹ�������־���ַ���ƴ�Ӳ���Ҳ��ִ�У�����๫˾ǿ��ʹ���������䣬����ֻ�е�ǰ����DEBUG����ʱ�Ż�ִ���ַ���ƴ�ӣ�

```
if (logger.isDebugEnabled()) {
    LOGGER.debug(��Hello �� + name);
}
```

���������ַ���ƴ�����⣬���е�̫�������ǲ��ǣ���Եأ�SLF4J�ṩ���������򵥵��﷨:

```
LOGGER.debug("Hello {}", name);
```

������ʽ���Ƶ�һ��ʾ��������û���ַ���ƴ�����⣬Ҳ����ڶ�������������

**pom������**

```
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.7</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>log4j-over-slf4j</artifactId>
            <version>1.7.7</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.1.2</version>
        </dependency>
```

**�ο����ϣ�**


http://ifeve.com/java-slf4j-think/

https://www.slf4j.org/