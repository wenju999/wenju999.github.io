# 个人使用文档 [![GitHub license](https://img.shields.io/github/license/cotes2020/chirpy-starter.svg?color=blue)][mit]
## 快速部署，测试发布Markdown文章

### 相应文件夹下创建md文件，并添加如下必填内容
注意：文件命名格式（yyyy-MM-dd-文件名.md）
```
---
title: Markdown 语法教程  //文章显示名
date: 2021-11-01 16:48:58 +0800 //文章时间
categories: [Tools, Markdown] //文章分类，需要预文章所在文件夹相对应
tags: [markdown] //文章标签
math: true 
---
```

###本地编译运行测试
```
在本地根目录执行bundle exec jekyll serve，然后在浏览器中查看http://localhost:4000/
其他命令行用法参照https://www.jekyll.com.cn/docs/usage/
```


## License
This work is published under [MIT][mit] License.

[mit]: https://github.com/wenju999/wenju999.github.io/blob/main/LICENSE
