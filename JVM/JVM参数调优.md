-XX:+PrintGC  每次触发GC的时候打印相关日志

-XX:+UseSerialGC 串行回收

-XX:+PrintGCDetails 更详细的GC日志

-Xms  堆初始值

-Xmx 堆最大可用值

-Xmn 新生代堆最大可用值

-XX:SurvivorRatio 用来设置新生代中eden空间和from/to空间的比例。

含以-XX:SurvivorRatio=eden/from=den/to

总结：在实际工作中，我们可以直接将初始的堆大小与最大堆大小相等，这样的好处是可以减少程序运行时垃圾回收次数，从而提高效率。

