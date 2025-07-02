# 计算机网络相关知识

## 1. 配置网络技巧

通过路由器固定静态ip，ssh连接路由器网关，内部端口(2025)，外部端口通过虚拟服务器(tp-link端口映射)，将外部端口(22025)映射到内部。

ssh连接：
```bash
ssh -p 22025 username@网关ip -p 22025
```

## 2. 使用 socat 做端口转发

sudo apt install socat

配置永久转发：

```bash
sudo tee /etc/systemd/system/socat.service
```

将 15100 端口转发到本地 9000
```bash
[Unit]
Description=Socat Port Forwarding
After=network.target

[Service]
ExecStart=/usr/bin/socat TCP4-LISTEN:15100,reuseaddr,fork TCP4:127.0.0.1:9000
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start socat
sudo systemctl enable socat
```