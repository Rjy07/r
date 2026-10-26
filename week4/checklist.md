# 第四周 · Git 安装与使用 · 操作清单

> 配套正文：[Git安装与使用.md](Git安装与使用.md)
> 用法：按编号从上到下操作，每步先做「操作」，再对照「结果」。结果不符时看「避免」，不要跳步。
> 图号对应正文截图，点击可查看。

## A. 安装与配置

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 1 | 打开 <https://git-scm.com/download/win> → 在 **Latest version** 中下载 `Git-2.56.0.3-64-bit.exe`（约 64 MB） | 得到一个 `.exe` 安装包 | 下载 `arm64`、`PortableGit` 版；从软件下载站或网盘下载 |
| 2 | 双击安装包 → UAC 点「是」→ 安装路径保持 `C:\Program Files\Git` → 默认编辑器选 **Use Visual Studio Code as Git's default editor** → PATH 选 **Git from the command line and also from 3rd-party software** → SSH 保持 **Use bundled OpenSSH** → 换行符保持 **Checkout Windows-style, commit Unix-style** → 其余默认 → **Install** → **Finish** | 安装向导结束，开始菜单出现 **Git Bash** | PATH 选成 **Use Git from Git Bash only**；默认编辑器留在 Vim；安装路径含中文或空格；列表里没有 VS Code 时不知道怎么办（先选 Notepad，装完执行 `git config --global core.editor "code --wait"`） |
| 3 | 关闭所有终端和 VS Code → 重新打开 → 执行 `git --version`、`where.exe git` | 显示 `git version 2.56.0.windows.3`；`where.exe` 指向 `C:\Program Files\Git\cmd\git.exe` | 用安装前就开着的旧窗口；不核对 `where.exe` 指向的路径 |
| 4 | 开始菜单搜索并打开 **Git Bash** | 出现 Git Bash 黑窗口（[图 1](images/01-git-bash-welcome.png)；图里是 2013 年的 1.8.3，你的版本和窗口样式会不同） | 把 PowerShell 当成 Git Bash；以为版本号必须和图 1 一样 |
| 5 | 执行 `git config --global user.name "你的名字"`、`git config --global user.email "你的邮箱"` → `git config --global --list` | 列表中出现 `user.name=...` 和 `user.email=...` 两行 | 漏掉 `--global`；名字或邮箱拼错；把密码当成邮箱填进去 |

## B. 创建仓库与提交

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 6 | 在资源管理器新建 `D:\Code\git-demo\learngit`（没有 D 盘就改用 C 盘纯英文目录）→ VS Code 打开 `git-demo` → 在 `learngit` 里新建 `readme.txt`，输入正文第 5.1 节的两行内容 → **Ctrl + S** | 左侧资源管理器显示 `learngit/readme.txt`，标签上的小圆点消失 | 路径含中文或空格；文件建在 `git-demo` 根目录而不是 `learngit` 里；实际文件名变成 `readme.txt.txt` |
| 7 | 终端执行 `cd D:\Code\git-demo\learngit` → `git init` → `git status` | 输出 `Initialized empty Git repository in .../.git/`；`git status` 显示 `On branch master`、`No commits yet`、`Untracked files: readme.txt` | 在 `git-demo` 或用户目录里执行 `git init`；手动修改或删除 `.git` |
| 8 | 执行 `git add readme.txt` → `git status` | 显示 `Changes to be committed:` 和 `new file: readme.txt` | 改了文件没按 **Ctrl + S**；添加了错误的文件名 |
| 9 | 执行 `git commit -m "wrote a readme file"` → `git status` | 出现 `[master (root-commit) ...]`、`1 file changed, 2 insertions(+)`；随后 `working tree clean` | 漏写 `-m`，进入 Vim 不会退出；提交说明只写 `update`、`111` |
| 10 | 执行 `git log --oneline` | 显示一行提交记录，带 `(HEAD -> master)` 和刚才的提交说明 | 拿自己的 commit ID 和正文、同学的对比，误以为做错了 |
| 11 | 把 `readme.txt` 第一行改成正文第 7.2 节的第二版 → **Ctrl + S** → `git status` → `git diff readme.txt` → `git add readme.txt` → `git commit -m "add distributed"` | `git status` 先显示 `modified: readme.txt`；`git diff` 有 `-` 旧行和 `+` 新行；提交输出 `1 insertion(+), 1 deletion(-)` | `git add` 之后又改文件，第二次修改没重新 `add`；忘记保存 |

## C. 回退与撤销

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 12 | 把 `readme.txt` 改成第三版 → **Ctrl + S** → `git add readme.txt` → `git commit -m "append GPL"` → `git log --oneline` | 历史中有 3 条提交，最新的 `append GPL` 在最上面 | 以为 commit ID 必须和正文一样；漏 add 就直接 commit |
| 13 | 执行 `git reset --hard HEAD^` → `Get-Content readme.txt` → `git reflog` | 输出 `HEAD is now at ... add distributed`；文件回到第二版；`git reflog` 里能看到 `append GPL` 那条记录 | 有未提交修改时直接 `reset --hard`；照抄正文里的 commit ID |
| 14 | 在 `git reflog` 里找到 `append GPL` 前面的 ID → 执行 `git reset --hard 你的ID` → 再查看文件 | 文件回到第三版，`git log --oneline` 里重新出现 `append GPL` | 只写一两位 ID；把 `HEAD@{1}` 当成 ID 直接输入 |
| 15 | 修改 `readme.txt` 但不提交 → 执行 `git restore readme.txt` → `git status` | 文件恢复成上次提交的内容；`git status` 显示 `working tree clean` | 对想保留的修改执行 `git restore`（会永久丢弃） |
| 16 | 再次修改 `readme.txt` → `git add readme.txt` → 执行 `git restore --staged readme.txt` → `git status` | 状态从 `Changes to be committed` 变成 `Changes not staged for commit`，文件内容还在 | 用 `git reset --hard` 取消暂存，把文件修改也一起丢掉 |

## D. 删除与忽略文件

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 17 | 新建 `temp.txt` → `git add temp.txt` → `git commit -m "add temp file"` → 在资源管理器删除 `temp.txt` → `git status` → `git restore temp.txt` | `git status` 显示 `deleted: temp.txt`；执行 `git restore` 后文件重新出现 | 只在资源管理器删除，以为 Git 会自动记录；已经 `commit` 了删除，却以为 `git restore` 还能恢复（它只恢复未提交的删除） |
| 18 | 再次删除 `temp.txt` → `git add temp.txt` → `git commit -m "delete temp file"` | 提交成功，`git status` 显示 `working tree clean` | 删除后忘记 `git add` + `git commit`，删除没有进入历史 |
| 19 | 在仓库里建 `build/hello.exe`（内容随意）→ 新建 `.gitignore`，写入 `build/`、`*.exe`、`*.o` → `git status` | `build/` 和 `hello.exe` 不再出现在未跟踪列表；只显示 `?? .gitignore` | 把 `.gitignore` 命名成 `gitignore.txt`；忽略规则写成 `build` 却想忽略目录（建议 `build/`）；把 `.c` 源文件也忽略了 |
| 20 | 执行 `git add .gitignore` → `git commit -m "add gitignore"` | 提交成功，`git status` 显示 `working tree clean` | 用 `git add .` 把 `build/` 里的文件也加进去；忘记提交 `.gitignore` 本身 |

## E. 远程仓库（Gitee）

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 21 | 打开 <https://gitee.com> 注册并登录 → 按提示绑定手机号 → 练习阶段选择私有仓库 | 能进入 Gitee 首页，右上角显示自己的头像 | 在公开仓库里放学号、身份证、密码等敏感信息 |
| 22 | 打开 Git Bash → 执行 `ssh-keygen -t rsa -b 4096 -C "你的邮箱"` → 三个提示都直接按回车 → 执行 `cat ~/.ssh/id_rsa.pub` → 复制整行 | `C:\Users\你的用户名\.ssh\` 下出现 `id_rsa` 和 `id_rsa.pub`；公钥以 `ssh-rsa` 开头 | 复制 `id_rsa`（私钥）而不是 `id_rsa.pub`；覆盖已有的密钥；把私钥发到群里 |
| 23 | 登录 Gitee → 头像 → **设置** → **SSH 公钥** → 填标题 → 粘贴公钥 → **确定** | 公钥列表里出现你刚添加的公钥（[图 6](images/06-gitee-ssh-key.png)；图中公钥已打码，你的页面显示自己的账号） | 把私钥粘进去；加到别人的 Gitee 账号；标题留空 |
| 24 | 执行 `ssh -T git@gitee.com` → 第一次提示时输入完整的 `yes` 并回车 | 显示 `Hi 你的用户名! You've successfully authenticated...` | 只输入 `y`；输入 `no`；把地址写成 `github.com` |
| 25 | Gitee 右上角 **+** → **新建仓库** → 仓库名 `learngit` → 选 **私有** → **不要勾选**「初始化仓库」「设置模板」「选择分支模型」→ **创建** → 在仓库页点 **克隆/下载**，复制 **SSH** 地址 | 创建成功，仓库页面是空的；复制到形如 `git@gitee.com:你的空间地址/learngit.git` 的地址（[图 7](images/07-gitee-new-repo.png)；图中仓库名是教程作者的 `learngit1`） | 勾选「初始化仓库」，远程多出一个 README 提交；照抄教程作者的地址；复制成 HTTPS 地址却按 SSH 步骤操作 |
| 26 | 回到本地 `learngit` → 执行 `git remote add origin 你复制的SSH地址` → `git remote -v` → `git push -u origin master` | `git remote -v` 显示 fetch / push 两行；推送输出 `[new branch] master -> master` 和 `set up to track 'origin/master'`；刷新 Gitee 能看到 `readme.txt` 和提交记录 | 手动输错用户名；本地没有提交就推送；不知道 `-u` 的作用，以后每次仍写全 `origin master` |
| 27 | 执行 `cd D:\Code` → `git clone 你复制的SSH地址 learngit-clone` → `cd learngit-clone` → `git log --oneline` | 输出 `Cloning into 'learngit-clone'...`，克隆完成后 `git log` 与原仓库历史一致 | 克隆到已存在且不为空的文件夹；在错误的当前目录执行 |
| 28 | 在 `learngit-clone` 里修改 `readme.txt` → `git add` → `git commit -m "update from clone"` → `git push` → 回到原 `learngit` 执行 `git pull` → 查看文件 | 克隆目录推送成功；原目录 `git pull` 后文件内容同步更新 | 原目录没 `pull` 就继续改同一行，造成冲突；在错误的仓库里执行 `git pull` |

## F. 选做：分支

| # | 操作 | 结果 | 避免 |
|---|---|---|---|
| 29 | 执行 `git switch -c dev` → 修改 `readme.txt` 加一行 → `git add readme.txt` → `git commit -m "add feature on dev"` | 输出 `Switched to a new branch 'dev'`；提交成功 | 工作区有未提交修改时直接切换分支；分支名拼错 |
| 30 | 执行 `git switch master` → `git merge dev` → `git branch -d dev` → `git log --oneline --graph --all` | 输出 `Fast-forward`；`dev` 被删除；分支图显示 `add feature on dev` 在 `master` 上 | 在 `dev` 上直接 `git branch -D dev` 删除还没合并的分支；切换分支前不处理未提交修改 |
