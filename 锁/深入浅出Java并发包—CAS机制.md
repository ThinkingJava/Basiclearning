# CAS基本概念

原子类底层实现保证线程安全，CAS无锁机制效率比有锁机制高。CAS无锁机制其实和乐观锁类似概念。

CAS体系中有三个参数，它包含三个参数CAS(V,E,N)：V表示要更新的变量，E表示预期值，N表示新值。仅当V值等于E值时，才会将V值的值设为N，如果V值和E值不同，则说明已经有其它线程做了更新，则当前线程什么也不做。最后，CAS返回当前V的真实值。

# CAS机制

在JDK1.5之前。Java主要靠synchronized这个关键字保证同步，已解决多线程下的线程不安全问题，但是这会导致锁的发生，会引发一些个性能问题。

锁主要存在一下问题

（1）在多线程竞争下，加锁、释放锁会导致比较多的上下文切换和调度延时，引起性能问题。

（2）一个线程持有锁会导致其它所有需要此锁的线程挂起。

（3）如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能风险。

Volatile是一个不错的选择，但是前面我们已经说了，volatile不能保证原子性，因此同步还是需要用到锁。

也许大家已经听说过，锁分两种，一个叫悲观锁，一种称之为乐观锁。Synchronized就是悲观锁的一种，也称之为独占锁，加了synchronized关键字的代码基本上就只能以单线程的形式去执行了，它会导致其他需要该资源的线程挂起，直到前面的线程执行完毕释放所资源。而另外一种乐观锁是一种更高效的机制，它的原理就是每次不加锁去执行某项操作，如果发生冲突则失败并重试，直到成功为止，其实本质上不算锁，所以很多地方也称之为自旋。

乐观锁用到的主要机制就是CAS。Compare And Swap。

CAS有三个操作数，内存数据v，旧的预期数据A，要修改的数据B。每次进行数据更新时，当且仅当预期值A和内存中的数据V相同时，才将内存中的数据修改为B，否则什么也不做。

使用这种机制编写的算法也叫非阻塞算法，标准定义为一个线程的失败或者挂起不影响其他线程的失败或者挂起的算法。

现在的CPU提供了特殊的指令来自动更新共享数据，而且能检测到其他数据的干扰，因此可以通过compareAndSet来提到锁定。前面我们提到的一些原子类其实就是用的这个原理，如AtomicInteger，我们来看一下它对应的源码。

```java
private volatile int value;
 
public final int incrementAndGet() {
        for (;;) {
            int current = get();
            int next = current + 1;
            if (compareAndSet(current, next))
                return next;
        }
} 
public final int getAndAdd(int delta) {
        for (;;) {
            int current = get();
            int next = current + delta;
            if (compareAndSet(current, next))
                return current;
        }
    }
```

这里很显然采取了CAS的机制，每次从内存中读取数据都需要和+1后的数据进行一次CAS操作，如果成功返回结果，否则就失败重试，直到重试成功为止！

但是方法compareAndSet却是利用JNI来完成CPU指令的操作。

我们来看一下对应的源码

```java
// setup to use Unsafe.compareAndSwapInt for updates
private static final Unsafe unsafe = Unsafe.getUnsafe();
 
public final boolean compareAndSet(int expect, int update) {
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
```

整个过程就是这样子的，利用CPU的CAS机制，同时借助JNI来完成Java的非阻塞算法。基本上Java中的原子类都是使用类似的机制来保证数据的原子操作的。

尽管CAS机制使得我们可以不依赖同步，不影响和挂起其他线程实现原子性操作，能大大提升运行时的性能，但是会导致一个ABA的问题。如线程一和线程二都取出了主存中的数据为A，这时候线程二将数据修改为B，然后又修改为A，此时线程一再次去进行compareAndSet的时候仍然能够匹配成功，而实际对的数据已经发生了变更了，只不过发生了2次变化将对应的值修改为原始的数据了，并不代表实际数据没有发生变化。这时  。



# CAS原子性

步骤 1.读旧值(即从系统内存中读取所要使用的变量的值，例如：读取变量i的值)

步骤2.求新值（即对从内存中读取的值进行操作，但是操作后不修改内存中变量的值，例如：i=i+1,这一步只进行i+1,没有赋值，不对内存中的i进行修改）

步骤3.两个不可分割的原子操作

第一步：比较内存中变量现在的值与 最开始读的旧值是否相同(即从内存中重新读取i的值，与一开始读取的i进行比较)
第二步：如果这两个值相同的话，则将求得的新值写入内存中（即：i=i+1,更改内存中的i的值）如果这两个值不相同的话，则重复步骤1开始
 注：这两个操作是不可分割的原子操作，必须两个同时完成

