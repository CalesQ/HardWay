###  一、基本概念
#### Pod
1. k8s里的最小执行单元
2. 1个pod里可以运行多个容器（容器隔离了六个维度的东西：Mount、User、PID、UTS、Net、IPC）, 但它们共享了UTS、Net、IPC
3. 可以把Pod理解成豌豆荚，容器比作里面的豌豆
4. 一个Pod里面运行多个容器，也叫做边车（SideCar）模式