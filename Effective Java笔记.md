[单例模式中为什么用枚举更好](http://www.importnew.com/6461.html)
### 实现单例的方式
- double checked locking 实现法：   
//
- 静态工厂实现法（）    
//被final 修饰的静态字段（成员变量）会在类加载的第一个阶段（加载阶段）执行    
//为了实现单例类的可序列化，除了加上 implements Serializable ,还需要声明实例是 transient，而且提供一个 readResolve方法
- 枚举      
//实现单例的最佳方式

### 对于所有对象都通用的方法
- 覆盖 toString方法，返回对象中值得关注的信息

- 谨慎的覆盖 clone 方法