# gitlab

## gitlab docker 安装

[https://gitlab.dev](https://gitlab.dev)

```text
sudo docker run --detach \  --hostname gitlab.dev \  --publish 80:80 --publish 2289:22 --publish 443:443 \  --name gitlab \  --restart always \  --volume /Users/chenfeng/gitlab/config:/etc/gitlab \  --volume /Users/chenfeng/gitlab/logs:/var/log/gitlab \  --volume /Users/chenfeng/gitlab/data:/var/opt/gitlab \  gitlab/gitlab-ee:latest
```

## 修改

* 配置gitlab.rb的扩展地址

```text
external_url 'https://gitlab.dev'
```

注意：端口目前只能是80

