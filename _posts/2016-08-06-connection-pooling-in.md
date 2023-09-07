---
layout: post
title: "Connection Pooling in the Cloud"
date: 2016-08-06
comments: true
tags: [development, cloud, cloud foundry]
---

[Spring](https://spring.io) apps are usually deployed to Cloud Foundry with the [Java Buildpack](https://github.com/cloudfoundry/java-buildpack) (if no explicit buildpack is mentioned, it would be used as the default buildpack for Java apps). The Java buildpack uses its [Spring Auto-reconfiguration framework](https://github.com/cloudfoundry/java-buildpack/blob/master/docs/framework-spring_auto_reconfiguration.md) to automatically reconfigure an application to use configured cloud services. It also adds the "cloud" profile as an active Spring profile for the application. This framework has its copy of Spring cloud connectors, which, along with other things, [initialises](https://github.com/spring-cloud/spring-cloud-connectors/blob/master/spring-cloud-spring-service-connector/src/main/java/org/springframework/cloud/service/relational/DbcpLikePooledDataSourceCreator.java) a Pooled datasource with a size of 4 and maxWaitTime of 30,000. This is done to be compatible with cloud providers' limited free-tier services. This setting is usually sufficient for any test or demo projects. But for any serious application, it is best to customise this per application-needs.

<!--break-->

But if the connection pool is left on default settings, you might encounter one or more warning messages in the application log, as shown below:

~~~
org.apache.tomcat.jdbc.pool.ConnectionPool         WARNING maxIdle is larger than maxActive, setting maxIdle to: 4
~~~

Depending upon the application, database usage pattern/demand, this message may be followed by another error message:

~~~
org.apache.tomcat.jdbc.pool.PoolExhaustedException: [...] Timeout: Pool empty. Unable to fetch a connection in 30 seconds, none available[size:4; busy:4; idle:0; lastwait:30000]
~~~

This error message appears if you are deploying a Spring app as is with the local DataSource configuration to Cloud Foundry (but you must have a service connection defined and bound to the deployed app). In that case, Java Buildpack's Spring Auto-reconfiguration framework reconfigures the datasource with the cloud service connection properties and ignores all local datasource customisation. you need to use the [Spring Cloud Spring Service Connector](http://cloud.spring.io/spring-cloud-connectors/spring-cloud-spring-service-connector.html) to remedy this and get more control over datasource properties.

This warning/error message may still appear if you use Spring Cloud Service Connector but do not supply pool properties, so it is initialized using default properties. You can supply the properties either programmatically like [this](http://cloud.spring.io/spring-cloud-connectors/spring-cloud-spring-service-connector.html#_relational_database_db2_mysql_oracle_postgresql_sql_server):

```java
//Connect to the 'my-own-personal-sql' relational database service, supplying configuration
@Bean
public DataSource dataSource() {
    PoolConfig poolConfig = new PoolConfig(5, 30, 3000);
    DataSourceConfig dbConfig = new DataSourceConfig(poolConfig, null);
    return connectionFactory().dataSource("my-own-personal-sql", dbConfig);
}
```

Or, if you use Spring Boot, you can use the spring.datasource.* properties in the application.properties file to specify datasource properties.

Check the blog post "[Binding to Data Services with Spring Boot in Cloud Foundry](https://spring.io/blog/2015/04/27/binding-to-data-services-with-spring-boot-in-cloud-foundry)" by Spring engineer [Dave Syer](https://spring.io/team/dsyer), where he discussed detailed scenarios of various types of service binding options for a Spring Boot App.



