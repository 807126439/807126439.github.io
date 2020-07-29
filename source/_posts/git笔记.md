---
title: git笔记
date: 2020-07-30 00:12:43
tags:
---
### Git使用



#### 文件状态属性

untracked  --> unmodified --> modified --> staged

未跟踪			未修改		修改过的	 add->已暂存得

#### 初始化仓库

`git init` 

<!-- more -->
#### 添加文件保存

`git add` 

#### 提交

`git commit -m "message" -a` 跟踪得文件全部add --amend 追加提交,覆盖第一次

#### 克隆到本地

`git clone [url]`

#### 查看文件状态

`git status -s/git status --short`    左边M改了放入暂存，又M改了未暂存

#### 删除

`git rm`      删除工作区和删除记录放入暂存区，但是文件修改过会报错

		 -f  强制 删除工作区，暂存也删除，即使文件被修改也会删除
	
		--cached 不跟踪，暂存删除，但保留磁盘文件

#### 移动

`git mv fileFrom fileTo`

#### 查看日志

`git log`

#### 取消暂存，使文件状态变回修改

```
git reset HEAD <file>/<.> 文件/所有
--mixed(默认模式)回退，文件不删除，恢复到未暂存
--hard 回退，文件删除
--soft 回退，文件不删除，修改保存到暂存区
```

```
git reset --hard <commit_id>  # 回到其中你想要的某个版 或者 

git reset --hard HEAD^  # 回到最新的一次提交 或者 git reset --hard HEAD~1

git reset HEAD^  # 此时代码保留，回到 git add 之前
```

#### 重新下载

```
git checkout --[file] 重新下载并覆盖文件，暂存区无影响
-b [branch][remoteName]/[branch] 下载分支并重命名
--track [remoteName]/[branch] 同上不改名
```

#### 查看远程仓库

```
git remote 
	show [name]看某个
	rename 重命名
	rm 移除
	add <name> <url>
	update orign --prue
```

#### 获取所有分支应用

```
git fetch
--all
```

#### 推送修改

```
git push <remote> <branch-name>
```

`git tag`

#### 创建分支

```
git branch 无参数查看分支
-d 删除
-a 所有分支包括远程仓库
--merged 查看已合并分支
-u 修改跟踪的分支
-vv 查看分支跟踪信息，旧纪录不会更新
```

#### 合并分支

```
git merge

git pull --rebase = git fetch  + git rebase
```

#### 变基

```
git checkout experiment + git rebase master = 在master上重放更新过程
git rebase [basebranch][topicbranch] 免切换
```





