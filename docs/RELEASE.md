# 🚀 自动发布工作流使用指南

本项目配置了完整的自动发布工作流，当您创建版本标签时，系统会自动构建、打包并发布到GitHub Release和WebDAV服务器。

## 📋 工作流功能

### ✨ 主要特性
- **🏷️ 标签触发** - 推送版本标签时自动执行
- **📦 智能打包** - 生成完整版和源码版两个发布包
- **🎯 GitHub Release** - 自动创建Release并上传文件
- **🌐 WebDAV同步** - 同时上传到WebDAV服务器
- **📝 变更日志** - 自动提取CHANGELOG.md中的版本说明
- **🔗 版本管理** - 维护最新版本链接

### 📦 发布包类型
1. **完整版本** (`danmu2rcon-X.X.X-full.zip`)
   - 包含所有依赖（node_modules）
   - 下载即用，无需额外安装
   - 适合最终用户

2. **源码版本** (`danmu2rcon-X.X.X-source.zip`)
   - 仅包含源代码
   - 需要运行 `npm install`
   - 适合开发者

## 🛠️ 配置要求

### GitHub Secrets设置
在GitHub仓库的 **Settings > Secrets and variables > Actions** 中添加：

```bash
# WebDAV服务器配置（必需）
WEBDAV_URL=https://your-webdav-server.com/dav
WEBDAV_USERNAME=your_username
WEBDAV_PASSWORD=your_password
```

### 权限设置
确保GitHub Actions具有以下权限：
- **Contents**: Write（创建Release）
- **Actions**: Write（运行工作流）

## 🏷️ 发布新版本

### 1. 准备发布
在发布前确保：
- [ ] 代码已测试无误
- [ ] 更新了`CHANGELOG.md`中的版本信息
- [ ] 更新了`package.json`中的版本号（可选）

### 2. 创建版本标签
```bash
# 1. 确保在main分支
git checkout main
git pull origin main

# 2. 创建并推送标签
git tag v1.0.0
git push origin v1.0.0

# 或者一次性创建带注释的标签
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

### 3. 监控发布过程
1. 访问GitHub仓库的 **Actions** 页面
2. 查看"🚀 自动发布版本"工作流状态
3. 等待工作流完成（通常需要3-5分钟）

## 📁 WebDAV目录结构

工作流会在WebDAV服务器创建以下目录结构：

```
webdav-root/
└── danmu2rcon/
    ├── danmu2rcon-latest.zip          # 最新版本（快速下载）
    ├── latest-version.json            # 最新版本信息
    └── releases/
        ├── v1.0.0/
        │   ├── danmu2rcon-1.0.0-full.zip
        │   ├── danmu2rcon-1.0.0-source.zip
        │   └── version-info.json
        ├── v1.0.1/
        │   ├── danmu2rcon-1.0.1-full.zip
        │   ├── danmu2rcon-1.0.1-source.zip
        │   └── version-info.json
        └── ...
```

## 📝 变更日志格式

为了让工作流正确提取变更信息，请在`CHANGELOG.md`中使用以下格式：

```markdown
# 变更日志

## [1.0.1] - 2024-01-15

### 新增
- 新功能A
- 新功能B

### 修复
- 修复问题A
- 修复问题B

### 优化
- 优化性能
- 改进UI

## [1.0.0] - 2024-01-01

### 新增
- 初始版本发布
- 基础功能实现
```

## 🔧 工作流自定义

### 修改触发条件
在`.github/workflows/release.yml`中修改：

```yaml
on:
  push:
    tags:
      - 'v*.*.*'      # 标准语义版本
      - 'v*.*.*-*'    # 预发布版本
      - 'release-*'   # 自定义格式
```

### 修改打包内容
在"创建发布包"步骤中修改复制的文件：

```bash
# 添加新文件
cp new-file.txt release-temp/danmu2rcon-${{ steps.version.outputs.clean_version }}/

# 排除某些文件
rm -f release-temp/danmu2rcon-${{ steps.version.outputs.clean_version }}/unwanted-file.txt
```

### 修改WebDAV路径
修改WebDAV目录结构：

```bash
# 自定义路径
WEBDAV_DIR="custom-path/releases/${{ steps.version.outputs.version }}"
```

## 🛡️ 安全注意事项

### WebDAV认证
- **永远不要**在代码中硬编码密码
- 使用GitHub Secrets存储敏感信息
- 定期更新WebDAV密码

### 文件权限
- 确保WebDAV服务器有适当的权限控制
- 考虑使用HTTPS协议
- 定期备份发布文件

## 🐛 故障排除

### 常见问题

**Q: 工作流触发失败**
```bash
# 检查标签格式
git tag --list | grep v1.0.0

# 确保标签已推送
git ls-remote --tags origin
```

**A: 确保标签格式符合`v*.*.*`模式**

**Q: WebDAV上传失败**
- 检查WebDAV URL是否正确
- 验证用户名和密码
- 确认服务器支持WebDAV协议

**Q: Release创建失败**
- 检查GITHUB_TOKEN权限
- 确认仓库设置允许Actions创建Release

### 调试技巧

1. **查看工作流日志**
   - 访问Actions页面查看详细日志
   - 关注错误步骤的输出

2. **本地测试打包**
   ```bash
   # 模拟打包过程
   mkdir -p release-temp/test
   cp *.js *.json *.md release-temp/test/
   cd release-temp && zip -r ../test.zip test/
   ```

3. **测试WebDAV连接**
   ```bash
   # 使用curl测试连接
   curl -u "username:password" https://your-webdav-server.com/dav/
   ```

## 🎯 最佳实践

### 版本管理
- 使用[语义版本控制](https://semver.org/lang/zh-CN/)
- 主版本号.次版本号.修订号（如：1.2.3）
- 预发布版本添加后缀（如：1.2.3-beta.1）

### 发布流程
1. 在开发分支完成功能
2. 合并到main分支
3. 更新CHANGELOG.md
4. 创建并推送标签
5. 验证自动发布结果

### 质量保证
- 发布前进行充分测试
- 保持CHANGELOG.md的更新
- 定期清理旧版本（如需要）

## 📞 技术支持

如果您在使用自动发布工作流时遇到问题：

1. 📚 查看GitHub Actions日志
2. 🔍 检查本文档的故障排除部分
3. 📧 联系维护者：WittF@qq.com
4. 🐛 提交Issue：[GitHub Issues](https://github.com/WittF/danmu2rcon/issues)

---

✨ **祝您发布愉快！** 🚀 