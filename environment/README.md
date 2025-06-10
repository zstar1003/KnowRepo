# 给新机装nvidia驱动

版本号575换成适合自己的版本号
```bash
sudo apt install nvidia-cuda-toolkit
sudo apt install nvidia-container-toolkit
sudo apt install nvidia-fabricmanager-575
sudo apt install libnvidia-nscq-575
sudo systemctl start nvidia-fabricmanager
sudo systemctl enable nvidia-fabricmanager
sudo systemctl status nvidia-fabricmanager
```