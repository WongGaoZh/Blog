## CAS原理分析及ABA问题详解

### CAS
CAS即Compare And Swap的缩写，翻译成中文就是比较并交换，其作用是让CPU比较内存中某个值是否和预期的值相同，如果相同则将这个值更新为新值，不相同则不做更新，也就是CAS是原子性的操作(读和写两者同时具有原子性)，其实现方式是通过借助C/C++调用CPU指令完成的，所以效率很高。
CAS的原理很简单，这里使用一段Java代码来描述

```java
public boolean compareAndSwap(int value, int expect, int update) {
//        如果内存中的值value和期望值expect一样 则将值更新为新值update
    if (value == expect) {
        value = update;
        return true;
    } else {
        return false;
    }
}
```

### Unsafe源码分析
Java是在Unsafe(sun.misc.Unsafe)类实现CAS的操作，而我们知道Java是无法直接访问操作系统底层的API的(原因是Java的跨平台性限制了Java不能和操作系统耦合)，所以Java并没有在Unsafe类直接实现CAS的操作，而是通过JDI(Java Native Interface)本地调用C/C++语言来实现CAS操作的。
Unsafe有很多个CAS操作的相关方法，这里举例几个

```java
public final native boolean compareAndSwapObject(Object var1, long var2, Object var4, Object var5);

public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);

public final native boolean compareAndSwapLong(Object var1, long var2, long var4, long var6);


```
我们拿public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);进行分析，这个方法是比较内存中的一个值(整型)和我们的期望值(var4)是否一样，如果一样则将内存中的这个值更新为var5，参数中的var1是值所在的对象，var2是值在对象(var1)中的内存偏移量，参数var1和参数var2是为了定位出值所在内存的地址。

>Unsafe.java在这里发挥的作用有：
1. 将对象引用、值在对象中的偏移量、期望的值和欲更新的新值传递给Unsafe.cpp
2. 如果值更新成功则返回true给开发者，没有更新则返回false

>Unsafe.cpp在这里发挥的作用有：
1. 接受从Unsafe传递过来的对象引用、偏移量、期望的值和欲更新的新值，根据对象引用和偏移量计算出值的地址，然后将值的地址、期望的值、欲更新的新值传递给CPU
2. 如果值更新成功则返回true给Unsafe.java，没有更新则返回false
>

>CPU在这里发挥的作用：
>
>1. 接受从Unsafe.cpp传递过来的地址、期望的值和欲更新的新值，执行指令cmpxchg，比较地址中的值是否和期望的值一样，一样则将值更新为新的值，不一样则不做任何操作
>2. 将操作结果返回给Unsafe.cpp
>


### CAS的缺点

#### ABA问题

#### 循环时间长开销大

如果CAS操作失败，就需要循环进行CAS操作(循环同时将期望值更新为最新的)，如果长时间都不成功的话，那么会造成CPU极大的开销。

解决方法：
限制自旋次数，防止进入死循环。

#### 只能保证一个共享变量的原子操作

CAS的原子操作只能针对一个共享变量。

解决方法：
如果需要对多个共享变量进行操作，可以使用加锁方式(悲观锁)保证原子性，或者可以把多个共享变量合并成一个共享变量进行CAS操作。

### CAS应用

我们知道CAS操作并不会锁住共享变量，也就是一种非阻塞的同步机制，CAS就是乐观锁的实现。

乐观锁

乐观锁总是假设最好的情况，每次去操作数据都认为不会被别的线程修改数据，所以在每次操作数据的时候都不会给数据加锁，即在线程对数据进行操作的时候，别的线程不会阻塞仍然可以对数据进行操作，只有在需要更新数据的时候才会去判断数据是否被别的线程修改过，如果数据被修改过则会拒绝操作并且返回错误信息给用户。

悲观锁

悲观锁总是假设最坏的情况，每次去操作数据时候都认为会被的线程修改数据，所以在每次操作数据的时候都会给数据加锁，让别的线程无法操作这个数据，别的线程会一直阻塞直到获取到这个数据的锁。这样的话就会影响效率，比如当有个线程发生一个很耗时的操作的时候，别的线程只是想获取这个数据的值而已都要等待很久。
