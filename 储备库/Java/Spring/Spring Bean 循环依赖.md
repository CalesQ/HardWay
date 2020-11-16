 ## Spring对于循环依赖的解决
 
 Spring循环依赖的理论依据其实是Java基于引用传递，当我们获取到对象的引用时，对象的field或者或属性是可以延后设置的。
 Spring单例对象的初始化其实可以分为三步：
 
 createBeanInstance， 实例化，实际上就是调用对应的构造方法构造对象，此时只是调用了构造方法，spring xml中指定的property并没有进行populate
 
 populateBean，填充属性，这步对spring xml中指定的property进行populate
 
 initializeBean，调用spring xml中指定的init方法，或者AfterPropertiesSet方法
 
 会发生循环依赖的步骤集中在第一步和第二步。
 
 ## 三级缓存

```java
/** Cache of singleton objects: bean name --> bean instance */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);
/** Cache of singleton factories: bean name --> ObjectFactory */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);
/** Cache of early singleton objects: bean name --> bean instance */
private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);
```

从字面意思来说：singletonObjects指单例对象的cache，singletonFactories指单例对象工厂的cache，earlySingletonObjects
指提前曝光的单例对象的cache。以上三个cache构成了三级缓存，Spring就用这三级缓存巧妙的解决了循环依赖问题。

## 解决方案

```java
addSingletonFactory(beanName, new ObjectFactory<Object>() {
@Override   public Object getObject() throws BeansException {
  return getEarlyBeanReference(beanName, mbd, bean);
}});
```

此处就是解决循环依赖的关键，这段代码发生在createBeanInstance之后，也就是说单例对象此时已经被创建出来的。
这个对象已经被生产出来了，虽然还不完美（还没有进行初始化的第二步和第三步），但是已经能被人认出来了（根据对
象引用能定位到堆中的对象），所以Spring此时将这个对象提前曝光出来让大家认识，让大家使用。

这样做有什么好处呢？让我们来分析一下“A的某个field或者setter依赖了B的实例对象，同时B的某个field或者setter
依赖了A的实例对象”这种循环依赖的情况。A首先完成了初始化的第一步，并且将自己提前曝光到singletonFactories中，
此时进行初始化的第二步，发现自己依赖对象B，此时就尝试去get(B)，发现B还没有被create，所以走create流程，
B在初始化第一步的时候发现自己依赖了对象A，于是尝试get(A)，尝试一级缓存singletonObjects(肯定没有，
因为A还没初始化完全)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，
由于A通过ObjectFactory将自己提前曝光了，所以B能够通过ObjectFactory.getObject拿到A对象(虽然A还没有
初始化完全，但是总比没有好呀)，B拿到A对象后顺利完成了初始化阶段1、2、3，完全初始化之后将自己放入到一级缓
存singletonObjects中。此时返回A中，A此时能拿到B的对象顺利完成自己的初始化阶段2、3，最终A也完成了初始化
，长大成人，进去了一级缓存singletonObjects中，而且更加幸运的是，由于B拿到了A的对象引用，所以B现在hold
住的A对象也蜕变完美了。

## 总结
Spring通过三级缓存加上“提前曝光”机制，配合Java的对象引用原理，比较完美地解决了某些情况下的循环依赖问题！

第三级缓存（ObjectFactory）中，Java 通过类文件获取 bean 的构造器，通过只运行构造器来构造一个具有“空壳”的对象，此时并不初始化其具有的属性，也就是上述
说到的提前曝光对象。**因此，构造器中的依赖不能通过此方法来解决**。

然后将在第三级缓存中创建出来的“空壳”对象以键值对的方式添加到第二级缓存（earlySingletonObjects）中。当有其他的对象初始化依赖到此对象时，就会通过第一级、第二级、第三级的顺序去寻找
所依赖的对象。通过这种方式，依赖一方的对象最终会完成所有的初始化（构造器->属性填充->spring 初始化init）就会被加入到一级缓存（SingletonObjects）中。最终完成依赖初始化。