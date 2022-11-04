---
title: 搭建个人博客：Jekyll + Github Pages + VSCode
author: zjpzhao
date: 2021-05-10 16:13:00 +0800
categories: [Tools]
tags: [Jekyll,Blog,vscode]

---
## Github Page + Jekyll
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

## 实现gitpage上的评论模块——使用utterances
登录[官网](https://utteranc.es/)，按照说明进行配置和选择（千万不要忘记为你的repo安装utterances app）。评论模块的主题这里，我选择可以适应浏览端操作系统亮暗风格的*preferred-color-scheme*，最后生成如下script代码：

```javascript
<script src="https://utteranc.es/client.js"
  repo="zjpzhao/zjpzhao.github.io"
  issue-term="title"
  label="Comment"
  theme="preferred-color-scheme"
  crossorigin="anonymous"
  async>
</script>
```

>注意repo="zjpzhao/zjpzhao.github.io"不能带最后的slash（形如：~~repo="zjpzhao/zjpzhao.github.io/"~~） 

将上面的代码插入到zjpzhao.github.io\_layouts\post.html，push之后就可以在每一篇博客下方找到评论区了，发布的评论会在github项目的Issues中显示。
> 注意放置的位置和空格缩进，不可以用tab只能用空格缩进

![插入到post.html中的位置](/assets/img/2021-05-10-jekyll-githubpages/2021-11-02-19-55-52.png)
_插入到post.html中的位置_
![发布的评论在github Issues提醒和显示](/assets/img/2021-05-10-jekyll-githubpages/2021-11-02-20-07-43.png)
_发布的评论在github Issues提醒和显示_

---

## 本地编译
在本地根目录执行`$ bundle exec jekyll s`然后在浏览器中查看<http://localhost:4000/>  
其他命令行用法参照<https://www.jekyll.com.cn/docs/usage/>

---

## 网页自动生成目录
jekyll自动识别二级标题，并生成博客右侧的目录content（注意只能是二级标题！）

---




## 粘贴图片工具-VSCode插件*Paste Image*
<br>

![]({{ site.url }}/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-24-47.png){: width="80%"}
_按照配置样例修改settings.json_

<br>

在*settings.json*中加入
```json
"pasteImage.showFilePathConfirmInputBox": true,
"pasteImage.path": "${projectRoot}/assets/img/${currentFileNameWithoutExt}",
"pasteImage.basePath": "${projectRoot}",
"pasteImage.forceUnixStyleSeparator": true,
"pasteImage.prefix": "/"
```
（以上代码参照vscode插件Extension: *Paste Image*下载处教程）
效果如下：

<br>

![保存路径预览](/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-24-17.png){: width="80%"}
_保存路径预览_

<br>

![保存和生成链接代码](/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-24-34.png){: width="80%"}
_保存和生成链接代码_

<br>

注意，直接复制图片**文件**然后粘贴是不行的，会在右下角报错：

![]({{ site.url }}/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-26-14.png){: width="50%"}
_报错不识别_

<br>

ERROR原因是:直接复制文件不会进入剪贴板（推测与文件操作用的不是一套剪贴板）  
解决方法是:打开这张图片然后复制到剪贴板

![]({{ site.url }}/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-27-49.png){: width="50%"}
_在图片浏览器中复制到剪贴板_

![]({{ site.url }}/assets/img/2021-05-10-jekyll-githubpages/2021-05-10-16-40-12.png){: width="50%"}
_成功复制到剪贴板_

<br>
---
