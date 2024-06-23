## 谈谈你对AOP的理解

- Spring的AOP就是面向切面编程。
- 对于某个功能点，添加切面，可以实现功能的动态扩展
- *将程序中的交叉业务逻辑（比如安全，日志，事务等）封装成一个切面，然后注入到目标对象（具体业务逻辑中）。*
- *AOP可以对某个对象或者某些对象的功能进行增强，比如对象中的方法进行增强，可以在执行某个方法之前额外的做一些事情，或者在某个方法执行之后做一些事情（关闭资源等）。*

## 谈谈你对IOC的理解

- IOC就是控制反转
- 将创建对象的过程交给Spring工厂进行创建，并且由Spring工厂去控制对象生成的类型，和对象的生命周期。
- 程序员可以更加专注到其他功能模块的开发中，而不需要分神到对象创建中。

## 解释一下Spring支持的几种bean的作用域

- singleton，*默认，每个容器中只有一个bean的实例，单例模式由BeanFactory自身来维护。生命周期和SpringIOC容器是一致的（但是在第一次注入时才被创建）*
- *prototype*
- *request*
- *session*
- *application*
- *websocket*
- *global-session*

## Spring事务传输机制



## Spring事务什么时候会失效



## Spring中的bean创建的声明周期有哪些步骤？



## Spring中Bean是线程安全的吗？



## ApplicationContext和BeanFactory有什么区别？



## Spring中事务是如何实现的？



## Spring中什么时候@Transactional会失效？



## Spring容器启动流程是怎么样的？



## Spring用到了哪些设计模式？

- 工厂设计模式，Factory
- 

## Spring MVC工作流程



## Spring MVC主要组件？



## 如何理解SpringBoot中的Starter



## SpringBoot中常用注解及其底层实现

- @SpringBootApplication
- @Bean
- @Controller,@Service,@Component
- @Autowired,@Repository

## SpringBoot是如何启动Tomcat的



## SpringBoot中配置文件的加载顺序是怎么样的？

- 首先加载根目录文件的application.yml
- 然后是根目录文件的application.xml
