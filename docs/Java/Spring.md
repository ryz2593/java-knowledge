<h2>Spring Ioc</h2>

（1）Spring Ioc就是控制发转，是指创建对象的控制权的转移，以前创建对象的主动权和时机由我们自己把控的，而现在这种权利转移到spring容器中，并由容器根据配置文件去创建实例和管理各实例之间的依赖关系，对象和对象之间松散耦合，也利于功能的复用。DI依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖Ioc容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，Ioc让对象的创建不用去new了，可以由spring自动生产，使用java的反射机制，根据配置文件在运行时动态的去创建和管理对象，并调用兑现的方法。

（3）Spring的Ioc的三种注入方式：构造器注入、setter方法注入、注解注入

<h3>AOP</h3>

AOP是面向切面的编程，其思想是把散布于不同业务但功能相同的代码从业务逻辑中抽取出来，封装成独立的模块，这些独立的模块被称为切面，切面的具体功能方法被称为关注点。在业务逻辑执行过程中，AOP会把分离出来的切面和关注点动态切入到业务流程中，这样做的好处是提高了代码的重用性和可维护性。

Spring框架提供了@AspectJ注解方法和基于XML架构的方法来实现AOP.

<h3>Bean的生命周期</h3>

> 1. bean对象的实例化---也就是我们常说的new

> 2. 按照spring上下文对实例化的bean进行配置---也就是IOC注入

> 3. 如果这个bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，此处传递的就是spring配置文件中的bean的id

> 4. 如果这个bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory(beanFactory)传递的是spring工厂自身（可以用这个方式来获取其他bean，只需在spring配置文件中配置一个普通的Bean就可以）

> 5. 如果这个bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入spring上下文(同样这个方式也可以实现步骤4的内容，但比4更好，因为ApplicationContext是BeanFactory的子接口，有更多的实现方法)

> 6. 如果这个bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法，BeanProcessor经常被用作Bean内容的更改，并且由于这个是在Bean初始化结束时调用的那个方法，也可以被应用于内存或缓存技术

> 7. 如果Bean在spring配置文件中配置了init-method属性会自动调用其配置的初始化方法

> 8、如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法、；

>> 注：以上工作完成以后就可以应用这个Bean了，那这个Bean是一个Singleton的，所以一般情况下我们调用同一个id的Bean会是在内容地址相同的实例，当然在Spring配置文件中也可以配置非Singleton，这里我们不做赘述。

> 9、当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用那个其实现的destroy()方法；

> 10、最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。

<h3>Spring支持的几种Bean的作用域</h3>

（1）Singleton:默认，每个容器中只有一个Bean的实例，单例模式由BeanFactory自身来维护

（2）prototype：为每个bean请求提供一个实例

（3）request:为每个网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收

（4）session：与request范围类似，确保每个会话中有一个bean的实例，在session过期后，bean会随之失效

（5）global-session:全局作用域


<h3>Spring如何处理线程并发问题</h3>

在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域，因为Spring在对一些Bean中非线程安全状态采用ThreadLocal进行处理，解决线程安全问题。

ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。同步机制采用了“时间换空间”的方式，仅提供一份变量，不同的线程在访问前需要获取锁，没获得锁的线程则需要排队。而ThreadLocal采用了“空间换时间”的方式。

ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。应为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

<h3>Spring框架中都用到了那些设计模式</h3>

（1）工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；

（2）单例模式：Bean默认为单例模式。

（3）代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；

（4）模板模式：用来解决代码重复的问题，比如 RedisTemplate，JmsTemplate， JpaTemplate。

（5）观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener。

<h3>Spring事务的实现方式和实现原理</h3>

Spring事务的本质其实就是数据库对事务的支持，没有数据库的事务支持，spring是无法提供事务功能的。真正的数据库层的失误提交和回滚是通过binlog或者redo log实现的。

（1）Spring事务的种类：

spring支持编程式事务管理和声明式事务管理两种方式：

①编程式事务管理使用TransactionTemplate。

②声明式事务管理建立在AOP之上的。其本质是通过AOP功能，对方法前后进行拦截，将事务处理的功能编织到拦截的方法中，也就是在目标方法开始之前加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务。

>> 声明式事务最大的优点就是不需要在业务代码中掺杂事务管理的代码，只需在配置文件中做相关的事务规则声明或者通过@Transactional注解的方式，便可将事务规则应用到业务逻辑中。

>> 声明式事务管理要优于编程式事务管理，这正是spring倡导的非侵入式的开发方式，使业务代码不受污染，只要加上注解就可以获得完全的事务支持。但是最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。

（2）Spring的事务传播行为：

spring事务的传播行为说的是，当多个事务同时存在的时候，spring如何处理这些事务的行为。

① PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务，该设置是最常用的设置。

② PROPAGATION_SUPPORTS：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行。‘

③ PROPAGATION_MANDATORY：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常。

④ PROPAGATION_REQUIRES_NEW：创建新事务，无论当前存不存在事务，都创建新事务。

⑤ PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。

⑥ PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。

⑦ PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行。

（3）Spring中的隔离级别：

① ISOLATION_DEFAULT：这是个 PlatfromTransactionManager 默认的隔离级别，使用数据库默认的事务隔离级别。

② ISOLATION_READ_UNCOMMITTED：读未提交，允许另外一个事务可以看到这个事务未提交的数据。

③ ISOLATION_READ_COMMITTED：读已提交，保证一个事务修改的数据提交后才能被另一事务读取，而且能看到该事务对已有记录的更新。

④ ISOLATION_REPEATABLE_READ：可重复读，保证一个事务修改的数据提交后才能被另一事务读取，但是不能看到该事务对已有记录的更新。

⑤ ISOLATION_SERIALIZABLE：一个事务在执行的过程中完全看不到其他事务对数据库所做的更新。
