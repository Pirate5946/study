### ArrayList
[参考文章--- jdk1.8.0_45源码解读——ArrayList的实现](http://www.cnblogs.com/CherishFX/p/4725394.html)     
[参考文章2---详细文字注释](http://blog.csdn.net/gulu_gulu_jp/article/details/51456969)    
[参考文章3---图片分析](http://blog.csdn.net/u011392897/article/details/57105709)
```
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```        
实际上java.util.ArrayList是个固定大小的对象，自身并不直接存储元素内容，而是持有一个引用==element==指向一个数组==Object[]==，真正负责存储元素内容的正是这个数组    

定义了一个int值==size==表示数组的长度==element.length==    

当需要扩容时，扩容的是这个数组，而不是ArrayList对象自身。(https://www.zhihu.com/question/48872729)

- 线程不安全 modCount记录修改次数

> 构造函数初始化

有三种构造器，原理是new一个object[]赋值给默认属性 elementData
```
public ArrayList(int initialCapacity) //指定容量的 object[],不能小于0

public ArrayList() //默认初始化为 {}，空的数组

public ArrayList(Collection<? extends E> c) // 
```

> 常用方法  

### 增加  
- public void add(int index, E element)     
往指定索引位置新增元素，确保索引没有越过上界和下界    
检查长度+1后有没有超过数组  
然后通过==system.arraycopy()方法==将位置为==index==的(==size-index==)个字符复制到到index+1的位置（相当于将原来的元素整体后移一位 ）

- public boolean addAll(int index, Collection<? extends E> c)   
原理跟 public void add(int index, E element)差不多，    
检查增加Collection后，数组是否需要扩容      
然后通过==system.arraycopy()方法==将原来从element[index]开始的size-index个元素复制到element[index+c.length]

==往arraylist指向的数组中添加元素时会判断元素个数，小于10则初始化数组长度为10==
- public boolean add(E e)
检查元素个数+1后是否超出数组容量，超出则将数组容量增加一半（原容量右移一位）
最后elementData[size++] = e

- public boolean addAll(Collection<? extends E> c)      
原理跟public boolean add(E e)差不多     
获取c的长度，检查size+a.length的长度是否超过数组容量一半（原容量右移一位）==如果新增一半还不够，就新增size+c.length，不能超过int.MAX_VALUE==  
最后将新增的元素复制到数组的末尾，size+=c.length


### 删除
- public E remove(int index)    
//判断index是否 <= size         
//将数组elementData中index位置之后的所有元素向前移一位      
//将原数组最后一个位置置为null，由GC清理

- public boolean remove(Object o)   
//移除ArrayList中首次出现的指定元素(如果存在)，ArrayList中允许存放重复的元素    
// 由于ArrayList中允许存放null，因此下面通过两种情况来分别处理。    
如果要删除的元素是null，null没有equals方法，只能判断值    
//私有的移除方法，跳过index参数的边界检查以及不返回任何值


- protected void removeRange(int fromIndex, int toIndex)        
//删除ArrayList中从fromIndex（包含）到toIndex（不包含）之间所有的元素，共移除了toIndex-fromIndex个元素

- public boolean removeAll(Collection<?> c)     
//删除ArrayList中包含在指定容器c中的所有元素        
读写双指针，遍历整个数组，判断数组中元素与传入的集合中的元素是否有重合，把不包含在原集合的元素保留下来，==重合的元素删除==

- public boolean retainAll(Collection<?> c)     
//移除ArrayList中不包含在指定容器c中的所有元素，与removeAll(Collection<?> c)正好相反  
把传入集合与原集合相同的元素保留下来，==不相同的元素删除==

- public void clear()   
//清空ArrayList，将全部的元素设为null
### 获取
- public E get(int index)   

### 修改
- public E set(int index, E element)        
检查索引是否越界        
将旧索引值返回，旧索引指向新传入的元素

### 查找元素
- public boolean contains(Object o)       
//遍历ArrayList，判断元素在集合中的索引值是否大于0
- public int indexOf(Object o)      
//正向查找，返回ArrayList中元素Object o第一次出现的位置，如果元素不存在，则返回-1

- public int lastIndexOf(Object o)          
//逆向查找，返回ArrayList中元素Object o最后一次出现的位置，如果元素不存在，则返回-1   
- public E get(int index)       
//返回指定索引处的元素

### 其他public方法  
- public void trimToSize()  
//将底层数组的容量调整为当前列表保存的实际元素的大小的功能

- public int size()     
//返回ArrayList的大小（元素个数）

- public boolean isEmpty()      
//判断ArrayList是否为空

- public Object clone()       
//返回此 ArrayList实例的浅拷贝（modcount = 0）

- public Object[] toArray()       
//返回一个包含ArrayList中所有元素的数组

- public <T> T[] toArray(T[] a)       
//如果给定的参数数组长度足够，则将ArrayList中所有元素按序存放于参数数组中，并返回      
//如果给定的参数数组长度小于ArrayList的长度，则返回一个新分配的、长度等于ArrayList长度的、包含ArrayList中所有元素的新数组
   
### 序列化 
- 写入函数writeObject(java.io.ObjectOutputStream s)

- 读取函数readObject(java.io.ObjectInputStream s)

### ArrayList的四种遍历方式 
- 泛型在编译时会被擦除（变为Object），通过迭代器遍历获取时向下转型为源类型（泛型补偿）














