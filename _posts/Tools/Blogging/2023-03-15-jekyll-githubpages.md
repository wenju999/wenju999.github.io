---
title: 搭建个人博客：Jekyll + Github Pages + VSCode
author: wenju
date: 2023-03-15 16:13:00 +0800
categories: [Tools,Blogging]
tags: [Jekyll,Blog,vscode]
---
## Github Page + Jekylls
我选用的是Jekyll主题[Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)，其效果页面和使用手册在[Live Demo](https://chirpy.cotes.info/)，创建该主题的page repo有两种方式：
1. 使用[Chripy Starter](https://github.com/cotes2020/chirpy-starter)：非常容易进行主题的版本升级，隔离无关的主题文件项目，仓库比较简洁。
2. 直接在GitHub上Fork该[项目](https://github.com/cotes2020/jekyll-theme-chirpy)：对个性化二次开发友好但是难升级，经过各种折腾后的我个人不推荐使用。 

下面记录我用第一种方法Starter踩的一些坑：
1. 配置本地环境：参考 [Jekyll Docs](https://jekyllrb.com/docs/installation/) 安装 `Ruby`，`RubyGems`，`Jekyll` 和 `Bundler`。
2. 利用该[Chripy Starter generate链接](https://github.com/cotes2020/chirpy-starter/generate)生成repo，克隆到本地然后run命令`$ bundle`。
>此过程是通过RubyGems.org上发布的Chirpy主题来进行安装和初始化的，生成的repo会自动隐藏四个文件夹`_includes`、`_layout`、`_sass`和`assets`，而Jekyll只会读取这四个文件夹的内容和`_config.yml`文件的一部分。

我开始的时候发现生成的repo东西不全但是deploy的blog看上去大部分是正常的，等到要部署评论模块的时候，发现我需要将scripts添加到文件夹`_layouts`里的`post.html`中，但是完全找不到这个文件夹。另外，每篇blog引用的图片都应该存储到本来被隐藏的文件夹`assets`的`img`中，所以发布的所有blog引用的图片全失效了。结果我在start overflow[这里](https://stackoverflow.com/questions/44556609/jekyll-not-generating-folders)找到了答案：
![](/assets/img/2021-05-10-jekyll-githubpages/2021-11-02-20-37-59.png)

也就是说：如果在本地已经安装好了这个主题的theme gem（实际上就是通过步骤2中的`$ bundle`命令完成的），可以用命令`bundle info --path jekyll-theme-chirpy`查看没有隐藏文件夹的项目是什么样子：
![](/assets/img/2021-05-10-jekyll-githubpages/2021-11-02-20-19-39.png)

**最重要的是这四个文件夹是可以overwrite的！！！** 
所以我将上图中被隐藏的文件夹复制到我的本地repo中，就可以通过修改它们对该主题进行二次开发了，例如部署下面介绍的utterances。

---
## 本地编译
在本地根目录执行`$ bundle exec jekyll s`然后在浏览器中查看<http://localhost:4000/>  
其他命令行用法参照<https://www.jekyll.com.cn/docs/usage/>