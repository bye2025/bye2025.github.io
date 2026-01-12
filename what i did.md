# 操作记录

## 完善 todo.html 文件

- 创建了一个完整的待办事项（Todo List）应用程序页面
- 包含现代化的用户界面，具有渐变色头部和响应式设计
- 实现了添加、删除和标记任务为已完成的功能
- 添加了实时统计数据（总任务数和已完成任务数）
- 实现了数据持久化存储在浏览器的 localStorage 中

## 解决 Git SSH 配置问题

### 问题描述
- SSH 身份验证失败，无法推送代码到 GitHub 仓库
- 错误信息："Permission denied (publickey)" 和 "Could not read from remote repository"

### 解决步骤
1. 重置错误的 SSH 配置：
   ```
   git config --unset core.sshCommand
   ```

2. 设置正确的 SSH 命令配置（仅针对当前仓库）：
   ```
   git config core.sshCommand 'ssh -i "A:\gitbye@outlook.com\ssh\gitbySSH\ssh1\secKYE" -o IdentitiesOnly=yes'
   ```

3. 成功推送更改到 GitHub：
   ```
   git push origin main
   ```

### 技术要点
- 该配置仅影响当前仓库，不会影响其他 Git 仓库
- 使用 `IdentitiesOnly=yes` 选项确保 SSH 只使用指定的密钥
- 通过 `-i` 参数明确指定私钥文件路径

## 在其他仓库使用相同 SSH 密钥的操作步骤

如果你在其他路径下的仓库也需要使用相同的 SSH 密钥，可以按照以下步骤进行配置：

### 1. 配置 SSH 客户端配置文件（推荐方法）

首先，在你的用户目录下编辑或创建 SSH 配置文件：

```
# Windows: %USERPROFILE%\.ssh\config
# 在 Windows 系统上，路径通常为 C:\Users\<用户名>\.ssh\config

Host github-personal
  Hostname ssh.github.com
  Port 443
  User git
  IdentityFile A:\gitbye@outlook.com\ssh\gitbySSH\ssh1\secKYE
  IdentitiesOnly yes

# 如果有其他账户，可以添加更多配置块
Host github-work
  Hostname ssh.github.com
  Port 443
  User git
  IdentityFile A:\work\ssh\keys\work_key
  IdentitiesOnly yes
```

**Windows 特殊注意事项：**
- 确保 .ssh 目录权限设置正确，否则 SSH 可能拒绝使用配置文件
- 在 Windows 上编辑配置文件，可以用记事本或其他文本编辑器
- 路径中的反斜杠有时需要转义，使用正斜杠通常更可靠
- 如果使用 Git Bash，可能需要将路径转换为 Unix 格式（例如 /a/gitbye... 而不是 A:\gitbye...）

这样配置的好处是：
- 为不同项目/账户使用不同的 SSH 密钥
- 不需要在每个仓库中单独配置
- 更易于管理和维护

### 2. 测试 SSH 连接
```bash
ssh -T git@github-personal
```

### 3. 克隆使用特定配置的仓库
```bash
git clone git@github-personal:username/repo-name.git
```

### 4. 对于已存在的仓库

如果你已经有一个克隆的仓库，可能需要更新远程 URL：
```bash
git remote set-url origin git@github-personal:username/repo-name.git
```

> 注意：使用 SSH 配置文件的方式更加系统化，可以一次性解决多个仓库的 SSH 连接问题，而不需要在每个仓库中单独配置。