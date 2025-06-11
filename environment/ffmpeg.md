# FFmpeg 常见命令

## 1. 将 mp4 文件 转成fps=1 的 mp4 文件

```bash
ffmpeg -i input.mp4 -vf fps=1 -c:v libx264 -r 1 output.mp4
```

参数说明：
    -i input.mp4：指定输入文件。
    -vf fps=1：通过视频滤镜（-vf）强制输出帧率为每秒1帧（fps=1），确保仅保留每秒的第一帧。
    -c:v libx264：使用H.264编码器（兼容性最佳），避免默认编码可能导致的兼容性问题。
    -r 1：显式设置输出视频的帧率为1fps，与fps=1滤镜配合使用，确保输出文件元数据正确标记帧率。
    output.mp4：输出文件名。

## 2. 将 mp4 文件 转成fps=1 的 mkv 文件

```bash
ffmpeg -i input.mp4 -vf fps=1 -c:v libx264 -r 1 output.mkv
```
    
## 3. 将 mp4 文件 转成fps=1 的 avi 文件

```bash
ffmpeg -i input.mp4 -vf fps=1 -c:v mpeg4 -r 1 output.avi
```
