GitHub 有文件大小限制，上传大文件需要使用 Git LFS（Large File Storage）。以下是完整方案：

## 方案1：使用 Git LFS（推荐）

### 1. 安装 Git LFS

```bash
# Windows (使用 Chocolatey)
choco install git-lfs

# 或者下载安装包
# 访问：https://git-lfs.github.com/

# 验证安装
git lfs version
```

### 2. 在仓库中启用 Git LFS

```bash
# 进入项目目录
cd your-repo

# 初始化 LFS
git lfs install

# 跟踪大文件类型（例如跟踪所有 .psd 文件）
git lfs track "*.psd"

# 或者跟踪特定文件
git lfs track "data/large-file.zip"

# 查看跟踪规则
cat .gitattributes
```

### 3. 正常提交和推送

```bash
# 添加文件
git add .gitattributes
git add large-file.psd

# 提交
git commit -m "Add large design file"

# 推送（LFS 文件会自动处理）
git push origin main
```

## 方案2：使用 GitHub Releases（适合超大文件）

### 1. 创建 Release 上传大文件

1. 在 GitHub 仓库页面点击 **Releases**
2. 点击 **Draft a new release**
3. 填写版本信息
4. 将大文件拖拽到附件区域
5. 发布 Release

### 2. 在项目中引用 Release 文件

在 README 中添加下载链接：
```markdown
## 下载大文件
https://github.com/username/repo/releases/download/v1.0/large-file.zip
```

## 方案3：分割大文件

### 使用 split 命令分割文件
```bash
# 分割文件（每个部分 100MB）
split -b 100m large-file.zip large-file-part-

# 合并文件（下载后）
cat large-file-part-* > large-file.zip
```

## 方案4：使用外部存储服务

### 1. 云存储服务
- **Google Drive**、**Dropbox**、**OneDrive**
- **AWS S3**、**Google Cloud Storage**
- 在项目中添加下载链接

### 2. 专业大文件托管
- **Git LFS**（GitHub 官方）
- **Git Annex**（替代方案）
- **SourceForge**（传统选择）

## Git LFS 详细使用指南

### 常用 LFS 命令
```bash
# 查看 LFS 状态
git lfs status

# 查看跟踪的文件
git lfs ls-files

# 迁移现有大文件到 LFS
git lfs migrate import --include="*.psd,*.zip"

# 查看 LFS 存储使用情况
git lfs env
```

### 跟踪常见文件类型
```bash
# 设计文件
git lfs track "*.psd"
git lfs track "*.ai"
git lfs track "*.sketch"

# 媒体文件
git lfs track "*.mp4"
git lfs track "*.mov"
git lfs track "*.avi"
git lfs track "*.wav"

# 数据文件
git lfs track "*.zip"
git lfs track "*.rar"
git lfs track "*.7z"
git lfs track "*.db"
git lfs track "*.sqlite"

# 文档
git lfs track "*.pdf"
git lfs track "*.epub"
```

## GitHub 文件大小限制

| 类型 | 限制 | 说明 |
|------|------|------|
| 普通文件 | 100 MB | 直接推送的最大文件大小 |
| Git LFS | 2 GB | 单个 LFS 文件大小限制 |
| 仓库总大小 | 推荐 1-5 GB | 无硬性限制，但性能考虑 |
| Releases 附件 | 2 GB | 每个附件大小限制 |

## 实际工作流程示例

### 情况1：已有大文件在仓库中
```bash
# 1. 安装 LFS
git lfs install

# 2. 跟踪大文件类型
git lfs track "*.zip"

# 3. 从 Git 历史中移除大文件并迁移到 LFS
git lfs migrate import --everything --include="*.zip"

# 4. 强制推送更新
git push origin main --force
```

### 情况2：添加新的大文件
```bash
# 1. 设置 LFS 跟踪
git lfs track "*.mp4"

# 2. 添加文件
git add presentation.mp4
git add .gitattributes

# 3. 提交和推送
git commit -m "Add presentation video"
git push origin main
```

## 故障排除

### 推送时出现大小错误
```bash
# 如果文件超过 100MB，需要先设置 LFS
git lfs track "large-file.exe"
git add .gitattributes
git add large-file.exe
git commit -m "Add large file via LFS"
git push origin main
```

### 检查大文件
```bash
# 查找仓库中的大文件
git ls-tree -r -l HEAD | sort -nr -k4 | head -10

# 或者使用 BFG Repo-Cleaner 清理历史大文件
java -jar bfg.jar --strip-blobs-bigger-than 100M your-repo.git
```

## 最佳实践

1. **尽早设置 LFS**：在添加大文件前设置 LFS 跟踪规则
2. **使用 .gitignore**：忽略临时文件和生成的大文件
3. **定期清理**：使用 `git gc` 优化仓库
4. **文档说明**：在 README 中说明大文件的下载方式

选择最适合你需求的方案，Git LFS 是最常用和推荐的解决方案。



可行的命令

# 查看 LFS 状态
git lfs status

# 查看跟踪的文件
git lfs ls-files

# 迁移现有大文件到 LFS
git lfs migrate import --include="*.psd,*.zip"

# 查看 LFS 存储使用情况
git lfs env



