# GitHub Actions 自动构建推送设置指南

## 已完成的配置

已创建 GitHub Actions 工作流文件：`.github/workflows/build-and-push.yml`

该工作流会自动：
1. 构建 Runtime 基础镜像
2. 构建 Agent 应用镜像
3. 推送到阿里云容器镜像服务

## 需要配置的步骤

### 1. 设置 GitHub Secrets

前往你的 GitHub 仓库：https://github.com/yejunhao159/ai-/settings/secrets/actions

点击 "New repository secret"，添加以下两个密钥：

| Secret 名称 | 值 | 说明 |
|------------|-----|------|
| `ALIYUN_USERNAME` | `shuhaidata` | 阿里云容器镜像服务用户名 |
| `ALIYUN_PASSWORD` | `你的密码` | 阿里云容器镜像服务密码 |

### 2. 推送代码到 GitHub

```bash
cd /home/yejh0725/小红书智能体测试/Agent

# 初始化 git（如果还没有）
git init

# 添加远程仓库
git remote add origin https://github.com/yejunhao159/ai-.git

# 添加所有文件
git add .

# 提交
git commit -m "Add GitHub Actions workflow for Docker build and push"

# 推送到 GitHub
git push -u origin main
# 如果默认分支是 master，使用：git push -u origin master
```

### 3. 触发构建

推送代码后，GitHub Actions 会自动触发构建。你可以在这里查看进度：
https://github.com/yejunhao159/ai-/actions

### 4. 手动触发（可选）

也可以在 Actions 页面手动触发：
1. 进入 https://github.com/yejunhao159/ai-/actions
2. 选择 "Build and Push Docker Image"
3. 点击 "Run workflow"

### 5. 版本标签（可选）

如果想推送特定版本，可以打 tag：

```bash
git tag v2.0.20
git push origin v2.0.20
```

这样会构建并推送 `2.0.20` 版本的镜像。

## 镜像地址

构建完成后，镜像地址为：
- 最新版本: `testacr-registry.cn-beijing.cr.aliyuncs.com/jrrc/claude-code-ui:latest`
- 指定版本: `testacr-registry.cn-beijing.cr.aliyuncs.com/jrrc/claude-code-ui:v2.0.20`

## 优势

- GitHub 服务器带宽快，推送速度比本地快得多
- 自动化构建，推送代码即可触发
- 不占用本地资源和时间
- 支持缓存，后续构建更快

## 注意事项

- 首次构建可能需要 10-20 分钟
- 后续构建因为有缓存会快很多（约 5-10 分钟）
- GitHub Actions 免费版每月有 2000 分钟限制
