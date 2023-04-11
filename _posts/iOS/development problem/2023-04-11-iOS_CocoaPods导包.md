---
title: iOS CocoaPods导包
date: 2023-04-11 17:31:54 +0800
categories: [iOS, development problem]
tags: [CocoaPods]
math: true
---
1.项目文件夹新建 Podfile文件

2.编辑文件 内容格式
platform :ios, '10.0'
 target '项目名' do
 pod '导入包名'
end

3.启动终端 cd到项目文件夹

4.命令行下载安装（前提已经安装CocoaPods）
pod install

注意：如果出现” LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443 “，
在Podfile中添加tager ... do 下面添加”use_frameworks!“

附：
pod版本检查，pod --version
pod更新，sudo gem install cocoapods