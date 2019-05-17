

https://gitlab.dev:8929

sudo docker run --detach \
  --hostname gitlab.dev \
  --publish 80:80 --publish 2289:22 --publish 443:443 \
  --name gitlab \
  --restart always \
  --volume /Users/chenfeng/gitlab/config:/etc/gitlab \
  --volume /Users/chenfeng/gitlab/logs:/var/log/gitlab \
  --volume /Users/chenfeng/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ee:latest
