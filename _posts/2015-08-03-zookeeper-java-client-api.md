---
layout: post
title: Apache ZooKeeper Java Client API 简介
categories: [distributed]
tags: [ZooKeeper]
---

### Apache ZooKeeper:
-----------------
ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、名字服务、分布式同步、组服务等。
ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。
ZooKeeper包含一个简单的原语集, 提供Java和C的接口。

### ZooKeeper 的安装:
-----------------
[ZooKeeper下载安装](http://zookeeper.apache.org)  

### Java Client 的使用:
---------------------
Watcher 在 ZooKeeper 是一个核心功能,Watcher 可以监控目录节点的数据变化以及子目录的变化,一旦这些状态发生变化,服务器就会通知所有设置在这个目录节点上的 Watcher,从而每个客户端都很快知道它所关注的目录节点的状态发生变化,而做出相应的反应.
可以设置观察的操作：exists,getChildren,getData

可以触发观察的操作：create,delete,setData

znode以某种方式发生变化时,“观察”(watch)机制可以让客户端得到通知.可以针对ZooKeeper服务的“操作”来设置观察,该服务的其他 操作可以触发观察.

比如,客户端可以对某个客户端调用exists操作,同时在它上面设置一个观察,如果此时这个znode不存在,则exists返回 false,如果一段时间之后,这个znode被其他客户端创建,则这个观察会被触发,之前的那个客户端就会得到通知.

说明: zookeeper客户端对server的操作都是不可回退的。
意 思是说，zk的客户端每次和server进行通信的时候，会记住server上最新的zxid。如果某个时刻，客户端和server断开了连接，那么等到 下次重新连接到集群中的机器上时，会检查当前连接上的那个server是否和client有相同的zxid，或者已经是更新的zxid了。一旦客户端发现 server的zxid比自己小，那么客户端会断开和这个server的连接，并且重新连接集群中的其它server.

1. 连接ZooKeeper服务器

{% highlight java %}
/***
*** [关于connectString服务器地址配置]
*** 格式: 192.168.1.1:2181,192.168.1.2:2181,192.168.1.3:2181
*** 这个地址配置有多个ip:port之间逗号分隔,底层操作:
*** ConnectStringParser connectStringParser =  new ConnectStringParser(“192.168.1.1:2181,192.168.1.2:2181,192.168.1.3:2181”);
*** 这个类主要就是解析传入地址列表字符串，将其它保存在一个ArrayList中
*** ArrayList<InetSocketAddress> serverAddresses = new ArrayList<InetSocketAddress>();
*** 接下去，这个地址列表会被进一步封装成StaticHostProvider对象，并且在运行过程中，一直是这个对象来维护整个地址列表。
*** ZK客户端将所有Server保存在一个List中，然后随机打乱(这个随机过程是一次性的)，并且形成一个环，具体使用的时候，从0号位开始一个一个使用。
*** 因此，Server地址能够重复配置，这样能够弥补客户端无法设置Server权重的缺陷，但是也会加大风险。
*** 
*** [客户端和服务端会话说明]
*** ZooKeeper中，客户端和服务端建立连接后，会话随之建立，生成一个全局唯一的会话ID(Session ID)。
*** 服务器和客户端之间维持的是一个长连接，在SESSION_TIMEOUT时间内，服务器会确定客户端是否正常连接(客户端会定时向服务器发送heart_beat，服务器重置下次SESSION_TIMEOUT时间)。
*** 因此，在正常情况下，Session一直有效，并且ZK集群所有机器上都保存这个Session信息。
*** 在出现网络或其它问题情况下（例如客户端所连接的那台ZK机器挂了，或是其它原因的网络闪断）,客户端与当前连接的那台服务器之间连接断了,这个时候客户端会主动在地址列表（实例化ZK对象的时候传入构造方法的那个参数connectString）中选择新的地址进行连接。
***
*** [会话时间]
*** 客户端并不是可以随意设置这个会话超时时间，在ZK服务器端对会话超时时间是有限制的，主要是minSessionTimeout和maxSessionTimeout这两个参数设置的。
*** 如果客户端设置的超时时间不在这个范围，那么会被强制设置为最大或最小时间。 默认的Session超时时间是在2 * tickTime ~ 20 * tickTime
*** @param connectString  Zookeeper服务地址
*** @param sessionTimeout Zookeeper连接超时时间
***/

public void connectionZookeeper(String connectString, int sessionTimeout){
    this.releaseConnection();
    try {
       // ZK客户端允许我们将ZK服务器的所有地址都配置在这里
       zk = new ZooKeeper(connectString, sessionTimeout, this );
       // 使用CountDownLatch.await()的线程（当前线程）阻塞直到所有其它拥有CountDownLatch的线程执行完毕（countDown()结果为0）
       connectedSemaphore.await();          
    } catch ( InterruptedException e  ) {
       LOG.error("连接创建失败，发生 InterruptedException , e " + e.getMessage(), e);         
    } catch ( IOException e  ) {
       LOG.error( "连接创建失败，发生 IOException , e " + e.getMessage(), e  );          
    }
}
{% endhighlight %}

2. 创建节点

{% highlight java %}
/*
* 创建zNode节点, String create(path<节点路径>, data[]<节点内容>, List(ACL访问控制列表), CreateMode<zNode创建类型>)
*    节点创建类型(CreateMode)
*    1、PERSISTENT:持久化节点
*    2、PERSISTENT_SEQUENTIAL:顺序自动编号持久化节点，这种节点会根据当前已存在的节点数自动加 1
*    3、EPHEMERAL:临时节点客户端,session超时这类节点就会被自动删除
*    4、EPHEMERAL_SEQUENTIAL:临时自动编号节点
* @param path zNode节点路径
* @param data zNode数据内容
* @return 创建成功返回true, 反之返回false.
*/

public boolean createPath( String path, String data  ) {
    try {
                    String zkPath =  this.zk.create(path, data.getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
                    LOG.info( "节点创建成功, Path: " + zkPath + ", content: " + data  );
                    return true;
                
    } catch ( KeeperException e  ) {
                    LOG.error( "节点创建失败, 发生KeeperException! path: " + path + ", data:" + data
                                                 + ", errMsg:" + e.getMessage(), e );
                
    } catch ( InterruptedException e  ) {
                    LOG.error( "节点创建失败, 发生 InterruptedException! path: " + path + ", data:" + data
                                                 + ", errMsg:" + e.getMessage(), e );
                
    }
            return false;
        
}
{% endhighlight %}

3. 删除节点

{% highlight java %}
/**
* <p>删除一个zMode节点, void delete(path<节点路径>, stat<数据版本号>)</p><br/>
* <pre>
*     说明
*     1、版本号不一致,无法进行数据删除操作.
*     2、如果版本号与znode的版本号不一致,将无法删除,是一种乐观加锁机制;如果将版本号设置为-1,不会去检测版本,直接删除.
* </pre>
* @param path zNode节点路径
* @return 删除成功返回true,反之返回false.
*/

public boolean deletePath( String path  ){
    try {
                    this.zk.delete(path,-1);
                    LOG.info( "节点删除成功, Path: " + path );
                    return true;
                
    } catch ( KeeperException e  ) {
                    LOG.error( "节点删除失败, 发生KeeperException! path: " + path
                                                 + ", errMsg:" + e.getMessage(), e );
                
    } catch ( InterruptedException e  ) {
                    LOG.error( "节点删除失败, 发生 InterruptedException! path: " + path
                                                 + ", errMsg:" + e.getMessage(), e );
                
    }
            return false;
        
}
{% endhighlight %}

4. 节点赋值/更新节点

