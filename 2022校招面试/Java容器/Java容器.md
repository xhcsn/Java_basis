# Java容器

## 1、Java集合概述
Java中除了以Map结尾的类以外，其他的类都实现了Collection接口；以Map结尾的类都实现了Map接口。

## 2、List、Set和Map三者的区别
- List中存储的元素是可重复的、有序的；
- Set中存储的元素是不可重复的、无序的（有序，但指的是加入的顺序有序——即使用foreach的时候是具有加入的顺序的）
- Map存储的是键值对，即Key-Value

## 3、集合框架底层数据结构总结
### 3.1、List接口
- ArrayList：Object[]数组，线程不安全
- Vector：Object[]数组，线程安全
- LinkedList：JDK1.6以及之前是双向循环链表，JDK1.7取消了循环
### 3.2、Set接口
- HashSet：基于HashMap实现的，底层采用HashMap来保存元素，基本上所有的方法都是调用HashMap方法实现的
- LinkedHashSet：基于LinkedHashMap实现的
- TreeSet：红黑树，自平衡的排序二叉树
### 3.3、 Map接口
- HashMap：JDK1.8之前HashMap是由数组和链表组成的，数组是HashMap的主体，链表则是为了解决哈希冲突而存在的。JDK1.8以后在解决哈希冲突时有了较大的变化。当链表长度大于阈值（即8），并且此时数组长度大于64时，会将链表转换为红黑树，以减少搜索时间。
- LinkedHashMap：LinkedHashMap继承自HashMap，在HashMap的基础上增加了一条双向链表以保持键值对的插入顺序。
- HashTable：数组+链表，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的。
- TreeMap：红黑树（自平衡的排序二叉树）

## 4、ArrayList和Vector的异同点
- ArrayList和Vector的底层都是使用Object[]进行存储，适用于频繁的查找工作，线程不安全；
- Vector是List的古老实现类，底层使用Object[]存储，线程安全的。

## 5、ArrayList和LinkedList的异同点
- ArrayList和LinkedList都是不同步的，都不保证线程安全。
- ArrayList的底层采用的是Object数组，LinkedList的底层采用的是双向链表（JDK1.6以及之前采用的是双向循环链表，JDK1.7取消了循环）。
- ArrayList支持随机访问，LinkedList不支持随机访问
- 内存空间占用：ArrayList的空间浪费在于底层object数组需要预留一定的空间供未来的数据加入；LinkedList的空间浪费在于每一个元素不仅要存储元素值，还要存储前驱节点和后继节点

## 6、RandomAccess接口
RandomAccess接口其实什么都没有定义，只是一个标识，标识实现这个接口的类具有随机访问功能，其实这个类本身就具有随机访问功能。

## 7、ArrayList的扩容机制
用户如果不指定初始容量，ArrayList的默认初始容量大小为10，以无参数构造方法创建ArrayList的时候，实际上初始化赋值的是一个空数组，当真正对数组进行添加元素操作时，才真正分配容量，类似单例的懒汉式。

每次加入数据的时候会检验数组是否还有余量，如果没有余量就会进行扩容，每次扩容为原来的1.5倍向下取整。

## 8、comparable和Comparator的区别
- comparable接口出自java.lang包，有一个compareTo(Object obj)方法用来排序
- Comparator接口实际上是出自java.util包，它有一个compare(Object obj1, Object obj2)方法用来排序

## 9、HashMap和HashTable的异同点
- HashMap是非线程安全的，HashMap是线程安全的，HashTable内部的方法基本都经过synchronized修饰
- HaspMap的效率高于HashTable
- HashMap中可以存储null的key和value，但是null作为key只能有一个，HashTable中出现null键或者null值的话会抛出NullPointerException
- HashTable的初始大小为11， 每次扩容都变为原来的2n+1。HashMap的默认初始化大小为16，每次扩容会变为原来的2倍
- HashMap在JDK1.8以后在解决哈希冲突时有了较大的变化，链表大于8，数组长度大于64会将链表转化为红黑树，HashTable没有这样的机制。

## 10、HashSet如何检查重复
当把对象加入到HashSet的时候，HashSet会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入的对象的hashcode值作比较，如果没有相符的hashcode，HashSet会假设对象未出现，但是如果发现有相同的hashcode，这时候会调用equals方法来检查hashcode相等的对象是否真的相同，如果两者相同。HashSet就不会让加入操作成功。

## 11、HashMap的长度为什么是2的幂次方
hash值的范围大概有40亿多，需要对hashcode码进行取模运算，得到的余数为数组的下标。

取余中如果除数是2的幂次方则等价于与其除数减一的与操作。可以提高效率。

## 12、HashMap多线程操作导致死循环问题
其实是因为头插法是一直插在链表头部，而rehash遍历的顺序又是顺序进行导致的死循环问题

## 13、ConcurrentHashMap和Hashtable的区别和联系
- JDK1.7的ConcurrentHashMap底层采用分段的数组+链表实现，JDK1.8采用的数据结构为数组+链表/红黑树
- 在JDK1.7的时候，ConcurrentHashMap（分段锁）对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。到了JDK1.8的时候已经摒弃了Segment的概念，而是直接用Node数组+链表+红黑树的数据结构来实现，并发控制使用synchronized和CAS来操作。（JDK1.6 以后 对synchronized锁做了很多优化）整个看起来就像是优化过且线程安全的HashMap，虽然在 JDK1.8中还能看到Segment的数据结构，但是已经简化了属性，只是为了兼容旧版本；Hashtable(同一把锁) :使用synchronized来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用put添加元素，另一个线程不能使用put添加元素，也不能使用get，竞争会越来越激烈效率越低。

## 14、ConcurrentHashMap线程安全的具体实现方式/底层具体实现
JDK1.7：首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。
JDK1.8：synchronized 只锁定当前链表或红黑二叉树的首节点，粒度更小