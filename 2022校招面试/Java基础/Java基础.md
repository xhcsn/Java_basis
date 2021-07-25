## 1、类型擦除以及常用的通配符
Java的泛型是伪泛型，在Java编译期间所有的泛型信息都会被擦除掉，也就是通常所说的类型擦除

常用的通配符为：T、E、K、V、？
- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个 java 类型
- K V (key value) 分别代表 java 键值中的 Key Value
- E (element) 代表 Element

## 2、为什么重写equals时必须重写hashCode方法
hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

## 3、Java中的几种基本数据类型是什么？对应的包装类型是什么？各自占用多少字节呢？

Java中有8种基本数据类型，分别为：byte（1）、short（2）、int（4）、long（8）、float（4）、double（8）、char（2）、boolean（1位）。

八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean 。

Byte,Short,Integer,Long 这 4 种包装类默认创建了数值 [-128，127] 的相应类型的缓存数据，Character 创建了数值在[0,127]范围的缓存数据

## 4、方法的重写

重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。

方法的重写要遵循“两同两小一大”：
- “两同”即方法名相同、形参列表相同；
- “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
- “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。

## 5、异常类层次结构图

在Java中所有异常都有一个共同的祖先——Throwable类。Throwable类有两个重要的子类Exception和Error。Exception能够被程序本身处理，Error只能尽量避免

Exception分为两类：受检查型异常和运行时异常。受检查型异常如果没有被try/catch捕获那么就无法通过编译，如IOException、ClassNotFoundException、SQLException等；运行时异常即使不进行处理也可以正常通过编译，如NullPointerException、NumberFormatException、ArrayIndexOutOfBoundsException、ClassCastException、ArithmeticException等

## 6、try-catch-finally
- try块：用于捕获异常。其后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块。
- catch块：用于处理try捕获到的异常。
- finally 块：无论是否捕获或处理异常，finally块里的语句都会被执行。当在try块或catch块中遇到return语句时，finally语句块将在方法返回之前被执行。

在以下三种情况中，finally块不会被执行：
- try或者finally块中使用了System.exit(int)退出程序
- 程序所在的线程死亡
- 关闭CPU

**注意：finally语句的返回值会覆盖了try语句块的返回值。**

## 7、Java中的IO流
主要分为四类：
- InputStream/Reader:所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer:所有输出流的基类，前者是字节输出流，后者是字符输出流。

## 8、final、static、this、super关键字总结
#### final关键字
- final修饰的类不能被继承，final类中的所有成员方法都会被隐式地指定为final方法
- final修饰的方法不能被重写
- final修饰的变量是常量，如果是基本数据类型的变量。则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能让其指向另一个对象。
#### static关键字
- 修饰成员变量和成员方法：被static修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享
- 静态代码块：静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次。
- 静态内部类：静态内部类与非静态内部类之间存在一个最大的区别: 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。没有这个引用就意味着：1. 它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非 static 成员变量和方法。
#### this关键字
this关键字用于引用类的当前实例
#### super关键字
super关键字用于从子类访问父类的变量和方法。

## 9、获取class对象的四种方式
- 知道具体类
>Class alunbarClass = TargetObject.class;
- 通过Class.forName()传入类的路径
>Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
- 通过对象实例instance.getClass()
>TargetObject o = new TargetObject();
>Class alunbarClass2 = o.getClass();
- 通过类加载器xxxClassLoader.loadClass()传入类路径
>Class clazz = ClassLoader.loadClass("cn.javaguide.TargetObject");

## 10、静态代理实现
静态代理中，我们对目标对象的每个方法的增强都是手动完成的。

其实现步骤如下：
- 定义一个接口及其实现
- 创建一个代理类同样实现这个接口
- 将目标对象注入进代理类，然后在代理类的对应方法调用目标类中的对应方法。这样就可以通过代理类屏蔽掉对目标对象的访问，并且可以在目标方法执行前后做一些自定义的代码。

例子如下所示：
#### 定义发送短信的接口
```java
public interface SmsService{
    String send(String message);
}
```
#### 实现发送短信的接口
```java
public SmsServiceImpl implements SmsService {
    public String send(String message){
        System.out.println("Send message: " + message);
        return message;
    }
}
```
#### 创建代理类并同样实现发送短信的接口
```java
public class SmsProxy implements SmsService{
    private final SmsService smsService;
    public SmsProxy(SmsService smsService){
        this.smsService = smsService;
    }
    @Override
    public String send(String message){
        System.out.println("before method send()");
        smsService.send(message);
        System.out.println("after method send()");
        return null;
    }
}
```
#### 实际使用
```java
public class Main{
    public static void main(String[] args) {
        SmsService smsService = new SmsServiceImpl();
        SmsProxy smsProxy = new SmsProxy(smsService);
        smsProxy.send("java");
    }
}
```
## 11、JDK动态代理
在Java动态代理机制中，InvocationHandler接口和Proxy类是核心

Proxy类中使用频率最高的方法是：newProxyInstance()，这个方法主要用来生成一个代理对象。
```java
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,InvocationHandler h) throws IllegalArgumentException{
    ......
}
```
一共有三个参数：
- loader：类加载器，用于加载代理对象
- interface：被代理类实现的一些接口
- h：实现了InvocationHandler接口的对象

要实现动态代理的话，必须实现InvocationHandler来自定义处理逻辑
```java
public interface InvocationHandler{
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable;
}
```
invoke()有以下三个参数：
- proxy :动态生成的代理类
- method : 与代理类对象调用的方法相对应
- args : 当前 method 方法的参数

通过Proxy类的newProxyInstance()创建的代理对象在调用方法的时候，实际会调用到实现InvocationHandler接口的类的invoke()方法。

JDK动态代理使用步骤如下：
- 定义一个接口及其实现类；
- 自定义InvocationHandler并重写invoke方法，在invoke方法中调用原生方法并自定义处理逻辑
- 通过Proxy.newProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h) 方法创建代理对象；

#### 定义发送短信的接口
```java
public interface SmsService{
    String send(String message);
}
```
#### 实现发送短信的接口
```java
public class SmsServiceImpl implements SmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
```
#### 定义一个JDK动态代理类
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
/**
 * @author shuang.kou
 * @createTime 2020年05月11日 11:23:00
 */
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;
    public DebugInvocationHandler(Object target) {
        this.target = target;
    }
    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        //调用方法之前，我们可以添加自己的操作
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return result;
    }
}
```
#### 获取代理对象的工厂类
```java
public class JdkProxyFactory {
    public static Object getProxy(Object target) {
        return Proxy.newProxyInstance(
                target.getClass().getClassLoader(), // 目标类的类加载
                target.getClass().getInterfaces(),  // 代理需要实现的接口，可指定多个
                new DebugInvocationHandler(target)   // 代理对象对应的自定义 InvocationHandler
        );
    }
}
```
#### 实际使用
```java
SmsService smsService = (SmsService) JdkProxyFactory.getProxy(new SmsServiceImpl());
smsService.send("java");
```
## 12、CGLIB动态代理
JDK动态代理只能代理实现了接口的类

在CGLIB动态代理机制中MethodInterceptor接口和Enhancer类是核心

实现步骤：
#### 添加依赖
```java
<dependency>
  <groupId>cglib</groupId>
  <artifactId>cglib</artifactId>
  <version>3.3.0</version>
</dependency>
```
#### 实现一个使用阿里云发送短信的类
```java
public class AliSmsService{
    public String send(String message){
        System.out.println("send message:" + message);
        return message;
    }
}
```
#### 自定义MethodInterceptor（方法拦截器）
```java
public class DebugMethodInterceptor implements MethodInterceptor{
    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable{
        System.out.println("before method " + method.getName());
        Object object = methodProxy.invokeSuper(o, args);
        //调用方法之后，我们同样可以添加自己的操作
        System.out.println("after method " + method.getName());
        return object;
    }
}
```
#### 获取代理类
```java
import net.sf.cglib.proxy.Enhancer;

public class CglibProxyFactory {

    public static Object getProxy(Class<?> clazz) {
        // 创建动态代理增强类
        Enhancer enhancer = new Enhancer();
        // 设置类加载器
        enhancer.setClassLoader(clazz.getClassLoader());
        // 设置被代理类
        enhancer.setSuperclass(clazz);
        // 设置方法拦截器
        enhancer.setCallback(new DebugMethodInterceptor());
        // 创建代理类
        return enhancer.create();
    }
}
```
#### 实际使用
```java
AliSmsService aliSmsService = (AliSmsService) CglibProxyFactory.getProxy(AliSmsService.class);
aliSmsService.send("java");
```

## 13、Java中常见的三种IO模型
#### BIO——同步阻塞IO模型
同步阻塞IO模型中，应用程序发起read调用后，会一直阻塞，直到在内核把数据拷贝到用户空间
#### NIO——同步非阻塞模型
只需要一个线程来轮询数据是否准备好，再经由该线程来通知对应的读取线程获得数据
#### AIO——异步模型
程序发起read调用后立即返回，等待内核准备好数据，并发送信号告诉用户进程IO操作执行完毕。



















