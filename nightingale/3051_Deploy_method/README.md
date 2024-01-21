- OAMlab
- https://github.com/oamlab

# 关于《部署方法》

---

注意：以下为提供参考的安装或编译过程，具体过程可根据自有情况进行调整。

## CentOS Stream 9：

### nightingale服务端

#### 1、安装docker-compose
```
wget https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 -O /usr/bin/docker-compose
chmod +x /usr/bin/docker-compose
docker-compose --version
```

#### 2、安装nightingale服务端
````
git clone https://gitlink.org.cn/ccfos/nightingale.git
cd nightingale/docker/compose-host-network
docker-compose up -d
docker-compose ps
````

### nightingale客户端

#### 1、在需要被监控的主机内，安装nightingale客户端
- 安装
````
wget https://download.flashcat.cloud/categraf-v0.3.45-linux-amd64.tar.gz
tar -xzvf categraf-v0.3.45-linux-amd64.tar.gz
cp ./categraf/conf/categraf.service /etc/systemd/system/categraf.service

systemctl daemon-reload
systemctl enable categraf.service
systemctl restart categraf.service
systemctl status categraf.service
````

- 修改配置文件：./categraf/conf/config.toml
- 根据服务端的安装情况配置IP
````
hostname = "$ip"
url = "http://x.x.x.x:17000/prometheus/v1/write"
url = "http://x.x.x.x:17000/v1/n9e/heartbeat"
````

- 启动
````
systemctl restart categraf.service
systemctl status categraf.service
````

### 服务端与客户端的启动、停止、查看状态
```
# 服务端启动
docker-compose up -d
docker-compose ps

# 服务端停止
docker-compose down

# 客户端：启动、停止
systemctl start categraf.service
systemctl stop categraf.service
```

### 查看客户端是否已连接服务端
```
# 检查TCP连接
netstat -anopt|grep 17000
```

### 管理界面
```
# 服务端：n9e
# 安装默认端口为17000 默认账号密码为：root/root.2020
http://x.x.x.x:17000
```