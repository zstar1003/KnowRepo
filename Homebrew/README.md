# HomeBrew上传流程

## 1. 列出可用证书信息

```
security find-identity -v -p codesigning
```

## 2. 创建dmg文件

创建dmg文件，并签名公证，上传dmg文件并托管。

创建app密码

https://account.apple.com/account/manage/section/security

查看应用签名信息

```
codesign -dvvv dist/FreeTex.app
```

签名安装包

```
codesign --sign "证书信息" \
         --force \
         --timestamp \
         "dmg文件路径"
```

应用公证

```
xcrun notarytool submit
release/FreeTex-Installer-1.0.0.dmg \
    --apple-id 自己appid \
    --team-id 自己的team \
    --password 创建的密码 \
    --wait
```

装订票据

```
xcrun stapler staple release/FreeTex-Installer-1.0.0.dmg
```

## 3. 填写 Homebrew 模板

```
brew create --cask "https://github.com/zstar1003/FreeTex/releases/download/v1.0.0/FreeTex-Installer-1.0.0.dmg" --set-name FreeTex
```

示例内容

```
cask "freepdf" do
version "5.1.0"
sha256 "a4072e874854a22428070674af156a6510cc4c0f2ccd0b7075bfa852f6e06287"

url "https://github.com/zstar1003/FreePDF/releases/download/v#{version}/FreePDF_v#{version}_macOS.dmg"
name "FreePDF"
desc "Reader that supports translating PDF documents"
homepage "https://github.com/zstar1003/FreePDF"

depends_on macos: ">= :big_sur"

app "FreePDF.app"

zap trash: [
"~/Library/Application Support/FreePDF",
"~/Library/Caches/FreePDF",
]
end
```

## 4. 本地测试

临时设置环境变量

```
export HOMEBREW_NO_AUTO_UPDATE=1
export HOMEBREW_NO_INSTALL_FROM_API=1
```

安装测试
```
brew install --cask freetex
```

卸载测试
```
brew uninstall freetex
```

校验文件
```
brew audit --new --cask freetex
brew style --fix freetex
```

## 5. 合并仓库

提交PR：https://github.com/Homebrew/homebrew-cask