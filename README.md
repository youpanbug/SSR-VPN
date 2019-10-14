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
### libsodium 一键安装教程(chacha20加密方法需要libsodium)
```
yum install -y wget && wget -N --no-check-certificate https://cloud.deng-quan.com/Microd-script/libsodium.sh && chmod +x libsodium.sh && ./libsodium.sh
或
wget https://download.libsodium.org/libsodium/releases/libsodium-1.0.18.tar.gz && tar xf libsodium-1.0.18.tar.gz && cd libsodium-1.0.18 && ./configure && make -j2 && make install && ldconfig && cd /

```