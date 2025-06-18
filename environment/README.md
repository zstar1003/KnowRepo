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

## 3. 给文档授权

批量杀进程

```bash
pkill -f "LLaMA-Factory/.venv/bin/python3"
```

