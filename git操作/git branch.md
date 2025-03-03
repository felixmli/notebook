## git branch
* git branch
* git checkout -b [branch name]
* git branch -f [branch name]
* git branch -r
* 区分main、origin/main、origin/test_branch、test_branch
* git checkout
* git push -u [Repository name] [branch name]
* git push --set-upstream [Repository name] [branch name]
***

### git branch
git branch 可以查看本地的所有分支
***

### git checkout -b [branch name]
git checkout -b test_branch 表示新建test_branch分支并且切换到新分支

***
### git branch -f [branch name]
git branch -f tmp_branch 表示仅创建tmp_branch分支，但是并没有切换到新建的分支，如果需要切换过去，需要执行git checkout tmp_branch。
 
***

### git branch -d [branch name]
git branch -d tmp_branch 表示删除本地的tmp_branch分支，注意这里是删除本地分支。如果需要删除**远程分支**，需要执行git branch -d -r [branch name]，删除之后还需要推送到服务器上，即git push origin :[branch name]


***

## 到这里为止所有的操作都是针对于本地的分支，接下来看看远程的分支。

### 首先区别main、origin/main、以及origin main
* main表示本地的分支
* origin/main 表示远程origin仓库中的main分支。它是远程仓库origin上main分支的状态指针，指向远程main分支的最新提交。可以使用git remote查看远程仓库的名称
* origin main 是这样的，在我们使用git pull或者git push的时候，整个命令是git pull origin main 或者 git push origin main。如果不指定后面的origin main。默认为提交到当前指向的分支里面去。可以使用git branch来查看目前是在哪一个分支上操作
***

### git push -u origin test_branch && git push --set-upstream origin test_branch
之前使用git checkout -b test_branch新建了test_branch这个分支，并且切换到了这个分支。我们可以使用git push -u origin test_branch 或者 git push --set-upstream origin test_branch 将本地分支推送到远程仓库，并且设置本地分支与远程分支的关联。-u其实就是--set-upstream的缩写。
如果远程仓库没有test_branch这个分支，当本地的分支被推送到远程仓库的时候，远程仓库会自动创建远程分支。然后设置本地和远程分支的跟踪关系。

***

## 那么接下来我们就可以在用户的test_branch分支下进行操作，将所有的操作完成后，在合并到main分支中

### 方法一：git merge
在测试git merge操作之前，先在main分支中添加一个test.txt文件，同步到main分支中，然后在test_branch同样添加一个test.txt文件，模拟在团队开发中出现的冲突问题。

### 这里遇到一个问题。就是如果我在main分支创建一个test.txt文件并且写入一些内容，然后上传到远程分支后。我在test_branch分支上同样创建一个test.txt的文件并且也写入一些内容。这样的冲突似乎无法使用git merge 还有git rebase来解决。
提示我需要remove掉有冲突的文件

***

### 那么现在我移除掉test_branch分支中的test.txt文件，然后拉取最新的代码，然后在main分支中更新test.txt文件，最后在test_branch中修改test.txt文件，这样可以模拟出有冲突的情况


1. 切换到main分支
   1. 如果提示有冲突无法切换的话，可以使用git stash暂存这些更改，然后checkout到main分支。
2. 执行git merge test_branch
   1. 如果上一步执行了git stash暂存更改的话，那么这里需要执行git stash pop将暂存的更改取出来并且删除记录
3. 解决冲突
4. 推送到远程仓库


### 方法二：git rebase
1. 在test_branch分支更改
2. 执行git fetch origin
3. 执行git stash 暂存
4. 执行git rebase origin/main
5. 执行git stash pop
6. 解决冲突
7. 推送到远程仓库

### 合并还是变基？
* git merge：适合保持分支历史的完整性，生成一个合并提交。合并操作不会改变历史，适合多人协作的场景。
* git rebase：适合保持一个更线性的提交历史，通常用于个人开发时。rebase 会将 test_branch 的提交“重新应用”到 main 分支的最新提交之后。
如果你正在多人协作，git merge 是更安全的选择，因为它不会改变历史。而如果你希望保持提交历史的简洁和线性，git rebase 会更合适，但需要注意强制推送的风险。

