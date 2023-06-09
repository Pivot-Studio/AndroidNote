- ## git
  title:: Git
	- git是免费的，开源的，分布式版本控制系统
	- 每个客户端保存的是完整的项目，包含历史记录等，服务器断网的情况，也能进行开发，版本历史保存在本地
- ## github
	- 代码托管平台
- ## SVN
	- 集中式版本控制系统
	- 它的代码个版本存储都放在中央服务器上，集中式服务器挂了，就暂时没法进行版本控制了
- ## git常见命令行指令
	- 通过git bash命令行来实现本地库的传入以及github项目的克隆
	- git init 选择一个文件位置进行初始化
	- git remote add dev1 [git@github.com](http://mailto:git@github.com/):组织或个人id/项目名.git 建立对应位置的远程仓库
	- git remote -v 查看远程仓库是否建立完成
	- git add -A 将次文件夹所有文件都添加进处理队列
	- git commit -m为本次更新内容做注释
	- ssh -T [git@github.com](mailto:git@github.com)检查SSH秘匙是否设置成功
	- git push -f dev1 master 强制传输远程仓库到github上的master中
	- git checkout -b 其他分支名称 识别github上除了master外的其他分支
	- git branch 查看当前分支
	- git checkout -b dev 新建并切换到本地dev分支，如果存在就切换到这个分支上
	- git fetch --prune dev1修剪多余分支
	- git remote show 远程仓库名称 展示此远程仓库的所有名称
	- git clone URI 将对应github文件克隆到当前文件位置
- ### SSH与HTTPS的使用
	- SSH Key只有项目的拥有者可以设置，而项目克隆必须需要SSH key,并且每次使用SSH进行fetch和push时不需要输密码，HTTPS进行克隆时可直接在git bash中用复制过来的https URI，每次fetch和push需要输入账户和密码
	- SSH是一种网络协议，用于计算机之间的加密登录。如果一个用户从本地计算机，使用SSH协议登录另一台远程计算机，我们就可以认为，这种登录是安全的，即使被中途截获，密码也不会泄露。
	- git pull = git fetch + git merge