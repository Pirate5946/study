JDK分为几个大的包，对于构建分布式Java应用而言，最重要的有 集合、并发、网络（包括网络BIO 以及网络 NIO）以及 序列化/反序列化 

## 4.1 集合包
Java中最常用的包，常用的有 Collection和Map 两个接口的实现类

Collection用于存放多个 单对象，Map用于存放 Key-Value形式的键值对

Collection常用的分为两种类型的接口，List和Set，List支持放入重复的对象，Set不支持放入重复的对象

对于Collection的实现类，要掌握以下几点
- Collection的创建
构造器
- 往Collection中增加对象    
add(E)方法
- 删除Collection中的对象
remove(E)方法
- 获取Collection的单个对象
get(int index)
- 遍历Collection中的对象    
通过Collection的iterator方法获取迭代器，进而遍历
- 判断对象是否存在于 Collection中
contains(E)
- Collection 中对象的排序


