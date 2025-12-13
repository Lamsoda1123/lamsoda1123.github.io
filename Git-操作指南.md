# GitHub 个人网站操作指南

## 📋 目录
- [登录校验](#登录校验)
- [推送代码](#推送代码)
- [常见问题](#常见问题)

---

## 🔐 登录校验

### 方法一：使用Personal Access Token（推荐）

#### 1. 获取GitHub Personal Access Token

1. **登录GitHub**
   - 访问 https://github.com/
   - 登录你的账户

2. **生成新Token**
   - 进入 Settings > Developer settings > Personal access tokens > Tokens (classic)
   - 点击 "Generate new token"
   - 输入密码确认身份

3. **配置Token权限**
   - Token name: `Personal Website Deploy`
   - Expiration: `90 days`（建议）
   - Select scopes: 勾选 `repo`（完整仓库访问权限）

4. **生成并保存Token**
   - 点击 "Generate token"
   - **⚠️ 重要：立即复制Token并保存！页面刷新后将无法再次查看**

#### 2. 验证登录状态

```bash
# 检查当前Git配置
git config --list | grep user

# 输出示例：
user.name=Your Name
user.email=your.email@example.com
```

---

## 📤 推送代码

### 完整推送流程

```bash
# 1. 进入项目目录
cd ~/Desktop/lamsoda1123.github.io

# 2. 查看当前状态
git status

# 3. 添加所有更改
git add .

# 4. 提交更改（添加提交信息）
git commit -m "Your commit message here"

# 5. 推送到远程仓库
git push
```

### 使用Token推送（如果SSH不可用）

```bash
# 方法1：在URL中嵌入Token
git push https://YOUR_TOKEN@github.com/Lamsoda1123/lamsoda1123.github.io.git

# 方法2：配置git凭据存储
git config --global credential.helper store
git push  # 首次需要输入用户名和Token
```

### 检查推送状态

```bash
# 检查本地与远程差异
git log --oneline origin/main..HEAD

# 如果显示本地领先远程的提交，需要推送
git push
```

---

## 🔄 SSH方式（更安全）

### 1. 配置SSH密钥

```bash
# 生成SSH密钥（如果还没有）
ssh-keygen -t ed25519 -C "your.email@example.com"

# 添加密钥到SSH代理
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 2. 添加SSH密钥到GitHub

1. 复制公钥内容：
```bash
cat ~/.ssh/id_ed25519.pub
```

2. 在GitHub中：
   - Settings > SSH and GPG keys
   - 点击 "New SSH key"
   - 粘贴公钥内容
   - 点击 "Add SSH key"

### 3. 使用SSH推送

```bash
# 切换到SSH远程仓库
git remote set-url origin git@github.com:Lamsoda1123/lamsoda1123.github.io.git

# 推送（无需输入密码）
git push
```

---

## ⚡ 快速推送命令

### 一键推送脚本

```bash
# 创建别名（一次性设置）
git config --global alias.pushall '!git add . && git commit -m "Update website" && git push'

# 以后只需运行：
git pushall
```

### 常用命令速查

| 命令 | 功能 |
|------|------|
| `git status` | 查看文件状态 |
| `git add .` | 添加所有更改 |
| `git commit -m "message"` | 提交更改 |
| `git push` | 推送到远程 |
| `git pull` | 拉取远程更新 |
| `git log --oneline` | 查看提交历史 |

---

## ❗ 常见问题

### Q1: 推送时提示 "Authentication failed"

**解决方案：**
```bash
# 方法1：重新设置远程仓库（使用Token）
git remote set-url origin https://YOUR_TOKEN@github.com/Lamsoda1123/lamsoda1123.github.io.git

# 方法2：清除缓存并重新输入凭据
git config --global --unset credential.helper
git config --global credential.helper store
```

### Q2: 远程仓库拒绝访问

**检查项：**
1. Token是否有效（未过期）
2. Token权限是否足够（需要repo权限）
3. 网络连接是否正常

### Q3: SSH连接被拒绝

**解决方案：**
```bash
# 测试SSH连接
ssh -T git@github.com

# 如果失败，重新添加密钥
ssh-add ~/.ssh/id_rsa
```

### Q4: 推送冲突

```bash
# 拉取远程更新并合并
git pull origin main --no-edit

# 如果有冲突，手动解决后
git add .
git commit -m "Resolve merge conflict"
git push
```

### Q5: 忘记提交信息

```bash
# 修改最后一次提交
git commit --amend -m "New commit message"

# 强制推送（仅用于未推送的提交）
git push --force
```

---

## 📚 补充资源

- [GitHub官方文档](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure)
- [Git官方教程](https://git-scm.com/docs)
- [GitHub Pages设置](https://pages.github.com/)

---

## 📞 获取帮助

如果遇到问题，可以：
1. 查看GitHub仓库的Actions页面了解部署状态
2. 检查GitHub Pages设置中的Source分支
3. 清除浏览器缓存后重新访问网站

**网站地址：** https://lamsoda1123.github.io/

---

*最后更新：2025年12月13日*
