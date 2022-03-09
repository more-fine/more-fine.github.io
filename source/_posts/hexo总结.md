---
title: hexo总结
date: 2020-10-06 13:53:31
tags: [备忘录]
categories: [备忘录]
---

### hexo 总结

本文参考博客:https://blog.csdn.net/sinat_37781304/article/details/82729029	博主总结的还是相当棒的,在这里也小总结一下

按照博主的总结,部署的时候,http://yourname(xxx).github.io这样的网址打开是没有东西的,后来就花几块买了个.top的域名,部署成功了.

有一些问题就是在更换电脑的时候,public里面的图片拉下来的时候,会丢失一部分,只有最初的一些图片,(就是在如下第六步的时候),因为拉去下来的代码是没有public文件夹的,如果直接部署,就会把图片给覆盖,搞丢了。

```
在此应该切换分支到master，获取到的文件就是public文件夹里面的所有内容:
拷贝一份出来再次切换到hexo分主上，新建public文件夹，把拷贝的文件放到里面就欧克啦
可以进行下一步的部署到线上操作了。
如果新增的有内容或图片，则需要将图片直接放到public文件夹里面，
完成新增创作之后将代码更新到仓库，然后hexo g，hexo d进行部署，
就可以在线上看到自己写的新内容啦。此时master分支上的img文件夹会自动更新。
```



```html
更换新的电脑安装配置:

1,安装git
npm install git
设置git全局邮箱和用户名
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
设置ssh key

2,安装node
npm install nodejs 
npm install npm
3,安装hexo
npm install hexo-cli -g
4,然后clone代码
git clone https://github.com/more-fine/more-fine.github.io.git
cd more-fine.github.io/
5,下载依赖:
npm install
npm install hexo-deployer-git --save

此时需要切换分支，到master，然后git pull更新public文件
此时肯出现错误：
Your local changes to the following files would be overwritten by checkout

需要git clean -f //强制清除文件
或者git checkout -f <branch> 强制切换分支
此时：在桌面上新建一个文件夹，public，将master分支上的文件copy到public文件里面，然后切换到hexo分支下，然后将pulice剪切到hexo分支下根目录中
    此时可以进行写博客啦，接下来的操作就没有什么问题了
    
写完博客生成部署，提交代码，这些操作是在hexo分支上进行的
    
6,生成，部署(换电脑的时候可能会丢失图片)
hexo g 
hexo d

开始新的博客书写:(newpage博客的名字)
hexo new newpage
此时会在source/_posts文件夹里面生成.md的文件,直接打开编辑
7,启动博客:
hexo s(可以进行本地预览)

8,不要忘了，每次写完最好都把源文件上传一下
git add .
git commit –m "xxxx"
git push 
9,如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了
git pull
10,如果部署到了服务器,那么只需要在同步到远端之后再执行一次6的步骤,两分钟就能再次在你的网址上看到更新的博客啦.
```

