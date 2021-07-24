## 1、JDK8新特性
#### 接口的默认方法
通过使用default关键字向接口添加非抽象方法实现
```java
interface Formula{
    double calculate(int a);
    default double sqrt(int a){
        return Math.sqrt(a);
    }
}
```
实现Formula接口的类只需要实现抽象方法calculate就可以直接使用默认方法sqrt

#### Lambda表达式
在老版本的Java中排列字符串方法：
```java
List<String> name = Arrays.asList("peter", "anna","mike","xenia");
Collections.sort(names, new Comparator<String>(){
    @Override
    public int compare(String a, String b){
        return b.compareTo(a);
    }
})
```
在JDK8中如下：
```java
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```
#### 方法和构造函数引用
JDK8允许通过::关键字传递方法或者构造函数的引用
