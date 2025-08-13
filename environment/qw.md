# qw 环境配置

## 开发环境

1. 前端

cd frontend
pnpm dev

2. 后端

cd frontend
pnpm dev

cd backend
.venv\Scripts\activate
(linux) source .venv/bin/activate

环境准备
python manage.py makemigrations
python manage.py migrate

python manage.py runserver

## 生产环境

1. 下载 postgresql

sudo apt install postgresql
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

2. 下载 clash

wget https://github.com/DustinWin/proxy-tools/releases/download/Clash-Premium/clashpremium-release-linux-amd64.tar.gz
tar zxvf  clashpremium-release-linux-amd64.tar.gz
mv CrashCore clash
wget -O config.yaml "自己的配置文件链接"
chmod +x clash
启动：./clash -d .
永久启动：nohup ./clash -d . &
永久配置代理：

vim ~/.bashrc
末尾添加 
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
source ~/.bashrc

3. 升级node版本


```c
# 安装 nvm
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
source ~/.bashrc  # 或者 source ~/.zshrc

# 安装 Node.js LTS
nvm install --lts

# 切换到新版本
nvm use --lts

# 设置默认版本
nvm alias default 'lts/*'
```

4. github代码拉取

配置ssh
ssh-keygen -t rsa -C "1198768105@qq.com"
cat /home/ubuntu/.ssh/id_rsa.pub

github上进行配置

git clone git@github.com:zstar1003/qiwu-write.git


5. 安装uv

sudo snap install astral-uv
uv sync

uv 配置镜像源


vim ~/.bashrc

末尾添加 
export UV_INDEX_URL=https://pypi.tuna.tsinghua.edu.cn/simple
export UV_EXTRA_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/

source ~/.bashrc

6. 数据库设置

```cmd
sudo -u postgres psql
```
输入postgres用户的密码，然后执行：
```sql
-- 创建数据库
CREATE DATABASE qiwu_write;

-- 创建用户（可选，也可以直接使用postgres用户）
CREATE USER qiwu_user WITH PASSWORD 'qiwu';

-- 授权
GRANT ALL PRIVILEGES ON DATABASE qiwu_write TO qiwu_user;

-- 退出
\q
```

7. 前端文件打包

修改.env文件

VITE_API_BASE_URL=http://公网ip/api

前端文件
pnpm run build 

得到服务端路径：/home/ubuntu/qiwu-write/frontend/dist/index.html


相关文件授权

sudo chown -R www-data:www-data /home/ubuntu/qiwu-write/frontend/dist
sudo chmod -R 755 /home/ubuntu/qiwu-write/frontend/dist
sudo chmod +x /home/ubuntu /home/ubuntu/qiwu-write /home/ubuntu/qiwu-write/frontend

后端文件
修改.env文件

长期启动：

nohup python manage.py runserver 0.0.0.0:8000 > django.log 2>&1 &

8. 安装 nginx

udo apt install nginx -y

sudo systemctl start nginx
sudo systemctl enable nginx


编辑 Nginx 的配置文件 /etc/nginx/nginx.conf

```c
server {
    listen 80;
    server_name 公网ip;

    root /home/ubuntu/qiwu-write/frontend/dist;
    index index.html;

    location / {
        try_files $uri $uri/ =404; 
        error_page 404 /index.html; 
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8000/api/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

重启 Nginx

sudo nginx -t   # 测试配置语法
sudo systemctl restart nginx  # 重启生效