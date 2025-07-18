# Git 本地开发配置指南

本文档提供了 docker-elk 项目的 Git 配置和开发流程指南，帮助你进行本地开发调整和版本回退。

## 当前项目状态

- 项目已正确克隆，远程仓库指向原始项目：`https://github.com/deviantony/docker-elk.git`
- 当前在 `main` 分支，与远程保持同步
- 工作目录干净，无未提交的更改

## Git 配置调整步骤

### 1. 创建开发分支

```bash
cd /home/jason/docker/docker-elk
# 创建并切换到开发分支
git checkout -b dev
# 或者根据功能创建特定分支
git checkout -b feature/your-feature-name
```

### 2. 设置你自己的远程仓库（推荐）

如果你计划长期开发，建议 fork 原项目到你的 GitHub 账户：

```bash
# 添加你的 fork 作为新的远程仓库
git remote add myfork https://github.com/mengdahuang/docker-elk.git
# 设置推送默认到你的 fork
git push --set-upstream myfork dev
```

### 3. 保留原始仓库作为上游

```bash
# 重命名原始远程仓库为 upstream
git remote rename origin upstream
# 添加你的 fork 为 origin
git remote add origin https://github.com/mengdahuang/docker-elk.git
```

## 日常开发工作流

### 开发新功能

```bash
# 从最新的 main 分支创建功能分支
git checkout main
git pull upstream main
git checkout -b feature/new-feature
# 进行开发...
git add .
git commit -m "feat: add new feature"
git push origin feature/new-feature
```

### 同步上游更新

```bash
# 定期同步原项目的更新
git checkout main
git pull upstream main
git push origin main
```

## 回退和版本管理

### 软回退（保留更改）

```bash
# 回退到上一个提交，保留工作目录的更改
git reset --soft HEAD~1
```

### 硬回退（丢弃更改）

```bash
# 完全回退到指定提交
git reset --hard <commit-hash>
# 例如：git reset --hard c8e4e16
```

### 创建标签用于版本管理

```bash
# 为重要版本创建标签
git tag -a v1.0.0 -m "My custom version 1.0.0"
git push origin v1.0.0
```

### 使用 stash 临时保存更改

```bash
# 临时保存当前更改
git stash push -m "临时保存的更改"
# 恢复保存的更改
git stash pop
```

## 推荐的分支策略

- `main`: 保持与上游同步的稳定分支
- `dev`: 你的主要开发分支
- `feature/*`: 具体功能开发分支
- `hotfix/*`: 紧急修复分支

## 常用 Git 命令速查

```bash
# 查看远程仓库
git remote -v

# 查看分支状态
git status
git branch -a

# 查看提交历史
git log --oneline -10

# 查看文件差异
git diff
git diff --staged

# 撤销文件更改
git checkout -- <file>

# 撤销暂存区文件
git reset HEAD <file>

# 修改最后一次提交
git commit --amend

# 查看某个提交的详细信息
git show <commit-hash>
```

## 注意事项

1. 在进行任何重要操作前，建议先备份当前工作
2. 使用 `git status` 经常检查工作目录状态
3. 提交信息要清晰明确，遵循约定式提交规范
4. 定期同步上游仓库的更新
5. 重要的开发节点要打标签进行标记

这样的配置可以让你既能跟踪原项目的更新，又能安全地进行自己的开发和实验。