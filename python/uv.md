# uv 相关实用知识

官方文档：https://docs.astral.sh/uv/getting-started/installation/#standalone-installer


## 1. 安装
```bash
# mac或者linux
curl -LsSf https://astral.sh/uv/install.sh | sh
wget -qO- https://astral.sh/uv/install.sh | sh
# win11
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 2. 更新
```bash
uv self update
```

## 3. 卸载
```bash
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
```

## 4. 安装python
```bash
uv python install
```

## 5. 查看python
```bash
uv python list
```

## 6. 创建虚拟环境
```bash
uv venv --python 3.12
```

## 7. 安装依赖
```bash
uv pip install 包名 --index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## 8. 修改配置文件换源
编辑文件：`~/.config/uv/uv.toml`
```bash
[install]
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```

## 9. 验证镜像是否生效
```bash
uv pip install requests -v
```
## 10. 初始化项目
```bash
uv init 项目名称
```

## 11. 创建虚拟环境
```bash
uv run
uv sync  # sync 会从 pyproject.toml + uv.lock 安装
uv lock
```

## 12. 安装依赖
```bash
# 查看依赖项树
uv tree
# 直接添加，不指定版本，安装最新版本
uv add requests
# 指定版本安装
uv add 'requests==2.31.0'

# 指定版本边界添加
uv add "httpx>=0.20"

# 从github添加
uv add "httpx @ git+https://github.com/encode/httpx"

# 添加requirements.txt 中所有的依赖项
uv add -r requirements.txt -c constraints.txt

# 平台依赖添加，只能在linux平台安装
uv add "jax; sys_platform == 'linux'"

# 指定python版本添加依赖
uv add "numpy; python_version >= '3.11'"

# 从本地添加（用于无网环境）
uv add /example/foo-0.1.0-py3-none-any.whl

# 开发依赖安装
uv add --dev pytest
```

## 13. 移除依赖
```bash
uv remove requests
```

## 14. 升级依赖
```bash
uv lock --upgrade-package requests
```

## 15. 将uv.lock其导出为 requirements.txt 格式
```bash
uv export --format requirements-txt
```

## 16. 缓存设置
缓存清除
```bash
# 清除缓存目录中的所有缓存条目，将其完全清空
uv cache clean

# 移除 `ruff` 包的所有缓存条目，适用于使单个或有限数量包的缓存失效。
uv cache clean ruff

#移除所有未使用的缓存条目。
uv cache prune
```

uv中通过 --cache-dir 、 UV_CACHE_DIR 或 tool.uv.cache-dir 指定的具体缓存目录。

- linux缓存目录：`$XDG_CACHE_HOME/uv 或 $HOME/.cache/uv`
- Windows 系统： `%LOCALAPPDATA%\uv\cache`

## 17. 根据pip安装
```bash
uv pip sync docs/requirements.txt
```