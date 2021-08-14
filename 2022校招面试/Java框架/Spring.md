#### 1、什么是Spring框架
Spring是一个开源的轻量级Java开发框架

Spring自带IoC和AOP，可以很方便地对数据库进行访问，可以很方便的集成第三方组件

#### 2、Spring中重要的模块
- Spring Core是核心模块，主要提供IoC依赖注入功能的支持
- Spring Aspects模块为与AspectJ的集成提供支持
- Spring AOP模块提供了面向切面的编程实现

#### 3、简述对Spring IoC的了解
IoC是一种设计思想，将原本在程序中手动创建对象的控制权交由Spring框架来管理。

将对象之间的相互依赖关系交给IoC容器来管理，并由IoC容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。IoC容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，而不用考虑对象是如何被创建出来的。

#### 4、谈谈自己对于AOP的理解
AOP指的是面向切面编程，可以将那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性

Spring AOP是基于动态代理的，如果要代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy去创建代理对象，而对于未实现接口的对象，Spirng AOP会使用Cglib生成一个被代理对象的子类作为代理

#### 5、Spring AOP和AspectJAOP有什么区别
Spring AOP属于运行时增强，而AspectJ是编译时增强。Spring AOP基于代理，而AspectJ基于字节码操作。

#### 6、什么是bean
bean代指那些被IoC容器所管理的对象。

我们需要告诉IoC容器帮助我们管理哪些对象，这个是通过配置元数据的定义的。配置元数据可以是XML文件、注解或者Java配置类。

#### 7、bean的作用域有哪些
- singleton：唯一bean实例，Spring中的bean默认都是单例的
- prototype：每次请求都会创建一个新的bean实例
- request：每次HTTP请求都会产生一个新的bean，该bean仅仅在当前的HTTP request内有效
- session：每次HTTP请求都会产生一个新的bean，该bean仅仅在当前的HTTP session内有效

#### 8、如何配置bean的作用域
xml方式：
```xml
<bean id="..." class="..." scope="singleton"></bean>
```
注解方式
```java
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```

#### 9、单例的线程安全
单例bean存在线程问题，主要是因为多个线程操作同一个对象的时候是存在资源竞争的。

常见的两种解决方法：
- 在bean中尽量避免定义可变的成员变量
- 在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在ThreadLocal中

#### 10、@Component和@Bean的区别
- @Component注解作用于类，@Bean注解作用于方法。
- @Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中，@Bean注解通常是我们在标有该注解的方法中定义产生这个bean，@Bean告诉了Spring这是某个类的实例
- @Bean注解比@Component注解的自定义性更强，很多地方只能通过@Bean注解来注册bean

#### 11、将一个类声明为bean的注解有哪些
我们一般使用@Autowired注解自动装配bean，要想把类标识为可用于@Autowired注解自动装配的bean的类，采用以下注解可以实现：
- @Component：通用的注解，可以标注任意类为Spring组件
- @Repository：对应持久层，即Dao层，主要用于数据库相关操作。
- @Service：对应服务层，主要涉及一些复杂的逻辑，需要用到Dao层。
- @Controller：对应SpringMVC控制层，主要用户接受用户请求并调用Service层返回数据给前端页面。

#### 12、bean的生命周期

#### 13、SpringMVC的工作原理
- 客户端（浏览器）发送请求，直接请求到DispatcherServlet。
- DispatcherServlet根据请求信息调用HandlerMapping，解析请求对应的Handler。
- 解析到对应的Handler（也就是我们平常说的 Controller 控制器）后，开始由HandlerAdapter适配器处理。
- HandlerAdapter会根据Handler来调用真正的处理器开处理请求，并处理相应的业务逻辑。
- 处理器处理完业务后，会返回一个ModelAndView对象，Model是返回的数据对象，View是个逻辑上的View。
- ViewResolver会根据逻辑View查找实际的View。
- DispaterServlet把返回的Model传给View（视图渲染）。
- 把View返回给请求者（浏览器）