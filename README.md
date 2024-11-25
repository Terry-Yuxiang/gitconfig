# Git Config

# 在 macOS 上生成多个 RSA 密钥并配置 GitHub

## 1. 生成多个 SSH 密钥
### 第一个账号
1. 打开终端。
2. 运行以下命令生成第一个密钥：
   ```bash
   ssh-keygen -t rsa -b 4096 -C "email_for_first_account@example.com"
   ```
3. 保存密钥为默认路径（`~/.ssh/id_rsa`）：
   ```bash
   Enter file in which to save the key (/Users/your_user/.ssh/id_rsa): [直接按回车]
   ```
4. 输入一个密码（可选），或直接回车跳过。

### 第二个账号
1. 运行以下命令生成第二个密钥：
   ```bash
   ssh-keygen -t rsa -b 4096 -C "email_for_second_account@example.com"
   ```
2. 保存密钥到不同的路径：
   ```bash
   Enter file in which to save the key (/Users/your_user/.ssh/id_rsa): /Users/your_user/.ssh/id_rsa_second
   ```
3. 输入一个密码（可选），或直接回车跳过。

---

## 2. 添加 SSH 密钥到 SSH Agent
### 启动 SSH Agent
运行以下命令：
```bash
eval "$(ssh-agent -s)"
```

### 添加第一个密钥
运行以下命令将第一个密钥添加到 SSH Agent：
```bash
ssh-add ~/.ssh/id_rsa
```

### 添加第二个密钥
运行以下命令将第二个密钥添加到 SSH Agent：
```bash
ssh-add ~/.ssh/id_rsa_second
```

---

## 3. 配置 `~/.ssh/config` 文件
编辑或创建 SSH 配置文件：
```bash
nano ~/.ssh/config
```

添加以下内容：

```plaintext
# 配置第一个账号
Host github-first
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa

# 配置第二个账号
Host github-second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_second
```

保存并退出。

---

## 4. 添加 SSH 公钥到 GitHub
### 提取公钥
分别提取两个密钥的公钥：
```bash
cat ~/.ssh/id_rsa.pub
cat ~/.ssh/id_rsa_second.pub
```

### 添加到 GitHub
1. 登录到对应的 GitHub 账号。
2. 打开 **Settings -> SSH and GPG keys**。
3. 点击 **New SSH Key**，添加公钥。
4. 对于第一个密钥：
   - Title: `GitHub First Key`
   - Key: 粘贴 `id_rsa.pub` 的内容。
5. 对于第二个密钥：
   - Title: `GitHub Second Key`
   - Key: 粘贴 `id_rsa_second.pub` 的内容。

---

## 5. 设置远程仓库 URL
### 使用第一个账号
进入项目目录，设置远程仓库的 URL：
```bash
git remote set-url origin git@github-first:username/repo.git
```

### 使用第二个账号
进入另一个项目目录，设置远程仓库的 URL：
```bash
git remote set-url origin git@github-second:username/repo.git
```

---

## 6. 测试 SSH 连接
分别测试两个账号的连接：
```bash
ssh -T github-first
ssh -T github-second
```

如果配置正确，你会看到类似以下的信息：
```plaintext
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
