# 环境安装

## 1. Ubuntu 安装ssh服务

```bash
sudo apt update
sudo apt install openssh-server
```

验证ssh服务状态

```bash
sudo systemctl status ssh
```

开放SSH端口

```bash
sudo ufw allow ssh
```

启用防火墙

```bash
sudo ufw enable
```

检查防火墙规则

```bash
sudo ufw status
```