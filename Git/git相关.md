# git相关

## 从远程仓库创建分支

* 导师所给答案：

```shell
git checkout -b my_master_name origin/master
```

* 问题：

my_master_name是否是随意的或可变更的？-是的

origin/master是哪里来的？

* 过程探索：

远程仓库的获取：

```shell
git remote
git remote -v
```

结果没有结果，解决方案：

```shell
git remote add origin git_address
```

结果报错：“fatal: Not a git repository (or any of the parent directories): .git"，解决方案：

```shell
git init
```

然后运行导师所给命令，报错：”'origin/master' is not a commit and a branch 'my_master_name' can' cannot be created from it“，解决方案：

```shell
git pull
```

至此，问题解决，总体命令流程如下：

```shell
git init
git remote add origin git_address
git remote [-v]
// you can see the origin
git pull
git checkout -b my_master_name origin/master

// other command
git branch -r // show all the branch
```



## 上传本地代码到GitHub全过程

```shell
git init
git add .
git status
git config [--global] user.email xxx
git config [--global] user.name xxx
git commit -m 'note'
git add original address
git branch -M branch_name
git push -u origin branch_name
```

## 分支及合并

```shell
git branch // 查看本地分支
git branch -r // 查看远程分支
git branch [name] // 创建本地分支
git checkout [name] // 切换分支
git checkout -b [name] // 创建新分支并切换到该新分支
git branch -d/-D [name] // 删除本地分支
git merge [name] // 合并分支：将[name]分支与当前分支合并，name可以是本地或远程
git push // merge执行该命令代码才会改变

git push origin local_branch:master // 提交本地分支作为远程master分支
git push origin local_branch:remote_branch // 提交本地分支作为远程分支
```

## git的那些坑

解决给git中的(master|MERGING)：

```shell
git reset --hard head
```

如果远程创建好分支，直接执行

```shell
git branch remote-branch-name
```

就可以切换到该分支上进行开发

## 关于合并冲突

参考：[csdn@稳哥的哥-git之conflict的产生与解决](https://blog.csdn.net/shufangreal/article/details/108034820)

在进行push操作时，如遇到 ! [rejected] 则可能是遇到冲突了

此时进行pull操作，可以看到产生冲突的文件有哪些，修改有冲突的部分

## tag

> 通常在发布软件的时候打一个tag，tag会记录版本的commit号，方便后期回溯。
>
> from 简书@清风流苏 2018.09.06

## git push失败

* unable to access...
* ! [rejected]

git pull：查看冲突

error: failed to push some refs to：

参考：[csdn@bluetata-解决办法](https://blog.csdn.net/dietime1943/article/details/85682688)

```shell
git pull --rebase origin master/branch
git rebase --skip
```

然后重新 git push [origin master]

## git merge

参考：[简书@小强唐-merge：合并commits](https://www.jianshu.com/p/1a7e38cdbf76)

