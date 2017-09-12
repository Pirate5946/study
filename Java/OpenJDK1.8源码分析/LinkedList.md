[jdk1.8.0_45源码解读——LinkedList的实现](http://www.cnblogs.com/CherishFX/p/4734490.html)     

[Java集合类框架学习 3 —— LinkedList(JDK1.8/JDK1.7/JDK1.6)](http://blog.csdn.net/u011392897/article/details/57115818)

1. 　　LinkedList是List和Deque接口的双向链表的实现。实现了所有可选列表操作，并允许包括null值。

2. 可以被当作堆栈、队列或双端队列进行操作。并且其顺序访问非常高效，而随机访问效率比较低。
3. 此实现不是同步的,

4. 类中的iterator()方法和listIterator()方法返回的iterators迭代器是fail-fast的：   
当某一个线程A通过iterator去遍历某集合的过程中，若该集合的内容被其他线程所改变了；那么线程A访问集合时，就会抛出ConcurrentModificationException异常，产生fail-fast事件。
---
### 1. 节点 Node结构
### 2. LinkedList 类结构
//通过LinkedList实现的接口可知，其支持队列操作，双向列表操作，能被克隆，支持序列化    
```
public class LinkedList<E> extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```    

 **LinkedList**包含了三个重要的对象：first、last 和 size。

(1) first 是双向链表的表头，它是双向链表节点所对应的类Node的实例

(2) last 是双向链表的最后一个元素，它是双向链表节点所对应的类Node的实例

(3) size 是双向链表中节点的个数。

### 3. 构造函数
LinkedList提供了两种种方式的构造器，    
- 构造一个空列表、
- 构造一个包含指定collection的元素的列表，这些元素按照该collection的迭代器返回的顺序排列的。

