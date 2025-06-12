# Docker相关命令

## 1. docker镜像源配置

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

## 2. 导出单个镜像到一个文件
```bash
docker save -o ragflowplus.tar zstar1003/ragflowplus:v0.4.2
docker save -o ragflowplus-management-web.tar zstar1003/ragflowplus-management-web:v0.4.2
docker save -o ragflowplus-management-server.tar zstar1003/ragflowplus-management-server:v0.4.2
```



## 3. 导出多个镜像到一个文件
```bash
docker save -o multiple_images.tar zstar1003/ragflowplus:v0.4.2 zstar1003/ragflowplus-management-web:v0.4.2 zstar1003/ragflowplus-management-server:v0.4.2
```

## 4. 加载镜像
```bash
docker load -i ragflowplus.tar
docker load -i ragflowplus-management-web.tar
docker load -i ragflowplus-management-server.tar
```