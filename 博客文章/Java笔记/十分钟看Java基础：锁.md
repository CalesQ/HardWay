说到锁，就不得不说 synchronized、volatile 和 lock 三兄弟了。


独占模式和共享模式，处于独占模式下时，其他线程试图获取该锁将无法取得成功。在共享模式下，多个线程获取某个锁可能（但不是一定）会获得成功

1. 独占模式下获取锁的方法为tryAcquire，此方法必须有子类实现，同一时刻只有一个线程可以获取同步状态，可以看到锁ReentrantLock就是采用的独占模式

2. 共享模式下获取锁的方法为tryAcquireShared，同一时刻可以有多个线程获取同步状态，此方法同样必须有子类实现，信号量Semaphore就是采用的共享模式，结合上面可以看见读写锁ReentrantReadWriteLock两者皆用到了