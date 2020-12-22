
## ReentractLock的公平与非公平实现

reentractLock 是通过AQS来实现的，内部通过继承AQS来实现代码业务加锁，AQS内部维护了一个CLH队列(重量级锁也是队列)，同时维护一个state标识位(无锁状态为0，加锁成功后为1，重入加锁则state继续++)，如果线程争用时发现new reentractLock的对象中的state >= 1 时，就会让线程封装成AQS内部的Node，进入CLH队列。当锁对象发生竞争时，AQS对象会初始化(非竞争状态下，不会初始化AQS对象)，AQS会初始化一个空的Node(CLH队列的head节点，里面的Thread为null)来标识当前持有锁对象的线程，当其他线程过来时，在head中追加一个Node且将线程LockSupport.park()，并且tail指向这个Nodel，每有一个线程争用，添加一个Node，tail移位。lock 比 synchronized 多的一个东西公平锁和非公平锁的设置，synchronized只能是非公平锁，还有一个是tryAcquire，尝试获取锁的功能