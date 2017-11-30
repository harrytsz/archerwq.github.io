---
layout: post
title: 基于Docker搭建本地Jykell环境
date: 2017-09-21
banner_image: jekyll_and_docker.jpg
tags: [Docker, Jekyll]
---

在本地安装Jekyll虽然不是很复杂，但对于不懂Ruby的小白用户来说，安装Ruby, RubyGems, GCC&Make也不是很轻松，还好有Docker，可以一键Setup.

<!--more-->

##### 安装Docker
参考：[Install Docker](https://docs.docker.com/engine/installation/)

##### 创建docker-compose.yml

```yaml
jekyll:
    image: jekyll/jekyll:pages
    command: jekyll serve --watch
    ports:
        - 3999:4000
    volumes:
        - ~/dev/git/archerwq.github.io:/srv/jekyll/
```

* **jekyll/jekyll:pages** 是专门适用于Github Pages的Jekyll镜像
* **jekyll serve --watch** 启动容器时运行的命令，这个命令会启动Jekyll内置的用于开发环境的Web服务器，**--watch**参数表示有任何文件变化时自动重新生成网页
* **3999:4000** 把容器的4000端口映射到宿主机的3999端口
* **~/dev/git/archerwq.github.io:/srv/jekyll/** 把宿主机上的Jekyll Site所在目录映射到容器的**/srv/jekyll/**目录，Jekyll默认从这个目录读Jekyll Site


##### 启动Docker容器
在docker-compose.yml所在目录运行 **docker-compose up** 命令， 如下:    
```shell
C02PJ5CZG3QC-2:blog qwang$ docker-compose up
Starting blog_jekyll_1
Attaching to blog_jekyll_1
jekyll_1  | Configuration file: /srv/jekyll/_config.yml
jekyll_1  |             Source: /srv/jekyll
jekyll_1  |        Destination: /srv/jekyll/_site
jekyll_1  |  Incremental build: disabled. Enable with --incremental
jekyll_1  |       Generating...
jekyll_1  |                     done in 5.404 seconds.
jekyll_1  |  Auto-regeneration: enabled for '/srv/jekyll'
jekyll_1  |     Server address: http://0.0.0.0:4000
jekyll_1  |   Server running... press ctrl-c to stop.
```


这样你就可以在本地编辑文章，然后通过访问 `http://localhost:3999` 动态的看到文章的显示效果。
