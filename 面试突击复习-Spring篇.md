## 谈谈你对AOP的理解

- Spring的AOP就是面向切面编程。
- 对于某个功能点，添加切面，可以实现功能的动态扩展
- *将程序中的交叉业务逻辑（比如安全，日志，事务等）封装成一个切面，然后注入到目标对象（具体业务逻辑中）。*
- *AOP可以对某个对象或者某些对象的功能进行增强，比如对象中的方法进行增强，可以在执行某个方法之前额外的做一些事情，或者在某个方法执行之后做一些事情（关闭资源等）。*

## 谈谈你对IOC的理解

- IOC就是控制反转
- *IOC容器实际上就是个map，里面存放各种对象（XML中配置的bean，通过注解标记的对象@Repository，@Service，@Controller，@Component等等），在项目启动的时候会读取配置文件里面的bean节点，根据全限定类名使用反射创建对象放到map里或者扫描注解通过反射创建对象放入到map中。*
- *后续在代码中需要用到这些对象时，再通过DI（依赖注入，@Autowired或者@Resource等注解）*
- *在没有引入IOC容器之前，对象A依赖于对象B，那么在A初始化或者运行到某个时间点的时候，需要自己主动创建B或者使用已经创建的对象B。无论是创建还是使用B，控制权都是在A手上。再引入IOC容器之后，A和B失去了直接联系，当A运行到需要B的时候，IOC容器会主动创建一个对象B注入到A需要的地方。*
- 将创建对象的过程交给Spring工厂进行创建，并且由Spring工厂去控制对象生成的类型，和对象的生命周期。

## 解释一下Spring支持的几种bean的作用域

- singleton，*默认，每个容器中只有一个bean的实例，单例模式由BeanFactory自身来维护。生命周期和SpringIOC容器是一致的（但是在第一次注入时才被创建）*
- *prototype，为每一个bean请求提供一个实例。在每次注入时都会创建一个新的对象。*
- *request，bean被定义为在每个HTTP请求中创建一个单例对象，也就是说每个请求中都会复用这一个单例对象*
- *session，与request类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效。*
- *application，bean被定义为在ServletContext的生命周期中复用一个单例对象*
- *websocket，bean被定义为在WebSocket的生命周期中复用一个单例对象*
- *global-session，全局作用域，global-session和Portlet应用相关。当应用部署在Portlet容器中工作时，它包含很多portlet。如果想要共用全局的存储变量的话，那么这全局变量需要存储在global-session中。全局作用域与Servlet中session作用域效果相同。*

## Spring事务传播机制

- *REQUIRED（Spring默认的事务传播类型）：如果当前没有事务，则自己新建一个事务；如果当前存在事务，则加入这个事务。无论父子方法哪个发生异常回滚，整个事务都会回滚。即使父方法捕捉了一场，也会发生回滚*
- *SUPPORTS：当前存在事务则加入当前事务；如果没有，就以非事务方法执行。*
- *MANDATORY：当前存在事务，则加入当前事务；如果当前事务不存在，则抛出异常。*
- *REQUIRES_NEW：创建一个新事物，如果存在当前事务，则挂起该事务。*
- *NOT_SUPPORTED：以非事务方式执行，如果当前存在事务，则挂起当前事务。*
- *NEVER：不使用事务，如果当前事务存在，则抛出异常。*
- *NESTED：如果当前事务存在，则嵌套事务中执行，否则REQUIRED的操作一样（开启一个事务）。*

## Spring事务什么时候会失效

- *1.发生自调用，类里面使用this调用本类的方法（this通常省略），此时这个this对象不是代理类，而是UserService对象本身。解决方法很简单，把this变成UserService的代理类即可。*
- *2.方法不是public的，@Transactional只能用于public的方法上，否则事务不会失效，如果要用在非public方法上，可以开启AspectJ代理模式。*
- *3.数据库不支持事务*
- *4.没有被Spring管理*
- *5.异常被吃掉，事务不会回滚（或者抛出的异常没有被定义，默认为RuntimeException）。*

## Spring中的bean创建的生命周期有哪些步骤？

- *推断构造方法*
- *实例化*
- *填充属性，也就是依赖注入*
- *处理Aware回调*
- *初始化前，处理@PostConstruct注解*
- *初始化，处理InitializingBean接口*
- *初始化后，进行AOP*

## Spring的Bean生命周期

- 实例化：为Bean分配空间
- 设置属性：将当前类以来的Bean属性，进行注入和装配
- 初始化：
  - 执行各种通知
  - 执行初始化的前置方法
  - 执行初始化方法
  - 执行初始化的后置方法
- 使用Bean：在程序中使用Bean对象
- 销毁Bean：将Bean对象进行销毁操作

## Spring中Bean是线程安全的吗？

- *Spring本身并没有针对Bean做线程安全的处理，所以如果Bean是无状态的（scope定义为singleton)，那么Bean则是线程安全的。如果Bean是有状态的，那么Bean不是线程安全的。*
- *另外，Bean具体是不是线程安全，跟Bean的作用域没关。Bean的作用域只是表示Bean的生命周期范围，对于任何生命周期的Bean都是一个对象，这个对象是不是线程安全的，还是得看Bean对象本身。*

## ApplicationContext和BeanFactory有什么区别？

- *BeanFactory是Spring中非常核心的组件，表示Bean工厂，可以生成Bean，维护Bean。*
- *而ApplicationContext继承了BeanFactory，所以ApplicationContext拥有BeanFactory所有的特点，也是一个Bean工厂。*
- *但是ApplicationContext除了继承BeanFactory之外，还继承了其他接口，从而ApplicationContext拥有获取系统环境变量、国际化、事件发布等功能，这些都是BeanFactory所不具备的。*

## Spring中事务是如何实现的？

- *1.Spring事务底层是基于数据库事务和AOP机制的*
- *2.首先对使用了`@Transactional`注解的bean，Spring会创建一个代理对象作为bean。*
- *3.当调用代理对象的方法时，先会判断该方法上是否加了`@Transactional`注解。*
- *4.如果加了，那么则利用事务管理器创建一个数据库连接。*
- *5.并且修改数据库连接的autocommit属性为false，禁止此链接的自动提交，这是实现Spring事务非常重要的一步。*
- *6.然后执行当前方法，方法中会执行SQL*
- *7.执行完当前方法后，如果没有出现异常就直接提交事务。*
- *8.如果出现了异常，并且这个异常是需要回滚的就回滚事务，否则仍然提交事务。*
- *9.Spring事务的隔离级别对应的就是数据库的隔离级别。*
- *10.Spring事务的传播机制是Spring事务自己实现的，也是Spring事务中最复杂的。*
- *11.Spring事务的传播机制是基于数据库来做的，一个数据库连接一个事务，如果传播机制配置为需要新开一个事务，那么实际上就是先建立一个数据库连接，在此新数据库连接上执行SQL。*

## Spring容器启动流程是怎么样的？

- 创建Spring容器时，首先进行扫描，扫描得到所有的BeanDefinition对象，并存在Map中。
- 从Map中筛选出非懒加载的单例BeanDefinition进行创建Bean，对于多例Bean不需要再启动过程中去进行创建，对于多例Bean会在每次获取Bean是利用BeanDefinition去创建。
- 利用BeanDefinition创建Bean就是Bean的创建生命周期，这期间包括了合并BeanDefinition、推断构造方法、实例化、属性填充、初始化前、初始化、初始化后等步骤，其中AOP就是发生在初始化后这一步骤中。
- 单例Bean创建完了之后，Spring会发布一个容器启动事件，此时Spring启动结束。
- Spring的扫描是通过BeanFactoryPostProcessor来实现的，依赖注入是通过BeanPostProcessor来实现的。
- Spring在启动过程中还会去处理@Import等注解。

## Spring用到了哪些设计模式？

- 工厂设计模式，BeanFactory
- 监听者模式，ApplicationListener（事件监听机制）
- 代理模式，AOP使用动态代理
- 装饰器模式，BeanWrapper（比单纯的Bean对象功能更强大）
- 适配器模式，ApplicationlistenermethodAdapter（将@EventListener注解的方法适配成ApplicationListener）
- 构造器模式，BeanDefinitionBuilder（BeanDefinition构造器）

## Spring MVC工作流程



## Spring MVC主要组件？



## 如何理解SpringBoot中的Starter，SpringBoot的自动装配机制

- *如果使用Spring+SpringMVC，此时需要引入mybatis等框架，需要到xml中定义mybatis需要的bean*
- *starter就是定义一个starter的jar包，写一个`@Configuration`配置类，将这些bean定义在里面，然后在starter包的`META-INF/spring.factories`中写入该配置类，springboot会按照约定来加载该配置类。
- 开发人员只需要将相应的starter包依赖进引用，进行相应的属性配置（使用默认配置时，不需要配置），就可以直接进行代码开发，使用对应的功能了，比如：`mybatis-spring-boot-starter`，`spring-boot-starter-redis`

## SpringBoot中常用注解及其底层实现

- @SpringBootApplication，这个注解标识了一个SpringBoot工程，实际上是另外三个注解的组合
  - @SpringBootConfiguration，这个注解实际就是一个@Configuration，表示启动类也是一个配置类
  - @EnableAutoConfiguration，向Spring容器中导入了一个selector，用来加载ClassPath下SpringFactories中所定义的自动配置类，将这些自动加载为配置bean。
  - @ComponentScan，标识扫描路径，因为默认是没有配置实际扫描路径，所以SpringBoot扫描的路径是启动类所在的当前目录。

- @Bean，用来定义Bean，类似于XML中的<bean>标签。在Spring启动时，会对加了@Bean注解的方法进行解析，将方法的名字作为beanName，并通过执行方法得到bean对象。
- @Controller,@Service,@Component
- @Autowired,@Repository

## SpringBoot是如何启动Tomcat的

- 首先，SpringBoot在启动时会先创建一个Spring容器
- 在创建Spring容器过程中，会利用@ConditionalOnClass技术来判断当前classpath中是否存在Tomcat依赖，如果存在则会生成一个启动Tomcat的Bean
- Spring容器创建完之后，就会获取启动Tomcat的bean，并创建Tomcat对象，绑定端口等，然后启动Tomcat

## SpringBoot中配置文件的加载顺序是怎么样的？





