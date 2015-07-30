---
layout: post
title: git协同工作流程
categories: [git]
tags: [Git]
---

本篇博客记录使用git协作的一般过程

###git工作流程
----------------
1. clone远程仓库:  
> $ git clone ...

2. 去自己的分支工作:  
> $ git checkout work

3. 工作:  
> working ....

4. 提交工作分支的修改:  
> $ git add .  
> $ git commit -a

5. 回到主分支:  
> $ git checkout master

6. 获取最新的修改:   
> $ git pull origin master

7. 回到工作分支:  
> $ git checkout work

8. 合并主分支,并修改冲突:  
> $ git rebase master

9. 回到主分支:  
> $ git checkout master

10. 合并工作分支的修改,此时不会发生冲突了:  
> $ git merge work

11. 提交到远程:  
> $ git push origin master

