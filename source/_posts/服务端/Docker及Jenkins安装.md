---
title: Docker及Jenkins安装
date: 2021-06-17 15:22:35
tags:
cover: /img/article/leetcode/primary/primary.png
top_img: /img/article/leetcode/primary/primary.png
description: 从 0 到 1 实现一套 CI/CD 流程
---

## 服务环境

三台服务器

## 安装 `Docker`

1. 需要安装 device-mapper-persistent-data 和 lvm2 两个依赖。

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

2. 添加阿里云 `Docker` 镜像源

```shell
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum install docker-ce -y
```

3. 启动 `Docker`

```shell
systemctl start docker
systemctl enable docker
```

4. 执行`docker -v`查看版本

```shell
docker -v
```

5. 配置阿里云镜像

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://qphcniic.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 安装 Jenkins

1. JDK 安装,1.8 版本的无法启动 Jenkins,可安装更高版本的 Java

```shell
yum install -y java
```

2. 使用 Yum 安装 Jenkins

```shell
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install jenkins
```

3. 启动 Jenkins

```shell
service jenkins start
# service jenkins restart restart 重启 Jenkins
# service jenkins restart stop 停止 Jenkins
```

4. 放行端口,默认为 8080

使用服务器厂商的,还需要在服务器控制台中放行端口

```shell
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=50000/tcp --permanent

systemctl reload firewalld
```

5. 在浏览器输入 IP 加端口,可以看见 Jenkins 界面,初次加载速度较慢

## 配置 Jenkins

加载完毕后会输入初始密码

1. 获取密码

```shell
cat /var/lib/jenkins/secrets/initialAdminPassword
```

2. 替换国内原

来到插件安装界面先替换安装源,否则加载巨慢......

```shell
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' /var/lib/jenkins/updates/default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' /var/lib/jenkins/updates/default.json
```

替换完成后点击`安装推荐的插件`

3. 安装完成之后,根据提示创建管理员账号

4. 将`Jenkins`加入`Docker`用户组

```shell
sudo groupadd docker          #新增docker用户组
sudo gpasswd -a jenkins docker  #将当前用户添加至docker用户组
newgrp docker                 #更新docker用户组
```

4. 重启 `Jenkins`

```shell
sudo service jenkins restart
```
