# 🚀 Danmu2RCON 快速使用指南

## 📦 自动化发布流程

### 1. 准备工作

- 确保已安装 Git
- 确保有 GitHub 仓库的写入权限
- 确保在项目根目录下运行命令

### 2. 创建新版本

1. 运行版本标签创建工具：
   ```bash
   create-tag.bat
   ```

2. 按照提示输入：
   - 新版本号（格式：1.0.0）
   - 版本描述（可选）

3. 确认发布信息：
   - 标签名称
   - 版本描述
   - 当前分支
   - 创建时间

### 3. 自动发布流程

创建标签后，系统会自动执行以下步骤：

1. ⚡ 触发 GitHub Actions 构建工作流
2. 📦 自动创建发布包：
   - 完整版本（包含依赖）
   - 源码版本（仅源码）
3. 🎯 创建 GitHub Release
4. 🌐 上传到 WebDAV 服务器
5. 📧 发送发布通知

### 4. 发布包内容

完整版本包含：
- 所有核心运行文件
- 依赖包（node_modules）
- 配置文件
- 文档文件
- 启动脚本

源码版本包含：
- 所有源代码文件
- 配置文件
- 文档文件
- 启动脚本

### 5. 系统要求

- Node.js 16.0+
- Git
- Windows 系统（用于运行 .bat 脚本）

### 6. 注意事项

- 建议在 main 分支创建正式版本标签
- 确保工作区没有未提交的更改
- 发布过程通常需要 3-5 分钟
- 可以通过 GitHub Actions 页面查看构建进度

### 7. 常见问题

1. 如果创建标签失败：
   - 检查 Git 是否正确安装
   - 确认是否有仓库写入权限
   - 验证版本号格式是否正确

2. 如果发布失败：
   - 检查 GitHub Actions 日志
   - 确认 WebDAV 配置是否正确
   - 验证网络连接是否正常

### 8. 获取帮助

- 📚 详细文档：查看项目 README.md
- 🐛 问题反馈：提交 GitHub Issue
- 📧 联系作者：WittF@qq.com

---

⭐ 如果这个项目对您有帮助，请给个星标支持！ 