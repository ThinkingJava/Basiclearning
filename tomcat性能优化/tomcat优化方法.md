# 1. 服务器资源

  服务器所能提供CPU、内存、硬盘的性能对处理能力有决定性影响。
  (1) 对于高并发情况下会有大量的运算，那么CPU的速度会直接影响到处理速度。
  (2) 内存在大量数据处理的情况下，将会有较大的内存容量需求，可以用-Xmx -Xms -XX:MaxPermSize等参数对内存不同功能块进行划分。我们之前就遇到过内存分配不足，导致虚拟机一直处于full GC，从而导致处理能力严重下降。
  (3) 硬盘主要问题就是读写性能，当大量文件进行读写时，磁盘极容易成为性能瓶颈。最好的办法还是利用下面提到的缓存。

# 2. 利用缓存和压缩

  对于静态页面最好是能够缓存起来，这样就不必每次从磁盘上读。这里我们采用了Nginx作为缓存服务器，将图片、css、js文件都进行了缓存，有效的减少了后端tomcat的访问。

  另外，为了能加快网络传输速度，开启gzip压缩也是必不可少的。但考虑到tomcat已经需要处理很多东西了，所以把这个压缩的工作就交给前端的Nginx来完成。可以参考之前写的《[利用nginx加速web访问](http://passover.blog.51cto.com/2431658/588602)》。

  除了文本可以用gzip压缩，其实很多图片也可以用图像处理工具预先进行压缩，找到一个平衡点可以让画质损失很小而文件可以减小很多。曾经我就见过一个图片从300多kb压缩到几十kb，自己几乎看不出来区别。

# 3. 采用集群

  单个服务器性能总是有限的，最好的办法自然是实现横向扩展，那么组建tomcat集群是有效提升性能的手段。我们还是采用了Nginx来作为请求分流的服务器，后端多个tomcat共享session来协同工作。可以参考之前写的《[利用nginx+tomcat+memcached组建web服务器负载均衡](http://passover.blog.51cto.com/2431658/648182)》。

# 4. 优化tomcat参数

  这里以tomcat7的参数配置为例，需要修改conf/server.xml文件，主要是优化连接配置，关闭客户端dns查询。

```javascript
<Connector port="8080"              protocol="org.apache.coyote.http11.Http11NioProtocol"             connectionTimeout="20000"             redirectPort="8443"              maxThreads="500"              minSpareThreads="20"             acceptCount="100"            disableUploadTimeout="true"            enableLookups="false"              URIEncoding="UTF-8" />
```

# 5. 改用APR库

  tomcat默认采用的BIO模型，在几百并发下性能会有很严重的下降。tomcat自带还有NIO的模型，另外也可以调用APR的库来实现操作系统级别控制。

  NIO模型是内置的，调用很方便，只需要将上面配置文件中protocol修改成org.apache.coyote.http11.Http11NioProtocol，重启即可生效。上面配置我已经改过了，默认的是HTTP/1.1。