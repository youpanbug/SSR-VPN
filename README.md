# SSR搭建步骤命令
### cenos7更新指令

```
yum update -y
```

### ssr脚本

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh
```

### (提示:如出现未安装wget,执行以下指令)

```
yum install wget
```

### 使用说明

```
#运行脚本
bash ssrmu.sh
```
### 文件位置

```
安装目录：/usr/local/shadowsocksr

配置文件：/usr/local/shadowsocksr/user-config.json

数据文件：/usr/local/shadowsocksr/mudb.json
```

### 脚本控制

```
启动 ShadowsocksR：/etc/init.d/ssrmu start

停止 ShadowsocksR：/etc/init.d/ssrmu stop

重启 ShadowsocksR：/etc/init.d/ssrmu restart

查看 ShadowsocksR状态：/etc/init.d/ssrmu status

ShadowsocksR 默认支持UDP转发，服务端无需任何设置。
```

### 其它

#### CentOS7 如果不能使用可能是因为防火墙firewall问题
```
#开放端口 比如你设置的SSR端口为2013
firewall-cmd --zone=public --add-port=2013/tcp --permanent
firewall-cmd --zone=public --add-port=2013/udp --permanent
#重新载入
firewall-cmd --reload
```

### 锐速bbr脚本(建议选择BBRplus)

```
wget "https://github.com/chiakge/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```



# 特别说明

## 安装libsodium库解决libsodium not found问题
## 0.案例
## 0.系统默认是没有 chacha20 加密方式的，需要手动编译 libsodium 1.0.8 及以上版本。安装需要以root权限安装

## 解决方案

## 0.获取root权限

```
su root
```

## 1. 安装依赖

### Debian 7/8、Ubuntu 14/15/16 及其衍生系列：

```
sudo apt-get update
sudo apt-get install build-essential wget -y
```

### Centos 6/7、RHEL 7 及其衍生系列：

```
yum groupinstall "Development Tools" -y
yum install wget -y
```

## 2. 下载 libsodium 最新版本(推荐用1.0.10版本)

### 可以从libsodium 官网下，也可以从github 下载。选择速度最快的下载方式。

#### <1> 从官网下载：

```
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
```

#### <2> 从 github 下载（其中 1.0.10 是 libusodium 的版本号，可以改成最新的）：

```
wget https://github.com/jedisct1/libsodium/releases/download/1.0.10/libsodium-1.0.10.tar.gz
```

## 3. 解压

#### <1>官网下载的：

```
tar xzvf LATEST.tar.gz
```

#### <2>github 下载的：

```
tar xzvf libsodium-1.0.10.tar.gz
```

## 4. 生成配置文件

```
cd libsodium*
```

```
./configure
```

## 5. 编译并安装

```
make -j8 && make install
```

## 6. 添加运行库位置并加载运行库：

```
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
```

```
ldconfig
```
原文：https://blog.csdn.net/hanshihao1336295654/article/details/79850584