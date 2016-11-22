---
layout: post
title: "Prototype-scoped Bean Instance in Spring"
date: 2016-09-06
comments: true
tags: [development, spring]
---

[Spring Framework](https://projects.spring.io/spring-framework/) supports quite a few different types of bean [scopes](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html#beans-factory-scopes). Singleton and Prototye scoped beans are the mostly used ones. It is common for Spring framework based applications to use singleton scoped beans, but sometime there may be a need to use a prototype scoped bean. In this post, I will show how to get instance of a prototype-scoped bean in Spring Framework.

<!--break-->

This can be done using by using [Method injection](https://spring.io/blog/2004/08/06/method-injection/). In annotation based configuration, there are two choices based upon the version of Spring framework is used. If using Spring version older than 4.1.x, then [javax.inject.Providerinterface](http://docs.oracle.com/javaee/6/api/javax/inject/Provider.html) need to be used to create a provider to get the instance of the prototype bean. Since Spring 4.1.x, there is a new annotation introduced [@lookup](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Lookup.html) which is basically annotation based alternative of method injection.Following is an example of creating a factory class for QueueTask class using both methods:

```java
@Component 
@Scope("prototype") 
public class QueueTask {	
    @Autowired    private AtomicInteger dataIdCounter;    @Autowired    private IntegrationTasksRepository integrationTasks;	
    @Autowired    private MongoOperations mongoOps;    @Autowired    private SquadRunItemsRepository squadRunItems;    @Autowired    private AmazonS3Service amazonS3Service;    @Override    public void run() { 
        // Do stuff
    }
}
	
//use this if you use Spring version >= 4.1.x @Componentpublic class IntegrationTaskFactory {    @Lookup //use of lookup annotation 
    public QueueTask getQueueTask(){        //spring will override this method        return null;    }    public QueueTask getQueueTaskWithParams(Map<String, String> params) {        // Get a new QueueTask object 
        QueueTask task = getQueueTask();		
        // Customize task based on params ...
        // Return the object        return task;    } 
}//use this if you use Spring version < 4.1.x @Componentpublic class IntegrationTaskFactory {    
    @Autowired    private Provider<QueueTask> myQueueTask;    //use of javax.inject.Provider interface 
    public QueueTask getQueueTaskWithParams(Map<String, String> params) {        // Get a new QueueTask object 
        QueueTask task = myQueueTask.get();
        // Customize task based on params ...
        // Return the object 
        return task;    }}
```
Method injection can be done in XML based configuration in a similar way as shown in the [documentation](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html#beans-factory-method-injection).

*Update*: The original motivation of writing this post is that while there is a section on Method injection in Spring documentation, the use of @lookup annotation is not shown. Since writing this post, I created a documentation [issue](https://jira.spring.io/browse/SPR-14765) in Spring Jira. And to my surprise, the issue is resolved and documentation is updated within 5 days by Spring Framework lead [Juergen Hoeller](https://spring.io/team/jhoeller). You can check the current document, showing the use of @lookup annotation, [here](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html#beans-factory-lookup-method-injection).