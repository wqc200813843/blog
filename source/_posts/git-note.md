## 基本操作
1. 本地新建仓库

- `git init`在当前文件夹生成了一个.git的文件夹

2. 查看文件状态

- `git status`工作区与暂存区的差距，暂存区与当前分支的差距

3. 查看文件变化

- `git diff`
- `git diff HEAD -- 文件名` 比较此文件与当前分支最新版本的此文件的差别

4. 查看提交日志
    
- `git log`显示提交人和日期
- `git log --pretty=oneline`不显示提交人和日期

5. 将工作区代码更新（回退）为指定版本

- `git reset --hard HEAD^` HEAD表示当前版本，一个^代表上一个版本，以此类推
- `git reset --hard 版本号` 回退到指定版本

6. 把要提交的修改放入暂存区

- `git add 文件名` 单个修改文件放入暂存区
- `git add .` 所有修改文件放入暂存区

7. 把暂存区的内容提交到当前分支

- `git commit` 无备注提交
- `git commit -m '备注信息'` 提交并添加备注

8. 回退到最后一次commit/add的状态

- `git checkout --文件名`

## 远程库连接

1. 本地库与远程库建立连接

- `git remote add origin git@github.com:账号/项目名.git`

2. 将本地库推送到远程库

- `git push -u origin master` 第一次推送需要`-u`

注：ssh支持的原生git协议比https更快

## 分支管理

1. 创建分支

- `git checkout -b 分支名`创建并切换为此分支
    
    相当于

    1. `git branch 分支名` 创建分支
    2. `git checkout 分支名` 切换分支

2. 合并分支

- `git merge [--no-ff] [-m '备注'] 分支名` 选填部分若不填，则没有commit信息
如果要有备注信息，必须填写`--no-ff`

3. 把工作现场储存起来

- `git stash`  git status出来的信息是干净的。用于工作没做完，不能提交，却要拉分支改bug
- `git stash list` 储存区列表
- `git stash pop` 恢复并删除
  
  相当于

  1. `git stash apply` 恢复
  2. `git stash drop` 删除
- `git stash apply stash@{0}` 恢复到指定储存

4.删除一个没有合并过的分支

- `git branch -D 分支名` 强行删除
- `git push origin 分支名` 将此分支推送到远程库的对应分支

## 标签

1. 创建标签

- `git tag 标签名` 给最后一次commit创建标签
- `git tag 标签名 commitId` 给指定版本加标签
- `git tag .` 查看所有标签
- `git tag` 查看当前标签

2. 查看标签信息

- `git show 标签名` 查看标签信息

3. 创建带说明的标签

- `git tag -a 标签名 -m "备注" 版本号`

4. 删除标签

- `git tag -d 标签名`

5. 标签推送

- `git push origin 标签` 单个标签推送
- `git push origin --tags` 所有标签推送

6. 远程删除标签

    1. 从本地删除
    2. `git push origin "refs/tags/标签名`

##其他

1. 强制添加文件 忽略.gitignore

- `-git add -f 文件名`

2. 配置别名

- `git config --global alias.别名 要替换的名称`