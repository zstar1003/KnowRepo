# ollama 相关配置

Linux下Ollama配置文件

```bash
vim /etc/systemd/system/ollama.service
```

上下文长度设置：

```bash
Environment="OLLAMA_CONTEXT_LENGTH=4096"
```

将 Ollama 的 OLLAMA_HOST 设置为对所有网络接口开放

```bash
[Service]
Environment="OLLAMA_HOST=0.0.0.0"
```



ollama重启：

```bash
systemctl daemon-reload
systemctl restart ollama
```

