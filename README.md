# V2ray搭建
### 1、获取权限
```
sudo -i
```
### cenos更新指令

```
yum update -y
```

### 自动安装脚本(V2Ray 官方维护并提供了适用于大多数主流系统的自动安装脚本)

```
bash <(curl -L -s https://install.direct/go.sh)
```
### 以下来自233大神提供(一键安装脚本是第三方脚本，有可能会有一些安全风险)

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

### 启动V2ray

```
systemctl start v2ray       //然后输入端口 ID测试是否能正常使用(端口（Port）、id（UUID）)
```

# 2、手动安装TCP BBR（Bottleneck Bandwidth and Round-trip propagation time）是由Google设计，于2016年发布的拥塞算法。
### 更新系统
```
yum update -y
```
### 查看系统版本，输出的release数值大于7.3即可

```
cat /etc/redhat-release
```

### 对于某些机器来说，安装一下wget

```
yum install wget
```
### 使用下面命令安装elrepo并升级内核

```
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```
```
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```
```
yum --enablerepo=elrepo-kernel install kernel-ml -y
```

### 更新grub文件并重启（reboot后，ssh会断开，稍等一会儿重新连接）

```
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
```
```
grub2-set-default 0
```
```
reboot
```

### 开机后查看内核是否已更换为4.9(或更高版本)

```
uname -r
```

### 启动BBR

```
echo "net.core.default_qdisc = fq" >> /etc/sysctl.conf
```
```
echo "net.ipv4.tcp_congestion_control = bbr" >> /etc/sysctl.conf
```
```
sysctl -p 
```

### 验证bbr是否已经开启

#### A，验证当前TCP控制算法的命令

```
sysctl net.ipv4.tcp_available_congestion_control
```
//返回值一般为：net.ipv4.tcp_available_congestion_control = bbr cubic reno 或者为：net.ipv4.tcp_available_congestion_control = reno cubic bbr

#### B，验证BBR是否已经启动

```
sysctl net.ipv4.tcp_congestion_control
```
//返回值一般为：net.ipv4.tcp_congestion_control = bbr

```
lsmod | grep bbr
```
//返回值有 tcp_bbr 模块即说明 bbr 已启动




# 3、更改配置文件

```
vi /etc/v2ray/config.json
```

### 更改配置说明(UUD在线生成器 https://www.uuidgenerator.net/)
### 注意:V2Ray的json配置文件支持 //、/* */形式的注释，所以不需要删除注解也可以运行，当你的文本编辑器支持 json 的语法检查时可能会对注释报错，不用理会，V2Ray会正确的处理它
```
{
  "inbounds": [{
    "port": 1998,   //端口(port)
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "24cc79bb-15f3-4ef7-a689-2334fb9a7888",   //UUID
          "level": 1,
          "alterId": 64
        }
      ]
    }
  }],
//一下为ss配置
  "inboundDetour": [
    {
      "protocol": "shadowsocks",
      "port": 30001, // 监听 30001 端口
      "settings": {
        "method": "aes-256-cfb", 
        "password": "v2ray",     // 密码，必须和客户端相同
        "udp": true             // 是否开启 UDP 转发
      }
    }
  ],
//到此ss配置结束
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  },{
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

# 3、更改服务器时间和时区

### 获取 PWD=/root)权限
```
sudo -i
```
### 查看当前时区
```
date -R
```
### 修改本机时间
```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
### 修改本机时区
```
echo 'Asia/Shanghai' >/etc/timezone
```



### 4、相关命令

#### 启动
```
systemctl start v2ray
```
#### 停止
```
systemctl stop v2ray
```
#### 重启
```
systemctl restart v2ray
```
#### 开机自启
```
systemctl enable v2ray
```
#### 查看运行状态
```
systemctl status v2ray
```
//如果显示为绿色文字的active(running)，则V2Ray的配置正确





# V2ray一键搭建脚本(由233大神提供)

```
bash <(curl -s -L https://git.io/v2ray.sh)
```
### 如果提示 curl: command not found ，那是因为你的 VPS 没装 Curl
#### ubuntu/debian 系统安装 Curl 方法:
```
apt-get update -y && apt-get install curl -y
```
#### centos 系统安装 Curl 方法:
```
yum update -y && yum install curl -y
```





## 5、其它

### 5-1、CentOS7 如果不能使用可能是因为防火墙firewall问题
```
## 开放端口 比如你设置的SSR端口为2013
firewall-cmd --zone=public --add-port=2013/tcp --permanent
firewall-cmd --zone=public --add-port=2013/udp --permanent
## 重新载入
firewall-cmd --reload
## 查看已开放端口
firewall-cmd --zone=public --list-ports
```

### 5-2、测速

#### 下载测速脚本

```
wget https://shiyu.pro/file/speedtest.py
```

#### 把脚本移动进去bin文件夹方便以后直接执行

```
mv speedtest.py /bin/
```

#### 赋予执行权力

```
chmod +x /bin/speedtest.py
```

#### 然后直接输入命令开始测速

```
speedtest.py
```







## 6、以下为SSR安装libsodium库

## 安装libsodium库解决libsodium not found问题
## 0.案例
## 0.系统默认是没有 chacha20 加密方式的，需要手动编译 libsodium 1.0.8 及以上版本。安装需要以root权限安装

## 解决方案

## 0.获取root权限

```
su root
```

## 6-1. 安装依赖

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

## 6-2. 下载 libsodium 最新版本(推荐用1.0.10版本)

### 可以从libsodium 官网下，也可以从github 下载。选择速度最快的下载方式。

#### <1> 从官网下载：

```
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
```

#### <2> 从 github 下载（其中 1.0.10 是 libusodium 的版本号，可以改成最新的）：

```
wget https://github.com/jedisct1/libsodium/releases/download/1.0.10/libsodium-1.0.10.tar.gz
```

## 6-3. 解压

#### <1>官网下载的：

```
tar xzvf LATEST.tar.gz
```

#### <2>github 下载的：

```
tar xzvf libsodium-1.0.10.tar.gz
```

## 6-4. 生成配置文件

```
cd libsodium*
```

```
./configure
```

## 6-5. 编译并安装

```
make -j8 && make install
```

## 6-6. 添加运行库位置并加载运行库：

```
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
```

```
ldconfig
```
原文：https://blog.csdn.net/hanshihao1336295654/article/details/79850584