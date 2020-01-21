# AENV

## Environment

hosts info

```text
#rancher port 8090 1443 admin/admin
127.0.0.1 run.i3hh.com
#harbor port 8091 1444 admin/Abc
127.0.0.1 reg.i3hh.com
#gitlab port 80 443
127.0.0.1 git.i3hh.com
```

## Install rancher

Exceute shell bash on your local host:

```
docker run -d --restart=no -v /Users/chenfeng/rancher:/var/lib/rancher/ -p 8090:80 -p 1443:443 rancher/rancher:stable
```

{% hint style="info" %}
 &lt;host path&gt; = /Users/chenfeng/rancher
{% endhint %}

[https://run.i3hh.com:1443/](https://run.i3hh.com:1443/)  admin/admin

* 使用域名时需要注意	cattle-cluster-agent 可能会启动失败，因为在启动需要通过域名来访问rancher集群，这时域名解析不到导致无法启动，解决办法是给cattle-cluster-agent手动配置一下host
* CoreDNS配置

## Install harbor

Download harbor and unzip to local host's path

{% code title="harbor-offline-installer-v1.9.1.tgz" %}
```bash
# unzip 
tar -zxf harbor-offline-installer-v1.9.1.tgz
# setting hostname、port in harbor.yml
# run prepare and install.sh
```
{% endcode %}

[http://reg.i3hh.com:8091/](http://reg.i3hh.com:8091/) admin/Abc

## Install gitlab

```text
sudo docker run --detach \
  --hostname git.i3hh.com \
  --publish 80:80 --publish 2289:22 --publish 443:443 \
  --name gitlab \
  --volume /Users/chenfeng/gitlab/config:/etc/gitlab \
  --volume /Users/chenfeng/gitlab/logs:/var/log/gitlab \
  --volume /Users/chenfeng/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ee:latest
```

[http://git.i3hh.com/](http://git.i3hh.com/)  root/abc



