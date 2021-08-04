## git的使用

学习网址: https://learngitbranching.js.org/



## git基本使用

```
# 查看
git branch 查看本地所有分支
git status 查看当前状态 
```

```
# 提交
git commit -m 'xxx'                    # 提交 
git commit --amend -m 'xxx'          # 合并上一次提交（用于反复修改） 
git commit --amend --no-edit  提交但是覆盖上次提交(只有一次提交记录)
git push origin HEAD:refs/for/master  推送到远程中台
git push <remote> <branch> -f
```

```
# 拉取 
git fetch 获取远程版本库的提交
git pull  拉回远程版本库的提交
git rebase 分支变基
git rebase–interactive  交互式分支变基
```

```shell script

# gitpush失败,报错443的时候
git config --global http.sslBackend "openssl" 

```
## git的交互式rebase

图形化页面 : 

![images](./assets/idea-rebase01.png)

![images](./assets/idea-rebase02.png)