# 使用logstash处理金证mid日志
金证mid支持udp日志，配置
cfglog.dat 如下，日志以udp发往 127.0.0.1:5143 端口：
```
log4cplus.rootLogger=INFO, ALL_MSGS
log4cplus.appender.ALL_MSGS=log4cplus::SysLogAppender
log4cplus.appender.ALL_MSGS.facility=local0
log4cplus.appender.ALL_MSGS.host=127.0.0.1
log4cplus.appender.ALL_MSGS.port=5143
```
config.ini 如下：
```
是否启用KDGW日志=YES
;当为YES时，mid第三方接口的日志会由log4cplus输出，run.log只记error级别的日志
```

udp的接收程序处理速度必须足够快，否则丢包。  
因此logstash采用两级模式：logstash01接收放到redis，logstash02解析入库elasticsearch

/etc/redis.conf 配置，防止内存爆掉。满了后会丢数据。
```
port 7379
maxmemory 2000mb
maxmemory-policy allkeys-lru
```

/etc/sysctl.conf 配置，增大udp接收buffer
```
# logstash udp input
net.core.rmem_default = 77000000
net.core.rmem_max     = 77777000
net.core.netdev_max_backlog = 1000

# redis
vm.overcommit_memory = 1
net.core.somaxconn = 65535
```

/etc/security/limits.conf
```
*  soft  nproc  16384
*  hard  nproc  16384
*  soft  nofile 65536
*  hard  nofile 65536
```

grep -v "^#" /etc/logstash/logstash.yml
```
path.data: /var/lib/logstash
pipeline.workers: 12
pipeline.output.workers: 12
pipeline.batch.size: 800
path.config: /etc/logstash/conf.d
config.reload.automatic: true
path.logs: /var/log/logstash
```

grep -v "^#" /etc/logstash/jvm.options |grep -v "^$"
```
-Xms1g
-Xmx1g
-XX:+UseParNewGC
-XX:+UseConcMarkSweepGC
-XX:CMSInitiatingOccupancyFraction=75
-XX:+UseCMSInitiatingOccupancyOnly
-XX:+DisableExplicitGC
-Djava.awt.headless=true
-Dfile.encoding=UTF-8
-XX:+HeapDumpOnOutOfMemoryError
```

监控redis队列深度
```
watch "redis-cli -p 7379 LLEN logstash"
```

监控udp有无丢包
```
watch "netstat -us|grep packet"
```
![image](https://user-images.githubusercontent.com/23710675/117418758-5f882580-af4e-11eb-9428-241282c46766.png)
