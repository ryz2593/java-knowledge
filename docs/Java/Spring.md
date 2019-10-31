###Spring Ioc
（1）Spring Ioc就是控制发转，是指创建对象的控制权的转移，以前创建对象的主动权和时机由我们自己把控的，而现在这种权利转移到spring容器中，并由容器根据配置文件去创建实例和管理各实例之间的依赖关系，对象和对象之间松散耦合，也利于功能的复用。DI依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖Ioc容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，Ioc让对象的创建不用去new了，可以由spring自动生产，使用java的反射机制，根据配置文件在运行时动态的去创建和管理对象，并调用兑现的方法。

（3）Spring的Ioc的三种注入方式：构造器注入、setter方法注入、注解注入

###AOP
AOP是面向切面的编程，其思想是把散布于不同业务但功能相同的代码从业务逻辑中抽取出来，封装成独立的模块，这些独立的模块被称为切面，切面的具体功能方法被称为关注点。在业务逻辑执行过程中，AOP会把分离出来的切面和关注点动态切入到业务流程中，这样做的好处是提高了代码的重用性和可维护性。

Spring框架提供了@AspectJ注解方法和基于XML架构的方法来实现AOP.

###Bean的生命周期

1. bean对象的实例化---也就是我们常说的new

2. 按照spring上下文对实例化的bean进行配置---也就是IOC注入

3. 如果这个bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，此处传递的就是spring配置文件中的bean的id

4. 如果这个bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory(beanFactory)传递的是spring工厂自身（可以用这个方式来获取其他bean，只需在spring配置文件中配置一个普通的Bean就可以）

5. 如果这个bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入spring上下文(同样这个方式也可以实现步骤4的内容，但比4更好，因为ApplicationContext是BeanFactory的子接口，有更多的实现方法)

6. 如果这个bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法，BeanProcessor经常被用作Bean内容的更改，并且由于这个是在Bean初始化结束时调用的那个方法，也可以被应用于内存或缓存技术

7. 如果Bean在spring配置文件中配置了init-method属性会自动调用其配置的初始化方法

8、如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法、；

注：以上工作完成以后就可以应用这个Bean了，那这个Bean是一个Singleton的，所以一般情况下我们调用同一个id的Bean会是在内容地址相同的实例，当然在Spring配置文件中也可以配置非Singleton，这里我们不做赘述。

9、当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用那个其实现的destroy()方法；

10、最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。

