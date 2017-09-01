# Linux源码安装nginx实现负载均衡"

本文介绍了如何在Linux上安装nginx并实现在多台主机上的服务负载均衡。

<!-- excerpt -->

> 环境：Ubuntu16.04 amd64

![nginx实现负载均衡](http://upload-images.jianshu.io/upload_images/459563-18a7a55dd2f2ecec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 源码安装
1.下载解压

```
wget http://nginx.org/download/nginx-1.12.0.tar.gz
tar zxvf nginx-1.12.0.tar.gz
```
2.配置安装路径

```
cd nginx-1.12.0
./configure --prefix=/opt/nginx // 此步骤报错可能是没有相应的C编译器等，需要先安装依赖包，如果成功，则跳过步骤3
//如果需要ssl使用
./configure --with-http_stub_status_module --with-http_ssl_module  --with-http_realip_module
// 如果需要rtmp，使用
./configure --add-module=/root/nginx-rtmp-module
```

3.安装依赖包

```
sudo apt-get install libpcre3 libpcre3-dev
sudo apt-get install gcc  zlib1g-dev
可选：sudo apt-get install openssl libssl-dev
```

如果报错`E: Unable to locate package pcre-devel`可能源问题，可以更换为其他源，这里选择阿里源
在`/etc/apt/sources.list`中替换为

```
    deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse  
    ##测试版源  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse  
    # 源码  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse  
    ##测试版源  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse  
    # Canonical 合作伙伴和附加  
    deb http://archive.canonical.com/ubuntu/ xenial partner  
    deb http://extras.ubuntu.com/ubuntu/ xenial main
```
完成后重新执行步骤2的命令

4.编译

```
make
// 没有的话，先安装sudo apt-get install make
```
5.完成安装

```
sudo make install
```
#### 快速安装

```
sudo apt-get update
sudo apt-get install nginx
```

最后在nginx配置文件中实现负载均衡

```
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;
# 负载均衡
    upstream nginxBalance {
#  将同一会话定向到同一个服务器
        ip_hash;
        server 10.211.55.9:9090;
        server 10.211.55.10:9090;
    }
    server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://nginxBalance;
        }
    }
}
```


相关连接
[Linux下Nginx的安装、升级及动态添加模块](https://segmentfault.com/a/1190000006755963)

