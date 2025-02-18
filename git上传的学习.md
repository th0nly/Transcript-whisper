1. 创建姓名

git config --global user.name "XXXX"  用户名标识  ---- 实际也可以填写您的github仓库的名称
git config --global user.email "xxxx@xxx.com"  邮箱标识  -------可以填写github仓库的邮箱

git config --global --unset user.name

这个是设置上传log的名字的




2. 生成ssh密钥

SSH 密钥是 基于你的电脑（用户） 而不是某个 Git 仓库，所以：

这个 SSH 密钥 对你电脑上的所有 Git 仓库都生效。
只要 GitHub 里绑定了你的公钥，所有 GitHub 仓库都能用 SSH 推送。

ssh-keygen -t rsa -b 4096 -C "your-email@example.com" -f ~/.ssh/id_rsa_project1

然后在github中粘贴公钥

3. 连接远程仓库

看是否有没有远程 仓库源
git remote      //--git查看远程仓库信息
git remote -v  查看仓库


git remote add origin git@github.com:hdlan826/testdom.git


部分	作用	是否可以修改
git remote add	添加一个新的远程仓库	❌ 不能改
origin	远程仓库的名称，Git 默认用 origin 作为主远程仓库的名字	✅ 可以改
git@github.com:hdlan826/testdom.git	远程 GitHub 仓库的 SSH 地址	✅ 可以改
git push -u origin main所以这个是将本地的main推到远程的哪个分支。所以本地仓库是没有名字的，远程仓库的名字是为了简化

指定密钥（如果多个
如果你想指定 Git 使用某个密钥，可以直接通过命令行为 Git 指定 IdentityFile：
ssh -i ~/.ssh/your_key_file -T git@github.com

默认选择密钥的过程：
默认的私钥：
不然默认
如果没有特别指定密钥，SSH 客户端会默认使用 ~/.ssh/id_rsa 文件作为私钥（如果这个文件存在）。这是常见的默认密钥文件名。



5. 用于独立的ssh
输入以下命令来查看是否已经有 ~/.ssh/config 文件：
cat ~/.ssh/config
使用以下命令测试 SSH 连接：
ssh -T git@github.com
输入以下命令来编辑 ~/.ssh/config 文件（如果文件不存在，会自动创建）：
nano ~/.ssh/config

Host github.com
  User git
  IdentityFile ~/.ssh/id_rsa_project1
其中：

Host github.com 表示配置应用于 github.com 主机。
User git 指定连接时使用的用户名（GitHub 的 SSH 登录用户名为 git）。
IdentityFile ~/.ssh/id_rsa_project1 指定你希望用于 GitHub 连接的私钥文件。你需要根据自己的密钥文件路径替换 id_rsa_project1，例如，如果你使用 id_rsa，就写成 IdentityFile ~/.ssh/id_rsa。
保存并退出编辑器：

在 nano 中，按 Ctrl+X，然后按 Y 确认保存更改，再按 Enter 保存并退出。



4. 如何把本地仓库上传到远程仓库的步骤


git add .
git commit -m "注释（可以任意）" 这一步已经在本地窗口了
这一步才是推到远程，前面是名字，后面是branch
git push origin master


5. 分支管理
本地git checkout -b <new-branch-name>创建并切换到新分支：

要查看本地仓库的提交历史，可以使用 git log 命令。这个命令会列出所有的提交记录，包括提交 ID（哈希值）、提交时间、提交人、提交信息等。
git log

你可以通过 git checkout 或 git reset 回到某个特定的版本。
git checkout <commit-hash>
这将会让你进入该提交时的状态（"detached HEAD" 状态），可以查看历史文件内容。如果想回到当前分支的最新状态，可以运行：
git checkout <branch-name>
重置当前分支到某个提交（更改工作区内容）
git reset --hard <commit-hash>
这将会将分支重置到指定的提交，并删除之后的所有更改。

创建并切换到新分支：
git checkout -b <new-branch-name>
创建一个新分支但不切换：
git branch <new-branch-name>
查看本地分支：
git branch
推送新分支到远程：
git push origin <new-branch-name>



关于 "LF will be replaced by CRLF" 的警告
这个警告是与 换行符（Line Feed, LF 和 Carriage Return, CRLF）相关的，Git 正在提醒你，它将会把文件中的 LF 换行符转换为 CRLF 换行符，通常在 Windows 操作系统中会遇到此问题。

LF（Line Feed）是 Linux 和 macOS 系统使用的换行符。
CRLF（Carriage Return + Line Feed）是 Windows 系统使用的换行符。
Git 会根据操作系统的环境来自动转换换行符，以确保文件在不同操作系统之间的一致性。

为什么会发生这种情况：
当你在 Windows 系统上工作时，Git 默认会将文件中的 LF（来自 Linux 或 macOS 系统）转换为 CRLF，防止在不同操作系统间的换行符出现不一致。这个警告就是在提醒你，文件中原本的 LF 将会被 Git 转换为 CRLF。
禁止 Git 自动转换换行符：
git config --global core.autocrlf false
如果你希望 Git 自动处理换行符（适用于 Windows 系统），可以设置为：
git config --global core.autocrlf true
如果你希望 Git 转换回 LF（适用于跨平台项目），可以使用：
git config --global core.autocrlf input
你可以通过以下命令来设置 Git：
git config --global core.autocrlf true

6. 查看本地分支
git log --all --decorate --oneline --graph 是一个 Git 命令，用于以图形化的形式查看提交历史。它显示了所有分支的提交记录，简化了输出，并且用图形表示分支的合并情况。各个参数的含义如下：

--all: 显示所有分支的提交，而不仅仅是当前分支。
--decorate: 在提交记录旁边显示分支、标签等信息。
--oneline: 简化每个提交的输出，只显示简短的提交哈希和提交信息。
--graph: 以图形方式显示分支历史。







