title: CentOS 安装 Nginx
date: 2019-09-17 17:29:08
tags: [DevOps, Nginx]

---

### 安装
安装必备组件及 Nginx

```bash
# 安装 gcc g++
yum -y install gcc automake autoconf libtool make
yum install gcc gcc-c++

# 安装 openssl pcre zlib nginx
yum -y install openssl openssl-devel
yum -y install pcre pcre-devel
yum -y install zlib zlib-devel
yum -y install nginx
```

### 配置
开机自动启动 Nginx

```bash
sudo systemctl enable nginx.service
```

Nginx 相关配置目录
* `/usr/share/nginx/html` 网站文件存放默认目录
* `/etc/nginx/conf.d/default.conf` 网站默认站点配置
* `/etc/nginx/conf.d/` 自定义配置文件
* `/etc/nginx/nginx.conf` 全局配置

Nginx 相关命令
```bash
# 启动
nginx
# test
nginx -t
# 重启
nginx -s reload
# 查看进程
ps -ef | grep nginx
# 关闭进程
kill -9 <pid>
```
