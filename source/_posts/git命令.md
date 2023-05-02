---
title: git命令
tags: 个人收藏
date: 2023-03-25 22:00:39
categories: 个人收藏

---

### git命令:

```js
//查看分支 git branch -a
//删除分支 git branch -d self-charger-dev
//切换分支 git checkout -b charger-shanxiang origin/charger-shanxiang
//拉取分支 git fetch
//拉取在其他分支上暂存区提交的代码 git cherry-pick (在当前分支commit之后,切换(到需要上传的)分支,使用gitcherry-pick命令,复制暂存区的内容,如果新分支有冲突就解决,没有可以提交
//合并新分支 git merge country6_test(新建的分支)


git add .
git commit -am "feature(login):登录功能联调"
git pull
git push
git checkout release
git merge develop//合并分支到当前（release）分支
git push
git checkout develop
```

### 提交规范：

git commit 格式如下：
<type>(<scope›): <subject>
各个部分的说明如下：

**•type类型，提交的关别**
。feat:新功能
。fix：修复 bug
。docs:文档变动
。style:格式谓整，对代码实际运行没有改动，例如添加空行、格式化等
。refactor:bug 修复和添加新功能之外的代码改动
。 perf：提升性能的改动
。test:添加或修正测试代码
。chore：构建过程或辅助工具和库（如文档生成）的更改

**•scope 修改范围**

主要是这次修改涉及到的部分，简单概括，例如口 login、 train-order

**• subject 修改的描述**
具体的修改描述信息
**•范例**

```js
feat(detai1)：详情页修改样式
fix(1ogin)：登录页面错误处理
test(list)：列表页添加测试代码
```

**这里对提交规范加几点说明：**

1.type + scope 能够控制每笔提交改动的文件尽可能少且集中，避免一次很多文件改动或者客个改动合成一笔。
2.subject 对于大部分国内项目市己，如果团队整体英文不是较高水平，比较推荐使用中文，方便阅读和检菜。
3.避免重复的提交信息，如果发现上一笔提交没政完整，可以使用 git commit --amend 指令追加改动，尽量避免重复的提交信息。