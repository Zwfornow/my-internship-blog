# Git 高级使用技巧

## 工作流管理

### 功能分支工作流
```bash
# 创建功能分支
git checkout -b feature/user-authentication

# 开发完成后提交
git add .
git commit -m "feat: 实现用户登录功能"

# 推送到远程
git push origin feature/user-authentication

# 创建 Pull Request 合并到 develop
```

### 提交信息规范
```
feat: 新功能
fix: 修复bug
docs: 文档更新
style: 代码格式调整
refactor: 重构代码
test: 测试相关
chore: 构建过程或辅助工具变动
```

## 实用命令

### 撤销更改
```bash
# 撤销工作区修改
git checkout -- <file>

# 撤销暂存区文件
git reset HEAD <file>

# 撤销最近一次提交（保留更改）
git reset --soft HEAD~1

# 完全撤销提交
git reset --hard HEAD~1
```

### 分支管理
```bash
# 查看分支关系图
git log --oneline --graph --all

# 清理已合并的分支
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# 重命名分支
git branch -m old-name new-name
```

## 高级技巧

### 储藏更改
```bash
# 临时保存工作区
git stash push -m "正在开发登录功能"

# 查看储藏列表
git stash list

# 恢复储藏
git stash pop

# 应用特定储藏
git stash apply stash@{1}
```

### 子模块管理
```bash
# 添加子模块
git submodule add https://github.com/user/repo.git

# 更新子模块
git submodule update --init --recursive

# 拉取所有子模块更新
git submodule foreach git pull origin main
```

> **⚠️ 注意**：操作危险命令前，确保重要代码已提交或备份！
