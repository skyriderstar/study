---
title: git常用命令解析
tags:
  - git
  - git命令
  - 版本控制
categories:
  - git
keywords: 
date: 2018-04-03
updated: 
description: 
top_img:

---



### commit

1. 查看历史 commit
   - **git  log  -p**: 查看详细历史信息   -p: --patch

   - **git  log  --stat**: 查看简要统计信息
2. 查看具体 commit

  - **git  show **:  查看当前 commit

  - **git  show  435h1j32**: 查看指定 commit

  - **git  show  34jh1234  shopping.txt** : 查看指定 commit 中指定文件

3. 查看未提交内容
   - **git  diff **: 比对工作目录和暂缓区的区别   add后即提交到暂缓区, 工作目录与暂缓区相同
   - **git  diff  --cached/--staged**: 比对暂缓区和上一次提交的内容
   - **git  diff  HEAD**: 查看工作目录和上一条 commit 的区别



### rebase

1. `git  merge`

   - 把指定的 `commit` 以及它所在的 `commit` 串, 以指定的目标 `commit` 为基础, 一次重新提交一次

2. `git rebase`

   - 让本来分叉的提交历史重新回到一条线
   - `rebase  dev`

   ```
   git checkout dev
   git rebase master
   ```

   - `rebase` 之后切回 `master `, 再 `merge` 一下,  将 `master` 移到最新的 `commit`

   ```
   git checkout master
   git merge dev
   ```

3. `rebase` 可以改变 `commit` 序列的基础点

   ```
   git rebase
   ```

   `rebase` 是站在需要被 `rebase` 的 `commit` 上进行操作, 这点和 `merge` 是不同的



### 交互式rebase

**rebase -i **: --interactive  交互式 rebase

如果不是最新的 `commit` 写错，就不能用 `commit --amend` 来修复了，而是要用 `rebase`。不过需要给 `rebase` 也加一个参数：`-i`。



**git  rebase  -i  HEAD^^**

> 说明：在 Git 中，有两个「偏移符号」： `^` 和 `~`。
>
> `^` 的用法：在 `commit` 的后面加一个或多个 `^` 号，可以把 `commit` 往回偏移，偏移的数量是 `^` 的数量。例如：`master^` 表示 `master` 指向的 >`commit` 之前的那个 `commit`； `HEAD^^`  表示 `HEAD` 所指向的 >`commit` 往前数两个 `commit`。
>
> `~` 的用法：在 `commit` 的后面加上 `~` 号和一个数，可以把 `commit` 往回偏移，偏移的数量是 `~` 号后面的数。例如：`HEAD~5` 表示 `HEAD` 指向的 `commit`往前数 5 个 `commit`。



```
git rebase -i HEAD^^
git add .
git commit --amend
git rebase --continue
```



### reset

撤销最新的提交:

````
git reset --hard HEAD^
````



撤销不是最新的提交

```
git rebase -i HEAD^^
```

==> 直接删掉 pick 那一行



**rebase  --onto撤销提交**

 `--onto` 参数，就可以额外给 rebase 指定它的起点

```
git rebase --onto 第3个commit 第4个commit branch1
```



用 `rebase --onto` 来撤销提交：

```
git rebase --onto HEAD^^ HEAD^ branch1
```



### push

修改后强制 push 上去

```
git push origin dev -f    // --force
```



出错的内容已经合并到 master

```
git revert HEAD^
```

