[JDK源码分析](http://blog.csdn.net/u010887744/article/category/6126950)

[HashMap JDK1.8源码分析](http://blog.csdn.net/u010887744/article/details/50346257)

1. HashMap是基于哈希表的Map接口实现的
2. 存储的是<key，value>对的映射，==允许多个null值和一个null键==。
3. 不保证映射的顺序，特别是它不保证该顺序恒久不变。
4. 不同步，可以通过Collections.synchronizedMap 来进行“包装”；或者使用 ConcurrentHashMap 保证多线程安全
---
### HashMap的数据结构
HashMap实际上是一个“链表的数组”的数据结构，==数组的每个元素存放链表的头结点==，即==数组和链表的结合体==。        

HashMap底层就是一个数组结构，数组中的每一项又是一个链表     
#### Node<K,V>的结构
链表中的每个元素为一个包含key值，key的哈希码，value值，下一个Node节点，
- final int hash; //hash码
- final K key;
- V value;
- Node<K,V> next; //指向链表中下一个实例    

==该数组的下标索引为key的哈希码==     

---
### HashMap 类结构
HashMap包含了几个重要的成员变量：table, size, threshold, loadFactor。

(01) table是一个Node[]数组类型，而Node实际上就是一个单向链表。哈希表的"key-value键值对"都是存储在Node数组中的。    

(02) size是HashMap的大小，它是HashMap保存的键值对的数量。   

(03) threshold是HashMap的阈值，用于判断是否需要调整HashMap的容量。threshold的值="容量*加载因子"，当HashMap中存储数据的数量达到threshold时（size>=threshold），就需要将HashMap的容量加倍。     

(04) loadFactor就是加载因子。

---
### 构造方法
//构造一个带指定初始容量和加载因子的空HashMap       
    public HashMap(int initialCapacity, float loadFactor)
    
//构造一个带指定初始容量和默认加载因子(0.75)的空 HashMap    
    public HashMap(int initialCapacity)
    
//构造一个具有默认初始容量 (16)和默认加载因子 (0.75)的空 HashMap    
    public HashMap()     
    
//构造一个映射关系与指定 Map相同的新 HashMap,容量与指定Map容量相同，加载因子为默认的0.75     
    public HashMap(Map<? extends K, ? extends V> m)    
    
//找出“大于Capacity”的最小的2的幂,使Hash表的容量保持为2的次方倍    
static final int ==tableSizeFor==(int cap)

---
### resize()方法

在HashMap的四种构造函数中并没有对其成员变量Node<K,V>[]table进行任何初始化的工作          

该初始化的诱发条件是在向HashMap中添加第一对<key,value>时，  
通过put(K key, V value) -> putVal(hash(key), key, value, false, true) -> resize()方法。  

故HashMap中尤其重要的resize()方法主要实现了两个功能：

1. 在table数组为null时，对其进行初始化，默认容量为16；

2. 当tables数组非空，但需要调整HashMap的容量时，将hash表容量翻倍。

![image](http://on-img.com/chart_image/59b25577e4b04308eadcd192.png)