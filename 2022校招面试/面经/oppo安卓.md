#### 自我介绍

#### volatile
volatile关键字是与Java的内存模型有关的，CPU的运算速度很快，但是读取写入的数据比较慢，因此在多线程的情况下，会从主存中读取一个变量的副本放在本地内存中，volatile关键字强制线程在使用变量的时候要到主存中去读取。

并发编程三个概念，原子性、有序性和可见性

#### 引用
强引用、软引用、弱引用、虚引用：监控对象何时回收

#### http和https的区别

　　1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

　　2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

　　3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

　　4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

#### 归并排序

```java
public static int[] sort(int a, int low, int high){
    int mid = (low + high) / 2;
    if(low < high){
        sort(a, low, mid);
        sort(a, mid + 1, high);
        merge(a, low, mid, high);
    }
    return a;
}

public static void merge(int[] a, int low, int mid, int high){
    int[] temp = new int[high - low + 1];
    int i = low;
    int j = mid + 1;
    int k = 0;
    while(i <= mid && j <= high){
        if(a[i] < a[j]){
            temp[k++] = a[i++];
        }else{
            temp[k++] = a[j++];
        }
    }
    while(i <= mid){
        temp[k++] = a[i++];
    }
    while(j <= high){
        temp[k++] = a[j++];
    }
    for(int x = 0; x < temp.length; x++){
        a[x + low] = temp[x];
    }
}

```

#### List Set Map区别

#### LinkedHashMap如何保证顺序
通过双向链表

#### hashMap底层

#### 快速排序
```java
public class QuickSort{
    public static void quickSort(int[] arr, int low, int high){
        int i, j, temp, t;
        if(low > high){
            return;
        }
        i = low;
        j = high;
        temp = arr[low];
        while(i < j){
            while(temp <= arr[j] && i < j){
                j--;
            }
            while(temp >= arr[j] && i < j){
                i++;
            }
        }
    }
}
```