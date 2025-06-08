# ollama 相关配置

Linux下Ollama配置文件

```bash
vim /etc/systemd/system/ollama.service
```

上下文长度设置：

```bash
Environment="OLLAMA_CONTEXT_LENGTH=4096"
```

ollama重启：

```bash
systemctl daemon-reload
systemctl restart ollama
```