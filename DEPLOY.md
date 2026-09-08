# 赣东北 2026 国庆 · 部署到 Cloudflare Pages

整个部署流程**只用一次**,之后你只需在本地修改 `index.html`,然后 `git push` 就会自动重新发布。

> **网址(部署后):** `https://scz-chutaoqihua.pages.dev`
> 也可以后续绑定自己的域名,本指南只讲默认 pages.dev 子域。

---

## 总览(6 步)

| 步骤 | 在哪里做 | 难度 | 用时 |
|---|---|---|---|
| 1. 注册 GitHub 账号 | 电脑浏览器 | 简单 | 2 分钟 |
| 2. 注册 Cloudflare 账号 | 电脑浏览器 | 简单 | 2 分钟 |
| 3. 在 GitHub 创建新仓库 | 电脑浏览器 | 简单 | 1 分钟 |
| 4. 本地安装 git(若没有) | 电脑终端 | 简单 | 3 分钟 |
| 5. 推送代码到 GitHub | 电脑终端 | 中等 | 3 分钟 |
| 6. Cloudflare Pages 绑定仓库 | 电脑浏览器 | 简单 | 2 分钟 |

> **必须在电脑上做的:** 第 1–6 步全部。手机做不了(需要命令行和代码提交)。
> 之后每次更新网页,**只需要在电脑终端 `git push` 一行命令**。

---

## 第 1 步:注册 GitHub

1. 打开 https://github.com/signup
2. 用邮箱注册,验证邮箱
3. 选择 Free 计划(永久免费)
4. 记住你的 **GitHub 用户名**(以后命令行会用到)

## 第 2 步:注册 Cloudflare

1. 打开 https://dash.cloudflare.com/sign-up
2. 用邮箱注册,验证邮箱
3. 进入 Dashboard 即可,不需要绑定域名

## 第 3 步:在 GitHub 创建仓库

1. 登录 GitHub,点右上角 `+` → **New repository**
2. 填写:
   - **Repository name:** `gandongbei-2026` (或 `scz-chutaoqihua`,这个名字只影响 GitHub 仓库 URL,不影响 Cloudflare Pages 的网址)
   - **Description:** 留空或写"赣东北 2026 国庆行程手册"
   - **Public** ✅(必须是 Public,Cloudflare Pages 免费版只支持 Public 仓库)
   - **不要勾选** Add a README file / .gitignore / license(我们本地已有文件)
3. 点 **Create repository**
4. 记下页面给出的仓库地址,形如:
   ```
   https://github.com/你的用户名/gandongbei-2026.git
   ```

## 第 4 步:本地安装 git

打开 PowerShell 或 Git Bash,执行:

```bash
git --version
```

- 如果显示 `git version 2.x.x` → 跳过这步
- 如果没装:去 https://git-scm.com/download/win 下载安装,**安装时一路 Next 即可**(默认选项会用 VS Code / Notepad++,但你不用)

## 第 5 步:推送代码到 GitHub

**打开 PowerShell 或 Git Bash**(我用 `bash` 语法,PowerShell 也兼容),**逐行执行**:

```bash
# 1. 进入项目目录
cd "D:\Desktop\1\gandongbei-2026"

# 2. 初始化 git 仓库(本目录已经创建)
git init

# 3. 配置你的 GitHub 用户信息(只第一次需要)
git config user.name "你的GitHub用户名"
git config user.email "你的GitHub注册邮箱"

# 4. 添加所有文件
git add .

# 5. 提交
git commit -m "init: 赣东北 2026 国庆行程手册"

# 6. 改名为 main 分支(GitHub 新版默认分支是 main)
git branch -M main

# 7. 关联远程仓库(替换 你的GitHub用户名)
git remote add origin https://github.com/你的GitHub用户名/gandongbei-2026.git

# 8. 推送到 GitHub
git push -u origin main
```

**第一次 push 时会弹窗:**
- 弹窗让你登录 GitHub,选 **Sign in with your browser**
- 浏览器会打开,点 **Authorize git-ecosystem**
- 回到终端,看到 `Writing objects: 100%` 即成功

如果不想每次都登录,可以把认证改为 **Personal Access Token**:
1. GitHub 右上角头像 → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token,勾选 `repo` 全部,生成后**复制保存**(只显示一次)
3. 推送时用户名输入 token,密码留空(或用 `git remote set-url origin https://<token>@github.com/你的用户名/gandongbei-2026.git` 改写)

## 第 6 步:Cloudflare Pages 绑定 GitHub

1. 打开 https://dash.cloudflare.com/
2. 左侧菜单 → **Workers 和 Pages** → **Pages** → 点 **Connect to Git**
3. 选择 **GitHub**,会弹窗授权
4. 选 **Only select repositories** → 勾选 `gandongbei-2026` → Install & Authorize
5. 回到 Cloudflare,选 `gandongbei-2026` 仓库 → **Begin setup**
6. 配置:
   - **Project name(项目名):** `scz-chutaoqihua` ← **这里才是你最终网址的名字**
   - **Production branch:** `main`
   - **Framework preset:** 选 **None**(纯静态)
   - **Build command:** **留空**
   - **Build output directory:** 留空或填 `/`
7. 点 **Save and Deploy**
8. 等待 1-2 分钟,看到 ✅ "Success!" → 点项目名 → 顶部会显示你的网址:

   ```
   https://scz-chutaoqihua.pages.dev
   ```

   **🎉 部署完成!**

---

## 之后怎么更新(你日常要做的)

每次给我新的高铁/酒店/门票截图,我会改 `index.html`,然后你只需:

```bash
cd "D:\Desktop\1\gandongbei-2026"
git add .
git commit -m "更新:补充高铁信息"
git push
```

30 秒后 Cloudflare 自动部署,刷新 `https://scz-chutaoqihua.pages.dev` 即可看到新版本。

---

## 常见问题

**Q1: 网址一定要叫 scz-chutaoqihua 吗?**
A: Cloudflare Pages 项目名决定子域。`scz-chutaoqihua` 共 16 字符,符合 Cloudflare 限制(≤22)。但 GitHub 仓库名随便取,不影响网址。

**Q2: 推送时报错 "Updates were rejected"?**
A: 你在 GitHub 网页上勾了 README,但本地没有。解决:
```bash
git pull origin main --rebase
git push
```

**Q3: 想绑定自己的域名?**
A: Cloudflare Pages → Custom domains → 添加,按提示在域名 DNS 加 CNAME 记录。全程 5 分钟。

**Q4: 怎么把已经存在 ~/10.1-10.5赣东北行程计划.xlsx 的更新同步过来?**
A: 发截图给我,或者把 xlsx 也放进 `D:\Desktop\1\gandongbei-2026\`,告诉我"读这个 xlsx",我会自动提取。

**Q5: 我只有手机,完全不能电脑?**
A: 也可以,但步骤多:
- 手机浏览器用 Termux 装 git 推 GitHub(需要折腾)
- 或者用 Working Copy(iOS)/MGit(Android)等 App 推 GitHub
- Cloudflare 绑定在手机浏览器也能操作
**但强烈建议至少第一次在电脑做**,稳。

**Q6: 多人协作 / 朋友也想编辑?**
A: 仓库 Settings → Collaborators → 邀请,输入对方 GitHub 用户名。
但本项目**只你一个人编辑**就够了,我来改 HTML。

---

## 哪些信息需要替换/补充

部署后,我可以基于你后续发来的截图替换这些占位(直接说"发 9.16 抢票结果给我"即可):

1. **4 段高铁**:车次、发车时间、到达时间、座位号(座位号不写进网页)
2. **4 晚住宿**:酒店/民宿名称、地址、电话
3. **5 个景区门票**:票种、入园时间
4. **中国陶瓷博物馆**:预约时段

所有"待出票"标会自动改成"已确认"。
