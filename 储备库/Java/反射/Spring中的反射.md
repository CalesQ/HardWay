## IOC

通过使用反射机制与工厂模式，使框架更灵活。开发者通过 xml 或是注解配置自定义类的信息（即找到对应类的字节码的位置），使得bean容器来
统一创建和管理类/bean，然后将创建的类/bean作为值，beanid作为key放入的哈希表结构中保存下来。然后通过构造方法、setter、注解三种方式，来将实例注入的对应
的引用位置。