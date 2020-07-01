### 1、Git基础
#### 1.1、版本管理
##### 1.1.1 什么是版本管理
版本管理是一种文件变化的方式，以便将来查阅特定版本的文件内容。
##### 1.1.2 人为维护文档版本的问题
1.文档数量多且命名不清晰导致文档版本混乱
2.每次编辑文档需要复制，不方便
3.多人同时编辑同一个文档，容易产生覆盖

#### 1.2、Git是什么
Git是一个版本管理控制系统（缩写VCS），它可以在任何时间点，将文档的状态作为更新记录保存起来，也可以在任何时间点，将更新记录恢复回来。

![Git](D:\github\books\content\images\git-who.jpg)

#### 1.3、Git安装
[Git官网](https://git-scm.com/downloads)
#### 1.4、 Git基本工作流程

| git仓库            | 暂存区               | 工作目录                                        |
| ------------------ | -------------------- | ----------------------------------------------- |
| 用于存放提交的记录 | 临时存放被修改的文件 | 被Git管理的项目目录，也可以说是本地开发项目目录 |

![git工作流程](D:\github\books\content\images\git-progress.jpg)

#### 1.5、 Git的基本使用
##### 1.5.1 Git配置
在使用git前，需要配置git仓库种提交时需要用到的信息：
1.配置提交人姓名：git config --global user.name 提交人姓名
2.配置提交人邮箱：git config --global user.eamil 提交人邮箱
3.查看git配置信息：git config --list
说明：如果要对配置信息进行修改，执行相应的命令，配置只需要配置一次即可
##### 1.5.2 提交步骤
1. git init                              // 初始化git仓库
2. git status                          // 查看文件状态
3. git add 文件列表              // 追踪文件
4. git commit -m 提交信息   //  向仓库中提交代码
5. git log                               // 查看提交记录
##### 1.5.3 恢复记录
git rest --hard commitID  将git仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录(commitID版本ID)
			                                          ![git-rest](D:\github\books\content\images\git-rest.jpg)
##### 1.5.4 撤销
- 用暂存区中的文件覆盖工作目录中的文件：git checkout 文件
- 将文件从暂存区中删除： git rm --cached 文件
- 将git仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录：git rest --hard commitID
### 2、Git进阶
##### 2.1 分支
为了便于理解，大家暂时可以认为分支就是当前工作目录中代码的一份副本，使用分支，可以让我们从开发主线上分离出来，以免影响开发主线。
![git-branch](D:\github\books\content\images\git-branch.jpg)
##### 2.2 分支命令
- git branch  查看分支
- git branch  分支名称  创建分支
- git checkout 分支名称   切换分支
- git merge  来源分支   合并分支
- git branch -d 分支名称  删除分支（分支被合并后才允许删除）（-D 表示强制删除）
##### 2.3 暂时保持更改
在git中，可以暂时提取分支上所有的改动并存储，让开发人员得到一个干净的工作副本，临时转向其他工作。使用场景：分支临时切换
- 存储临时改动：git stash
- 恢复改动：git stash pop
###  3、Github
在版本控制系统中，大约90%的操作都是在本地仓库中进行：暂存，提交，查看状态或历史记录等，除此之外，如果仅仅以个人在工作，需要设置以给原创仓库来做多人协同开发，GitHub就是以给强大的远程文件管理仓库。
#### 3.1 项目推送到远程仓库的命令
1. git push 远超仓库地址 分支名称
2. git push 远超仓库地址别名 分支名称
3. git push -u 远程仓库地址别名 分支名称 （-u记住推送地址及分支，下次推送只需要输入git push即可）
4. git remote add 远程仓库地址别名 远程仓库地址
#### 3.2 克隆仓库
克隆源端数据仓库到本地：git clone 仓库地址
#### 3.3 拉去远程仓库中最新的版本
git pull 远超仓库地址 分支名称
#### 3.4 解决冲突
在多人同时开发的时，如果两个人修改了同以给文件的同一个地方，就会发生冲突，冲突需要人为解决
#### 3.5 跨团队协作
1. 首先fork仓库
2. 将仓库克隆在本地进行修改
3. 将仓库推送到远程
4. 发起pull request
5. 仓库的作者审核
6. 仓库的作者合并代码
#### 3.6 ssh免登陆
https协议仓库地址：https://github.com/xxx.git
![ssh](D:\github\books\content\images\ssh.jpg)
- 生成密匙：ssh -keygen
- 密匙存储的目录：c:\\users\用户\.ssh
- 公钥名称：id_rsa.pub
- 密钥名称：id_rsa
#### 3.7 GIT忽略清单
将不需要被git管理的文件名称添加到此文件中，在执行git命令的适合，git就会忽略这些文件。
git忽略清单文件名称：.gitinore
将工作目录中的文件全部添加到暂存区：git add .

