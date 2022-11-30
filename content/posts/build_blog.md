---
title: "搭建博客记录"
date: 2022-11-30T09:48:13+08:00
description: "使用Hugo、腾讯云、docker搭建博客的记录"
---

![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221130095116.png)

# 安装Hugo

参考 Hugo [官方文档 - Quick Start](https://gohugo.io/getting-started/quick-start/)

```
 安装 Hugo
brew install hugo

# 创建一个新的网站（本地），blog是目录名，网站所有资源都放在这个根目录下
hugo new site blog && cd blog

# 拉取并设置主题
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo theme = \"ananke\" >> config.toml

# 创建一篇新文章
hugo new posts/my-first-post.md

# 构建静态网站文件，并启动 HTTP 服务
hugo server -D
```

启动后可本地访问

![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221129193238.png)

# 推送远端仓库

* 国内使用[coding](https://coding.net/)作为代码仓库
* 国外使用[github](https://github.com/)作为代码仓库

```
cd blog
## coding
git remote add coding git@e.coding.net:minhzzz/blog/blog.git
## github
git remote add github git@github.com:szuming/blog.git
git push -u coding "master"
git push -u github "master"
```



# 服务器搭建

## 购买腾讯云

[【腾讯云】(2核2G 低至50元/1年)](https://cloud.tencent.com/act/cps/redirect?redirect=2853&cps_key=56c72db760611c062c55541c1049b3c8) 

## 远程登录配置

1. 创建密钥![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221129195450.png)

2. 配置密钥![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221129195536.png)

3. weterm配置登录信息

   ![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221129200405.png)

   

## 安装Git与Docker

```
yum install git
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```



## 初始化配置

```
git config http.postBuffer 524288000
git config --global credential.helper store
git clone git@e.coding.net:minhzzz/blog/blog.git
## 上边语句 Clone private 仓库需要输入用户名密码
cd blog
git pull
## 重新拉取主题
git submodule init
git submodule update

## 定时任务，每一分钟执行一次拉取仓库，实现实时自动更新
echo "*/1 * * * * cd /usr/local/blog && git pull" >> /var/spool/cron/root
## 启动docker
systemctl start docker
```



## 部署Hugo容器

```
docker run --rm -it   -v /usr/local/blog:/src   -p 1313:1313   klakegg/hugo   server -D
```



##  防火墙白名单添加端口号

![](https://mingpic-1253982367.cos.ap-shenzhen-fsi.myqcloud.com/pic/20221129213818.png)



## 验证网站是否成功拉起

http://43.139.210.218:1313



## 域名申购与绑定

https://console.cloud.tencent.com/domain/all-domain



### 参考

[老白码农在奋斗](https://panzhongxian.cn/cn/2022/10/build-personal-blog-step-by-step/)

