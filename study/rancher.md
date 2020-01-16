# Rancher

## Environment

hosts info

```text
#rancher port 8090 1443 admin/admin
127.0.0.1 run.i3hh.com
#harbor port 8091 1444 admin/Abc
127.0.0.1 reg.i3hh.com
#gitlab port 8092 1445
127.0.0.1 git.i3hh.com
```

## Install for docker

Exceute shell bash on your local host:

```
docker run -d --restart=no -v /Users/chenfeng/rancher:/var/lib/rancher/ -p 8090:80 -p 1443:443 rancher/rancher:stable
```

{% hint style="info" %}
 &lt;host path&gt; = /Users/chenfeng/rancher
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}



