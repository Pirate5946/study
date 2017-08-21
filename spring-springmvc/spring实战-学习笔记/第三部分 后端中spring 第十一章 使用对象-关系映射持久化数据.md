本章内容：
- 使用Spring和Hibernate
- 借助上下文Session，编写不依赖于Spring的Repository
- 通过Spring使用JPA
借助Spring Data实现自动化的JPA Repository

与Spring对JDBC的支持那样，Spring对ORM
框架的支持提供了与这些框架的集成点以及一些附加的服务：
- 支持集成Spring声明式事务；
- 透明的异常处理；
- 线程安全的、轻量级的模板类；
- DAO支持类；
- 资源管理。
## 11.1　在Spring中集成Hibernate
### 11.1.1　声明Hibernate的Session工厂

## 11.2　Spring与Java持久化API
在Spring中使用JPA的第一步是要在Spring应用上下文中将实体管理器工厂（entity manager factory）按照bean的形式来进行配置。
### 11.2.1　配置实体管理器工厂




























