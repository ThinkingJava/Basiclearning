# **1.static的作用**

static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。

static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

static代码块,static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

# **2.final关键字的作用**

1、被final修饰的类不可以被继承

2、被final修饰的方法不可以被重写

3、被final修饰的变量不可以被改变   (**引用不可变，引用指向的内容可变**)

“引用”是Java中非常重要的一个概念，对于引用的理解不深，很容易犯一些自己都没有意识到的错误。被final修饰的变量，不管变量是在是哪种变量，切记**不可变的是变量的引用而非引用指向对象的内容**。另外，本文中关于final的作用还有两点没有讲到：

1、被final修饰的方法，JVM会尝试为之寻求内联，这对于提升Java的效率是非常重要的。因此，假如能确定方法不会被继承，那么尽量将方法定义为final的。

2、被final修饰的常量，在编译阶段会存入调用类的常量池中。

# ***3.接口与抽象类的区别***

（1）抽象类可以有构造方法，接口中不能有构造方法。

（2）抽象类中可以有普通成员变量，接口中没有普通成员变量

（3）抽象类中可以包含静态方法，接口中不能包含静态方法

（4） 一个类可以实现多个接口，但只能继承一个抽象类。

（5）接口可以被多重实现，抽象类只能被单一继承

（6）如果抽象类实现接口，则可以把接口中方法映射到抽象类中作为抽象方法而不必实现，而在抽象类的子类中实现接口中方法

抽象类：

  1、包含一个或多个抽象方法的类本身必须被声明成抽象的。

  2、除了抽象方法之外，抽象类还可以包含具体数据和具体方法

  3、扩展抽象类的两种选择（抽象方法的具体实现在子类中）：

​       A、 抽象类中定义部分抽象类或不定义抽象类方法，这样就必须将子类也标记为抽象类。

​       B、定义全部的抽象方法，这样子类就不是抽象的了

   4、不能直接被实例化，可以间接使用

   5、一个类如果继承一个抽象类，必须实现该抽象类里声明的抽象方法

接口：

1、一个类可以实现（implement）一个或多个接口，并在需要接口的地方，随时使用实现了相应接口的对象

2、接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一格式进行定义

3、接口中的所有方法自动地属于public，因此，声明方法时，不必提供关键字public，不过，实现接口时，必须把方法声明成public,否则，编译器将认为这个方法的访问属性是包可见性，即类的默认访问属性。

4、接口中不能含有实例域或静态方法，但却可以包含常量

5、接口不能被实例化

6、接口中定义的方法都为抽象方法

注：当多个接口中有同名、同参、返回值类型不同的方法时，会产生命名冲突（因为接口支持多继承，防止出现二义性）

7、接口之间允许多继承（java只在接口允许多继承）

8、Java 8用默认方法与静态方法这两个新概念来扩展接口的声明

# **4.反射机制**

JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法;对于任意一个对象，都能够调用它的任意方法和属性;这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

![avatar](data:image/jpeg;base64,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)

1.反射用于动态类加载

![avatar](E:20180703205137276.jpg)

2.方法反射实例

```java
          public class ReflectCase {

            public static void main(String[] args) throws Exception {
                Proxy target = new Proxy();
                Method method = Proxy.class.getDeclaredMethod("run");
                method.invoke(target);
            }

            static class Proxy {
                public void run() {
                    System.out.println("run");
                }
            }
        }
```
通过Java的反射机制，可以在运行期间调用对象的任何方法，如果大量使用这种方式进行调用，会有性能或内存隐患么？为了彻底了解方法的反射机制，只能从底层代码入手了。

Method获取

调用`Class`类的`getDeclaredMethod`可以获取指定方法名和参数的方法对象`Method`

```JAVA
        @CallerSensitive
        public Field getDeclaredField(String name)
            throws NoSuchFieldException, SecurityException {
            checkMemberAccess(Member.DECLARED, Reflection.getCallerClass(), true);
            Field field = searchFields(privateGetDeclaredFields(false), name);
            if (field == null) {
                throw new NoSuchFieldException(name);
            }
            return field;
        }
```

其中`privateGetDeclaredMethods`方法从缓存或JVM中获取该`Class`中申明的方法列表，`searchMethods`方法将从返回的方法列表里找到一个匹配名称和参数的方法对象。

```java
        private static Method searchMethods(Method[] methods,
                                            String name,
                                            Class<?>[] parameterTypes)
        {
            Method res = null;
            String internedName = name.intern();
            for (int i = 0; i < methods.length; i++) {
                Method m = methods[i];
                if (m.getName() == internedName
                    && arrayContentsEq(parameterTypes, m.getParameterTypes())
                    && (res == null
                        || res.getReturnType().isAssignableFrom(m.getReturnType())))
                    res = m;
            }

            return (res == null ? res : getReflectionFactory().copyMethod(res));
        }
```

如果找到一个匹配的`Method`，则重新copy一份返回，即`Method.copy()`方法

```java
        Method copy() {
            // This routine enables sharing of MethodAccessor objects
            // among Method objects which refer to the same underlying
            // method in the VM. (All of this contortion is only necessary
            // because of the "accessibility" bit in AccessibleObject,
            // which implicitly requires that new java.lang.reflect
            // objects be fabricated for each reflective call on Class
            // objects.)
            if (this.root != null)
                throw new IllegalArgumentException("Can not copy a non-root Method");

            Method res = new Method(clazz, name, parameterTypes, returnType,
                                    exceptionTypes, modifiers, slot, signature,
                                    annotations, parameterAnnotations, annotationDefault);
            res.root = this;
            // Might as well eagerly propagate this if already present
            res.methodAccessor = methodAccessor;
            return res;
        }
```

privateGetDeclaredMethods

从缓存或JVM中获取该`Class`中申明的方法列表，实现如下：

```java
        private Field[] privateGetDeclaredFields(boolean publicOnly) {
            checkInitted();
            Field[] res;
            ReflectionData<T> rd = reflectionData();
            if (rd != null) {
                res = publicOnly ? rd.declaredPublicFields : rd.declaredFields;
                if (res != null) return res;
            }
            // No cached value available; request value from VM
            res = Reflection.filterFields(this, getDeclaredFields0(publicOnly));
            if (rd != null) {
                if (publicOnly) {
                    rd.declaredPublicFields = res;
                } else {
                    rd.declaredFields = res;
                }
            }
            return res;
        }
```

其中`reflectionData()`方法实现如下：

```java
        private ReflectionData<T> reflectionData() {
            SoftReference<ReflectionData<T>> reflectionData = this.reflectionData;
            int classRedefinedCount = this.classRedefinedCount;
            ReflectionData<T> rd;
            if (useCaches &&
                reflectionData != null &&
                (rd = reflectionData.get()) != null &&
                rd.redefinedCount == classRedefinedCount) {
                return rd;
            }
            // else no SoftReference or cleared SoftReference or stale ReflectionData
            // -> create and replace new instance
            return newReflectionData(reflectionData, classRedefinedCount);
        }
```

这里有个比较重要的数据结构`ReflectionData`，用来缓存从JVM中读取类的如下属性数据：

```java
        private static class ReflectionData<T> {
            volatile Field[] declaredFields;
            volatile Field[] publicFields;
            volatile Method[] declaredMethods;
            volatile Method[] publicMethods;
            volatile Constructor<T>[] declaredConstructors;
            volatile Constructor<T>[] publicConstructors;
            // Intermediate results for getFields and getMethods
            volatile Field[] declaredPublicFields;
            volatile Method[] declaredPublicMethods;
            volatile Class<?>[] interfaces;

            // Value of classRedefinedCount when we created this ReflectionData instance
            final int redefinedCount;

            ReflectionData(int redefinedCount) {
                this.redefinedCount = redefinedCount;
            }
        }
从reflectionData()方法实现可以看出：reflectionData对象是SoftReference类型的，说明在内存紧张时可能会被回收，不过也可以通过-XX:SoftRefLRUPolicyMSPerMB参数控制回收的时机，只要发生GC就会将其回收，如果reflectionData被回收之后，又执行了反射方法，那只能通过newReflectionData方法重新创建一个这样的对象了，newReflectionData方法实现如下：
         private ReflectionData<T> newReflectionData(SoftReference<ReflectionData<T>> oldReflectionData,
                                                        int classRedefinedCount) {
                if (!useCaches) return null;

                while (true) {
                    ReflectionData<T> rd = new ReflectionData<>(classRedefinedCount);
                    // try to CAS it...
                    if (Atomic.casReflectionData(this, oldReflectionData, new SoftReference<>(rd))) {
                        return rd;
                    }
                    // else retry
                    oldReflectionData = this.reflectionData;
                    classRedefinedCount = this.classRedefinedCount;
                    if (oldReflectionData != null &&
                        (rd = oldReflectionData.get()) != null &&
                        rd.redefinedCount == classRedefinedCount) {
                        return rd;
                    }
                }
            }
通过unsafe.compareAndSwapObject方法重新设置reflectionData字段;
在privateGetDeclaredMethods方法中，如果通过reflectionData()获得的ReflectionData对象不为空，则尝试从ReflectionData对象中获取declaredMethods属性，如果是第一次，或则被GC回收之后，重新初始化后的类属性为空，则需要重新到JVM中获取一次，并赋值给ReflectionData，下次调用就可以使用缓存数据了。
Method调用

获取到指定的方法对象Method之后，就可以调用它的invoke方法了，invoke实现如下：
```

```java
        @CallerSensitive
        public Object invoke(Object obj, Object... args)
            throws IllegalAccessException, IllegalArgumentException,
               InvocationTargetException
        {
            if (!override) {
                if (!Reflection.quickCheckMemberAccess(clazz, modifiers)) {
                    Class<?> caller = Reflection.getCallerClass();
                    checkAccess(caller, clazz, obj, modifiers);
                }
            }
            MethodAccessor ma = methodAccessor;             // read volatile
            if (ma == null) {
                ma = acquireMethodAccessor();
            }
            return ma.invoke(obj, args);
        }
```

应该注意到：这里的`MethodAccessor`对象是`invoke`方法实现的关键，一开始`methodAccessor`为空，需要调用`acquireMethodAccessor`生成一个新的`MethodAccessor`对象，`MethodAccessor`本身就是一个接口，实现如下：

```java
public interface MethodAccessor {
    Object invoke(Object var1, Object[] var2) throws IllegalArgumentException, InvocationTargetException;
}

在acquireMethodAccessor方法中，会通过ReflectionFactory类的newMethodAccessor创建一个实现了MethodAccessor接口的对象，实现如下：
        public MethodAccessor newMethodAccessor(Method var1) {
            checkInitted();
            if (noInflation && !ReflectUtil.isVMAnonymousClass(var1.getDeclaringClass())) {
                return (new MethodAccessorGenerator()).generateMethod(var1.getDeclaringClass(), var1.getName(), var1.getParameterTypes(), var1.getReturnType(), var1.getExceptionTypes(), var1.getModifiers());
            } else {
                NativeMethodAccessorImpl var2 = new NativeMethodAccessorImpl(var1);
                DelegatingMethodAccessorImpl var3 = new DelegatingMethodAccessorImpl(var2);
                var2.setParent(var3);
                return var3;
            }
        }
```

在`ReflectionFactory`类中，有2个重要的字段：`noInflation`(默认`false`)和`inflationThreshold`(默认15)，在`checkInitted`方法中可以通过`-Dsun.reflect.inflationThreshold=xxx`和`-Dsun.reflect.noInflation=true`对这两个字段重新设置，而且只会设置一次；

如果`noInflation`为`false`，方法`newMethodAccessor`都会返回`DelegatingMethodAccessorImpl`对象，`DelegatingMethodAccessorImpl`的类实现

```java
        class DelegatingMethodAccessorImpl extends MethodAccessorImpl {
            private MethodAccessorImpl delegate;

            DelegatingMethodAccessorImpl(MethodAccessorImpl var1) {
                this.setDelegate(var1);
            }

            public Object invoke(Object var1, Object[] var2) throws IllegalArgumentException, InvocationTargetException {
                return this.delegate.invoke(var1, var2);
            }

            void setDelegate(MethodAccessorImpl var1) {
                this.delegate = var1;
            }
        }
```

其实，`DelegatingMethodAccessorImpl`对象就是一个代理对象，负责调用被代理对象`delegate`的`invoke`方法，其中`delegate`参数目前是`NativeMethodAccessorImpl`对象，所以最终`Method`的`invoke`方法调用的是`NativeMethodAccessorImpl`对象`invoke`方法，实现如下：

```java
    class NativeMethodAccessorImpl extends MethodAccessorImpl {
        private final Method method;
        private DelegatingMethodAccessorImpl parent;
        private int numInvocations;

        NativeMethodAccessorImpl(Method var1) {
            this.method = var1;
        }

        public Object invoke(Object var1, Object[] var2) throws IllegalArgumentException, InvocationTargetException {
            if (++this.numInvocations > ReflectionFactory.inflationThreshold() && !ReflectUtil.isVMAnonymousClass(this.method.getDeclaringClass())) {
                MethodAccessorImpl var3 = (MethodAccessorImpl)(new MethodAccessorGenerator()).generateMethod(this.method.getDeclaringClass(), this.method.getName(), this.method.getParameterTypes(), this.method.getReturnType(), this.method.getExceptionTypes(), this.method.getModifiers());
                this.parent.setDelegate(var3);
            }

            return invoke0(this.method, var1, var2);
        }

        void setParent(DelegatingMethodAccessorImpl var1) {
            this.parent = var1;
        }

        private static native Object invoke0(Method var0, Object var1, Object[] var2);
    }
这里用到了ReflectionFactory类中的inflationThreshold，当delegate调用了15次invoke方法之后，如果继续调用就通过MethodAccessorGenerator类的generateMethod方法生成MethodAccessorImpl对象，并设置为delegate对象，这样下次执行Method.invoke时，就调用新建的MethodAccessor对象的invoke()方法了。

这里需要注意的是：
generateMethod方法在生成MethodAccessorImpl对象时，会在内存中生成对应的字节码，并调用ClassDefiner.defineClass创建对应的class对象，实现如下：
         return (MagicAccessorImpl)AccessController.doPrivileged(new PrivilegedAction<MagicAccessorImpl>() {
                        public MagicAccessorImpl run() {
                            try {
                                return (MagicAccessorImpl)ClassDefiner.defineClass(var13, var17, 0, var17.length, var1.getClassLoader()).newInstance();
                            } catch (IllegalAccessException | InstantiationException var2) {
                                throw new InternalError(var2);
                            }
                        }
                    });
在ClassDefiner.defineClass方法实现中，每被调用一次都会生成一个DelegatingClassLoader类加载器对象
        static Class<?> defineClass(String var0, byte[] var1, int var2, int var3, final ClassLoader var4) {
            ClassLoader var5 = (ClassLoader)AccessController.doPrivileged(new PrivilegedAction<ClassLoader>() {
                public ClassLoader run() {
                    return new DelegatingClassLoader(var4);
                }
            });
            return unsafe.defineClass(var0, var1, var2, var3, var5, (ProtectionDomain)null);
        }
```

这里每次都生成新的类加载器，是为了性能考虑，在某些情况下可以卸载这些生成的类，因为类的卸载是只有在类加载器可以被回收的情况下才会被回收的，如果用了原来的类加载器，那可能导致这些新创建的类一直无法被卸载，从其设计来看本身就不希望这些类一直存在内存里的，在需要的时候有就行了。

# 5.从100亿条记录的文本文件中取出重复数最多的前十条。

第一步：Query统计
    Query统计有以下俩个方法，可供选择：

1、直接排序法
	首先我们最先想到的的算法就是排序了，首先对这个日志里面的所有Query都进行排序，然后再遍历排好序的Query，统计每个Query出现的次数了。

```
但是题目中有明确要求，那就是内存不能超过1G，一千万条记录，每条记录是255Byte，很显然要占据2.375G内存，这个条件就不满足要求了。

让我们回忆一下数据结构课程上的内容，当数据量比较大而且内存无法装下的时候，我们可以采用外排序的方法来进行排序，这里我们可以采用归并排序，因为归并排序有一个比较好的时间复杂度O(NlgN)。

排完序之后我们再对已经有序的Query文件进行遍历，统计每个Query出现的次数，再次写入文件中。

综合分析一下，排序的时间复杂度是O(NlgN)，而遍历的时间复杂度是O(N)，因此该算法的总体时间复杂度就是O(N+NlgN)=O（NlgN）。

2、Hash Table法
在第1个方法中，我们采用了排序的办法来统计每个Query出现的次数，时间复杂度是NlgN，那么能不能有更好的方法来存储，而时间复杂度更低呢？

题目中说明了，虽然有一千万个Query，但是由于重复度比较高，因此事实上只有300万的Query，每个Query255Byte，因此我们可以考虑把他们都放进内存中去，而现在只是需要一个合适的数据结构，在这里，Hash Table绝对是我们优先的选择，因为Hash Table的查询速度非常的快，几乎是O(1)的时间复杂度。

那么，我们的算法就有了：维护一个Key为Query字串，Value为该Query出现次数的HashTable，每次读取一个Query，如果该字串不在Table中，那么加入该字串，并且将Value值设为1；如果该字串在Table中，那么将该字串的计数加一即可。最终我们在O(N)的时间复杂度内完成了对该海量数据的处理。

本方法相比算法1：在时间复杂度上提高了一个数量级，为O（N），但不仅仅是时间复杂度上的优化，该方法只需要IO数据文件一次，而算法1的IO次数较多的，因此该算法2比算法1在工程上有更好的可操作性。
```

第二步：找出Top 10
    算法一：普通排序
    我想对于排序算法大家都已经不陌生了，这里不在赘述，我们要注意的是排序算法的时间复杂度是NlgN，在本题目中，三百万条记录，用1G内存是可以存下的。

1. ```
   算法二：部分排序
   题目要求是求出Top 10，因此我们没有必要对所有的Query都进行排序，我们只需要维护一个10个大小的数组，初始化放入10个Query，按照每个Query的统计次数由大到小排序，然后遍历这300万条记录，每读一条记录就和数组最后一个Query对比，如果小于这个Query，那么继续遍历，否则，将数组中最后一条数据淘汰，加入当前的Query。最后当所有的数据都遍历完毕之后，那么这个数组中的10个Query便是我们要找的Top10了。
   
   不难分析出，这样，算法的最坏时间复杂度是N*K， 其中K是指top多少。
   
   算法三：堆
   在算法二中，我们已经将时间复杂度由NlogN优化到NK，不得不说这是一个比较大的改进了，可是有没有更好的办法呢？
   
   分析一下，在算法二中，每次比较完成之后，需要的操作复杂度都是K，因为要把元素插入到一个线性表之中，而且采用的是顺序比较。这里我们注意一下，该数组是有序的，一次我们每次查找的时候可以采用二分的方法查找，这样操作的复杂度就降到了logK，可是，随之而来的问题就是数据移动，因为移动数据次数增多了。不过，这个算法还是比算法二有了改进。
   
   基于以上的分析，我们想想，有没有一种既能快速查找，又能快速移动元素的数据结构呢？回答是肯定的，那就是堆。
   借助堆结构，我们可以在log量级的时间内查找和调整/移动。因此到这里，我们的算法可以改进为这样，维护一个K(该题目中是10)大小的小根堆，然后遍历300万的Query，分别和根元素进行对比。
   
   具体过程是，堆顶存放的是整个堆中最小的数，现在遍历N个数，把最先遍历到的k个数存放到最小堆中，并假设它们就是我们要找的最大的k个数，X1>X2...Xmin(堆顶)，而后遍历后续的N-K个数，一一与堆顶元素进行比较，如果遍历到的Xi大于堆顶元素Xmin，则把Xi放入堆中，而后更新整个堆，更新的时间复杂度为logK，如果Xi<Xmin，则不更新堆，整个过程的复杂度为O(K)+O(（N-K）*logK)=O（N*logK）。
   
   
   ```

# 6.堆排序

关于堆的一些知识点回顾

1.堆是一个完全二叉树
2.完全二叉树即是：若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。
3.堆满足两个性质: 堆的每一个父节点数值都大于（或小于）其子节点，堆的每个左子树和右子树也是一个堆。
4.堆分为最小堆和最大堆。最大堆就是每个父节点的数值要大于孩子节点，最小堆就是每个父节点的数值要小于孩子节点。排序要求从小到大的话，我们需要建立最大堆，反之建立最小堆。
5.堆的存储一般用数组来实现。假如父节点的数组下标为i的话，那么其左右节点的下标分别为：(2*i+1)和 (2*i+2)。如果孩子节点的下标为j的话，那么其父节点的下标为(j-1)/2。
6.完全二叉树中，假如有n个元素，那么在堆中最后一个父节点位置为(n/2-1)。

## 算法思想及实现

1. 建立堆

2. 调整堆

3. 交换堆顶元素和堆的最后一个元素

   算法的两种实现方式：

```java
        //自下而上
        public static void buildHeap(int[] arr){
            int size = arr.length;
            //从最后一个节点遍历，对整棵树进行大根堆的调整
            //也就是说，是按照自下而上，每一层都是自左向右来进行调整的

            for(int i=0;i<size;i++){
                int currentIndex = i;
                int fatherIndex = (i-1)/2;
                // 堆调整的逻辑在建堆和后续排序过程中复用的
                while(arr[currentIndex]>arr[fatherIndex]){
                    swap(arr,currentIndex,fatherIndex);
                    currentIndex = fatherIndex;
                    fatherIndex = (fatherIndex-1)/2;
                }
            }
        }

        public static void abjustHeap(int []arr){
            int size = arr.length;
            // 按照完全二叉树的特点，从最后一个非叶子节点开始，对于整棵树进行大根堆的调整
            // 也就是说，是按照自下而上，每一层都是自右向左来进行调整的
            // 注意，这里元素的索引是从0开始的
            // 另一件需要注意的事情，这里的建堆，是用堆调整的方式来做的
            // 堆调整的逻辑在建堆和后续排序过程中复用的
            for(int i=(size-1)/2;i>=0;i--){
                // 可以参照sort中的调用逻辑，在堆建成，且完成第一次交换之后，实质上i=0；也就是说，是从根所在的最小子树开始调整的
                // 接下来的讲解，都是按照i的初始值为0来讲述的
                // 这一段很好理解，如果i=0；则k=1；k+1=2
                // 实质上，就是根节点和其左右子节点记性比较，让k指向这个不超过三个节点的子树中最大的值
                // 这里，必须要说下为什么k值是跳跃性的。
                // 首先，举个例子，如果a[0] > a[1]&&a[0]>a[2],说明0,1,2这棵树不需要调整，那么，下一步该到哪个节点了呢？肯定是a[1]所在的子树了，
                // 也就是说，是以本节点的左子节点为根的那棵小的子树
                // 而如果a[0}<a[2]呢，那就调整a[0]和a[2]的位置，然后继续调整以a[2]为根节点的那棵子树，而且肯定是从左子树开始调整的
                // 所以，这里面的用意就在于，自上而下，自左向右一点点调整整棵树的部分，直到每一颗小子树都满足大根堆的规律为止
                for(int k=2*i+1;k<size;k=2*k+1){
                    // 让k先指向子节点中最大的节点
                    if(k+1<size&&arr[k]<arr[k+1]){
                        k++;
                    }
                    // 如果发现子节点更大，则进行值的交换
                    if(arr[k]>arr[i]){
                        swap(arr,k,i);
                        // 下面就是非常关键的一步了
                        // 如果子节点更换了，那么，以子节点为根的子树会不会受到影响呢？
                        // 所以，循环对子节点所在的树继续进行判断
                        i=k;
                    }else{
                        // 如果不用交换，那么，就直接终止循环了
                        break;
                    }
                }
            }
        }

```