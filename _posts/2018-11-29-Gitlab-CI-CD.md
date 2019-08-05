---
layout:     post
title:      "Gitlab CI/CD 自动构建"
subtitle:   "Gitlab CI"
date:       2018-11-29 12:00:00
author:     "pengcu"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - CI/CD
---
> 这篇文章主要介绍一下 GitLab CI 相关功能，并通过 GitLab + GitLab-Runner 实现自动化构建项目； 

<!--more-->

### Head
- `持续集成(Continuous Integration)` 是一种软件开发实践，每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。
- `持续部署（Continuous Deployment)` 是通过自动化的构建、测试和部署循环来快速交付高质量的产品。

### Content
GitLab CI/CD 是 GitLab 默认集成的 CI 功能。只要在项目仓库的根目录添加 `.gitlab-ci.yml` 文件，并且配置了 GitLab-Runner，项目会根据配置文件触发工作流。

#### GitLab + GitLab-Runner 搭建
搭建直接使用 docker compose 启动，compose 配置如下
``` sh
version: '3'
services:
  gitlab:
    image: 'gitlab/gitlab-ce:latest'
    restart: always
    container_name: gitlab
    hostname: 'url/ip'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
              external_url 'http://url/ip'
    ports:
      - '80:80'
      - '443:443'
      - '8022:22'
    volumes:
      - './gitlab/config:/etc/gitlab'
      - './gitlab/logs:/var/log/gitlab'
      - './gitlab/data:/var/opt/gitlab'  
  gitlab-runner:
    image: 'gitlab/gitlab-runner:alpine'
    restart: always
    network_mode: "host"
    container_name: gitlab-runner
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./config.toml:/etc/gitlab-runner/config.toml
```

在启动前，需要先生成 config.toml 配置文件；  
启动 docker-compose 后，需要进入容器执行注册，  
注册所需要的 GitLab 地址和 token 需要在 GitLab 设置中查看到。
``` sh
# 生成 Runner 配置文件
touch config.toml
# 启动 Runner
docker-compose up -d
# 激活 Runner
docker exec -it gitlab-runner gitlab-runner register
```

最后就是 gitlab 网页上的设置、配置项目内 .gitlab-ci.yml 文件。