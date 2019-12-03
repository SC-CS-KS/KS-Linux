# HOST

***主机名和域名的区别***
```md
主机名通常在局域网内使用，通过hosts文件，主机名就被解析到对应的IP。
域名通常在internet上使用，但是优先级低于 hosts 文件中内容，
因此如果你不想使用internet上的域名解析，可以更改自己的hosts文件，加入自己的域名解析。
```

## hostname
```md
hostname是Linux系统下的一个内核参数，它保存在/proc/sys/kernel/hostname下，但是它的值是Linux启动时从rc.sysinit读取的。
/etc/rc.d/rc.sysinit中HOSTNAME的取值来自与/etc/sysconfig/network下的HOSTNAME
```
```md
/etc/sysconfig/network 确实是hostname的配置文件
hostname的值跟该配置文件中的HOSTNAME有一定的关联关系，但是没有必然关系，
hostname的值来自内核参数/proc/sys/kernel/hostname，
如果我通过命令sysctl kernel.hostname=Test修改了内核参数，那么hostname就变为了Test了。
```
***修改了hostname后，如何使其立即生效而不用重启操作系统?***
```sh
# sysctl kernel.hostname=Test
或
# echo Test >/proc/sys/kernel/hostname
```
***修改hostname有几种方式？***
```md
hostname DB-Server  --运行后立即生效（新会话生效），但是在系统重启后会丢失所做的修改
sysctl kernel.hostname=DB-Server --运行后立即生效（新会话生效），但是在系统重启后会丢失所做的修改
修改/etc/sysconfig/network下的HOSTNAME变量 -- 需要重启生效，永久性修改
```
***hostname跟/etc/hosts 下配置有关系吗?***
```md
hostname的修改、变更完全不依赖hosts文件。
hosts文件的作用相当如DNS，早期的互联网计算机数量少，单机hosts文件里足够存放所有联网计算机。
```

## /etc/hosts
```md
hosts —— the static table lookup for host name（主机名查询静态表）。
是 Linux 系统上一个负责IP地址与域名快速解析的文件，还包括主机的别名。
```
```md
在没有域名解析服务器的情况下，系统上的所有网络程序都通过查询该文件来解析对应于某个主机名的IP地址，否则就需要使用DNS服务程序来解决。
优先级 ： DNS 缓存 > hosts > DNS服务
```
> * hosts格式配置
```md
ip地址   主机名/域名   （主机别名）
```