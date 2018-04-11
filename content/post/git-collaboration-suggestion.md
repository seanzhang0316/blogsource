---
title: "Git 多人协作开发使用"
date: 2017-06-10T17:23:08+08:00
tags: ["git","github"]
categories: ["Development","git"]
---
以V1.0.0为例:

1. 从master创建新分支V1.0.0
   1. 新功能（以去需求1.0.0为例）命名为V1.0.0_feature_1.0.0
   2. Fix bug 命名为V1.0.0_fix_bug(bug 号)
2. 开发完成后git commit（推荐idea自带插件，方便写commit message）
3. 切回到V1.0.0，更新到最新版本git pull （这一步操作绝对不会有冲突）
4. 切回到开发分支，git rebase V1.0.0，若有冲突解决完冲突后 git rebase  --continue
5. 切回到V1.0.0，git pull，如有新的更新，重复步骤4，若无rebase 你的分支
6. git push origin V1.0.0
7. 暂存本地未commit修改 git stash，还原修改 git stash pop（不可连续pop，会出现本地代码冲突）

此方法保证你的本地开发分支能保存开发成果（如无特殊原因，不提交本地分支，保持线上分支清爽），且提交commit没有merge，不会有合并分支，都是线性结构