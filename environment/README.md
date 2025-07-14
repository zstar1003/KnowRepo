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

新版 ssh 将 ssh 分成 ssh.service 和 ssh.socket 两个服务，修改端口需同步修改。


## 2. 新机 Ubuntu 24.04 安装 cuda 和 cudnn，适配4090显卡

安装cuda
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pin
sudo mv cuda-ubuntu2404.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.9.1/local_installers/cuda-repo-ubuntu2404-12-9-local_12.9.1-575.57.08-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2404-12-9-local_12.9.1-575.57.08-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2404-12-9-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-9
```

安装cudnn
```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.10.2/local_installers/cudnn-local-repo-ubuntu2404-9.10.2_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2404-9.10.2_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2404-9.10.2/cudnn-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cudnn
```

## 3. 给用户授权文件夹

```bash
sudo chown -R zxy:zxy ./DeepSeek-R1-0528
sudo chmod -R 755 ./DeepSeek-R1-0528 
```

## 4. 批量杀进程

```bash
pkill -f "LLaMA-Factory/.venv/bin/python3"
```

## 5. 在 Linux 上安装 Docker 的步骤

参考官方文档：https://docs.docker.com/engine/install/ubuntu

1. 设置 Docker 的apt存储库。

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. 安装 Docker 包。

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. 验证 Docker 安装：
```bash
docker --version
```

## 6. 在 Linux 上 Docker 换源

1. 编辑 Docker 的配置文件

```bash
sudo mkdir -p /etc/docker
sudo vim /etc/docker/daemon.json
```

2. 配置镜像源

```bash
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://docker.imgdb.de",
        "https://docker-0.unsee.tech",
        "https://docker.hlmirror.com",
        "https://docker.1ms.run",
        "https://func.ink",
        "https://lispy.org",
        "https://docker.xiaogenban1993.com"
    ]
}
```

3. 重启 Docker 服务

```bash
sudo systemctl daemon-reexec
sudo systemctl restart docker
```

## 7. 在 Linux 上 使用 yycd

启动clash：
```bash
# ~/clash
./clash -d .
```

开启端口转发 9999 -> 9090 

访问：http://localhost:9999/ui