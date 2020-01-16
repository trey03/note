# Rancher

## Install for docker

Exceute shell bash on your local host:

```
$ sudo docker run -d --restart=unless-stopped -v <host path>:/var/lib/rancher/ -p 8090:80 -p 1443:443 rancher/rancher:stable

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



