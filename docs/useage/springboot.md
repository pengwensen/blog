### SpringBoot使用文档

#### 日志配置
> logback-spring.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false" scan="false" scanPeriod="60 seconds">

    <springProperty scope="context" name="LOG_HOME" source="logging.file.path"/>
    <springProperty scope="context" name="APP_NAME" source="spring.application.name"/>

    <!-- 定义控制台输出 -->
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} - [%thread] - %-5level - %logger{50} - %msg%n</pattern>
        </layout>
    </appender>

    <appender name="appLogAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 指定日志文件的名称 -->
        <file>${LOG_HOME}/${APP_NAME}.log</file>

        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_HOME}/${APP_NAME}-%d{yyyy-MM-dd}-%i.log</fileNamePattern>
            <MaxHistory>30</MaxHistory>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>10MB</MaxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>

        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [ %thread ] - [ %-5level ] [ %logger{50} : %line ] - %msg%n</pattern>
        </layout>
    </appender>

    <!-- 日志输出级别,优先级从高到低 ERROR、WARN、INFO、DEBUG -->
    <root level="INFO">
        <appender-ref ref="stdout"/>
        <appender-ref ref="appLogAppender"/>
    </root>
</configuration>
```

> application配置
```properties
logging.level.com.xiaoniu=debug
logging.config=classpath:logback-spring.xml
logging.file.path=D:/logs
```