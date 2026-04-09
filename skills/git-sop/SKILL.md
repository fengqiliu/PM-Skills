---
name: git-sop
description: Git 标准操作流程 SOP，包含 init、add、commit、push、status、log、pull 等常用操作及自动检查验证。使用场景：用户要求执行 git 操作时。
---

# Git SOP - 标准操作流程

执行任何 Git 操作前，遵循此 SOP。**自动检查是强制步骤**，每个操作后必须验证执行结果。

## 核心原则

1. **先检查，后操作** - 使用 `git status` 和 `git log` 了解当前状态
2. **小步提交** - 每个提交聚焦单一变更，便于追溯和回滚
3. **操作后验证** - 每个 Git 命令执行后必须运行验证命令确认结果
4. **危险操作需确认** - push --force、reset --hard、branch -D 等破坏性操作需用户确认

---

## SOP 流程

### SOP 1: git init

初始化新仓库。

**执行命令:**
```bash
git init
```

**自动检查:**
```bash
ls -la .git && git status
```

**验证标准:**
- `.git` 目录存在且包含必要文件（HEAD, config, objects, refs）
- 当前分支为 `main` 或 `master`
- 工作区状态为 "No commits yet"

---

### SOP 2: git add

暂存文件变更。

**执行命令:**
```bash
# 暂存所有变更
git add -A

# 暂存特定文件
git add <file>

# 暂存特定目录
git add <directory>/

# 交互式暂存（推荐用于精细控制）
git add -p
```

**自动检查:**
```bash
git status
```

**验证标准:**
- 暂存区显示待提交的文件（绿色）
- 未暂存文件清晰列出（红色）
- 确认没有意外文件被暂存（如 .env, node_modules, *.log）

---

### SOP 3: git commit

提交暂存区变更。

**执行命令:**
```bash
git commit -m "<type>: <subject>

<body>

<footer>"
```

**type 规范:**
| type | 说明 |
|------|------|
| feat | 新功能 |
| fix | 修复 bug |
| docs | 文档更新 |
| style | 代码格式（不影响功能）|
| refactor | 重构（不影响功能）|
| test | 测试相关 |
| chore | 构建/工具变动 |

**自动检查:**
```bash
git log --oneline -1 && git status
```

**验证标准:**
- commit 已创建，hash 可查看
- 工作区状态为 "nothing to commit, working tree clean"
- commit message 格式正确

---

### SOP 4: git push

推送提交到远程仓库。

**执行命令:**
```bash
# 推送当前分支到默认远程仓库
git push

# 推送并设置上游分支
git push -u origin <branch>

# 强制推送（需确认）
git push --force-with-lease
```

**⚠️ 危险操作警告:**
- `--force` 会覆盖远程历史，**必须先确认**无其他人在该分支工作
- 推荐使用 `--force-with-lease` 而非 `--force`

**自动检查:**
```bash
git status && git log --oneline -3
```

**验证标准:**
- push 成功，无错误
- 本地分支已跟踪远程分支
- 远程仓库可查看到新提交

---

### SOP 5: git status

查看工作区状态。

**执行命令:**
```bash
git status
```

**自动检查:**
```bash
git status --short
```

**验证标准:**
- 清晰显示：当前分支、暂存区文件、未跟踪文件
- 无意外的大文件或敏感文件

---

### SOP 6: git log

查看提交历史。

**执行命令:**
```bash
# 查看最近 10 条提交
git log --oneline -10

# 查看详细提交信息
git log --pretty=format:"%h %an %s" -10

# 查看特定文件的变更历史
git log --oneline -- <file>

# 查看分支合并历史
git log --graph --oneline --all -15
```

**自动检查:**
```bash
git log --oneline -3
```

**验证标准:**
- 确认最新提交是你刚做的
- commit message 清晰可读

---

### SOP 7: git pull

拉取远程变更并合并。

**执行命令:**
```bash
# 标准拉取
git pull

# 拉取并变基（保持线性历史）
git pull --rebase

# 拉取特定分支
git pull origin <branch>
```

**⚠️ 潜在冲突警告:**
- 如果有冲突，Git 会暂停并标记冲突文件
- 冲突解决后需要 `git add` 和 `git rebase --continue`

**自动检查:**
```bash
git status && git log --oneline -3
```

**验证标准:**
- 无冲突提示
- 工作区已更新
- 提交历史包含远程的新提交

---

## 完整工作流示例

### 场景: 提交一个 bug 修复

```
1. 检查当前状态
   git status && git log --oneline -3

2. 修复代码...

3. 暂存变更
   git add -A
   git status  # 验证暂存内容

4. 提交变更
   git commit -m "fix: resolve null pointer in user service

   - add null check before accessing user profile
   - add unit test for edge case"

5. 推送提交
   git push
   git status  # 验证推送成功
```

---

## 常见问题处理

### 问题: 提交信息写错

```bash
# 未 push，可修改上次提交
git commit --amend -m "correct message"

# 已 push，需谨慎处理
git commit --amend -m "correct message"
git push --force-with-lease
```

### 问题: 暂存了不想提交的文件

```bash
# 取消暂存单个文件
git reset HEAD <file>

# 取消暂存所有文件
git reset HEAD
```

### 问题: 工作区有未提交的变更但需要切换分支

```bash
# 方法1: 暂存变更
git stash
git checkout <other-branch>
# ... 工作完成后
git stash pop

# 方法2: 强制切换（会丢失变更）
git checkout -f <other-branch>
```

### 问题: 推送被拒绝（远程有更新）

```bash
# 方法1: 变基后推送
git pull --rebase
git push

# 方法2: 手动合并
git pull
# 解决冲突后
git add -A
git commit
git push
```

---

## 自动检查清单

每个 Git 操作后必须确认：

- [ ] 命令执行成功（无错误输出）
- [ ] `git status` 显示预期状态
- [ ] 没有意外的文件变更
- [ ] 操作结果符合预期
