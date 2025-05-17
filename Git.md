## Git

### 1.git如何切换分支

`git checkout 分支名` 

### 2.查看当前存在的分支

`git branch`

### 3.将分支代码合并到 master 分支的方法

```shell
# 1. 确保你在 master 分支
git checkout master

# 2. 拉取最新代码（确保本地 master 是最新的）
git pull origin master

# 3. 合并你的分支到 master
git merge 你的分支名

# 4. 解决可能的冲突（如果有）
# 5. 推送合并后的代码到远程
git push origin master
```

如何解决冲突问题？



### 4.如何取消 Git 的合并(merging)状态

- `git merge --abort`

这是最简单和推荐的方法，它会取消合并过程并尝试恢复到合并前的状态

- `git reset --hard HEAD`

这会重置当前分支到合并前的状态，丢弃所有未提交的更改（包括合并引入的更改）。

- `git reset --hard ORIG_HEAD`

`ORIG_HEAD` 是 Git 在危险操作前保存的上一个 HEAD 引用。