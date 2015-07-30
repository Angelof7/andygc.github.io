---
layout: post
title: 监控远程服务器JVM
categories: [server]
tags: [JVM]
---

###远程服务器配置:
-----------------
在tomcat中的bin/catalina.sh中添加:  
{% highlight sh %}
JAVA_OPTS="$JAVA_OPTS  
 -Djava.rmi.server.hostname=nts3.photo.163.org  
 -Dcom.sun.management.jmxremote.port=8950  
 -Dcom.sun.management.jmxremote.authenticate=false  
 -Dcom.sun.management.jmxremote.ssl=false"  
{% endhighlight %}

###JConsole
-----------------
![jconsole.png][1]

###VisualVM
-----------------
![jvisualvm.png][2]

###参考
[Monitoring and Management Using JMX](http://docs.oracle.com/javase/1.5.0/docs/guide/management/agent.html)

  [1]: /album/2015/jconsole.png
  [2]: /album/2015/jvisualvm.png
