# CoreDNS

## Getting And Install

在需要作为DNS服务器上安装如下

```
$ yum -y install bind bind-chroot bind-utils
```

{% hint style="info" %}
 dns服务可以是内部机器，注意：DNS服务器名是named，端口53用于客户端查询，端口953用于DNS主从同步
{% endhint %}

DNS相关配置文件：

/etc/named.conf 是BIND的核心配置文件，包含了BIND 的基本配置，但其并不包括区域数据。

/var/named/目录是DNS数据库文件存放目录，每一个域文件都放在这里。

**1.设置DNS主配置文件**：

{% code title="/etc/named.conf" %}
```bash
（1）设置监听主机为本机：整行注释掉或改为localhost

     //   listen-on port 53 { localhost; };

（2）设置允许查询的主机：整行注释掉或改为any

    //      allow-query     { any; };
```
{% endcode %}

以上两条配置可为局域网内客户端访问互联网域名提供解析服务；

**2.添加区域管理名称：**

```text
#vim /etc/named.rfc1912.zones
zone "i3hh.com" IN {                                                                                                                                                     
        type master;
        file "i3hh.com.zone";
};
```

**3.添加区域数据库**

```text
# vim /var/named/i3hh.com.zone
$TTL   1D
@      IN SOA master admin (1 3H 1M 1D 3H )
       NS master
master IN A 192.168.56.1
git    IN A 192.168.56.1
run    IN A 192.168.56.1
reg    IN A 192.168.56.1

```

**4.修改数据库文件所属组以及权限**

```text
chgrp named /var/named/test.com.zone 
chmod 640 /var/named/test.com.zone 
```

**5.启动服务/重新加载配置文件**

```text
systemctl start named
rndc reload
```

**6. CoreDNS的configMap配置**

```text
#key=Corefile ,增加i3hh.com:53

i3hh.com:53 {
   errors
   cache 30
   forward . 192.168.56.101
}
.:53 {
    errors
    health {
      lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
      pods insecure
      fallthrough in-addr.arpa ip6.arpa
    }
    prometheus :9153
    forward . "/etc/resolv.conf"
    cache 30
    loop
    reload
    loadbalance
}
```

pod可使用配置的域名服务器了

参考：

[CoreDNS](https://www.codercto.com/a/80817.html)

