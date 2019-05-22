# MyBatis的底层实现原理

动态代理的功能：通过拦截器方法回调，对目标target方法进行增强。

言外之意就是为了增强目标target方法。上面这句话没错，但也不要认为它就是真理，殊不知，动态代理还有投鞭断流的霸权，连目标target都不要的科幻模式。

注：本文默认认为，读者对动态代理的原理是理解的，如果不明白target的含义，难以看懂本篇文章，建议先理解动态代理。

一、自定义JDK动态代理之投鞭断流实现自动映射器Mapper

首先定义一个pojo

1. ![](E:\学习\MyBatis的底层实现原理\20180911105349654.png)


再定义一个接口UserMapper.java

![2](E:\学习\MyBatis的底层实现原理\20180911105358419.png)

接下来我们看看如何使用动态代理之投鞭断流，实现实例化接口并调用接口方法返回数据的。

自定义一个InvocationHandler。

![](E:\学习\MyBatis的底层实现原理\20180911105427832.png)

上面代码中的target，在执行Object.java内的方法时，target被指向了this，target已经变成了傀儡、象征、占位符。在投鞭断流式的拦截时，已经没有了target。

写一个测试代码：

![](E:\学习\MyBatis的底层实现原理\20180911105509888.png)

output：

![](E:\学习\MyBatis的底层实现原理\20180911105519854.png)

这便是Mybatis自动映射器Mapper的底层实现原理。

## **二、Mybatis自动映射器Mapper的源码分析**

首先编写一个测试类：

![](E:\学习\MyBatis的底层实现原理\2018091110553168.png)

Mapper长这个样子：

![](E:\学习\MyBatis的底层实现原理\20180911105538585.png)

org.apache.ibatis.binding.MapperProxy.java部分源码。

![](E:\学习\MyBatis的底层实现原理\20180911105547458.png)

org.apache.ibatis.binding.MapperProxyFactory.java部分源码。

![](E:\学习\MyBatis的底层实现原理\2018091110555578.png)

这便是Mybatis使用动态代理之投鞭断流。

## 三、接口Mapper内的方法能重载（overLoad）吗？（重要）

类似下面：

![](E:\学习\MyBatis的底层实现原理\20180911105602658.png)

**Answer：不能。**

原因：在投鞭断流时，Mybatis使用package+Mapper+method全限名作为key，去xml内寻找唯一sql来执行的。类似：key=x.y.UserMapper.getUserById，那么，重载方法时将导致矛盾。对于Mapper接口，Mybatis禁止方法重载（overLoad）

![](E:\学习\MyBatis的底层实现原理\20190304182351171.png)

![](E:\学习\MyBatis的底层实现原理\20190304182410373.png)