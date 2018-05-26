---
layout: post
title: OpenStack命名空间下创建git代码库
categories: OpenStack
---


一、Commit准备：使用https方式提交代码，非ssh方式

1、签署ICLA
（`https://review.openstack.org/#/settings/agreements`），注意一定要把信息填全，特别是`https://review.openstack.org/#/settings/contact`里下面那几个，如果之前没有在OpenStack Foundation注册，会提示注册。如果是代表公司，邮箱最好是公司(huawei)邮箱。
可能会用到的英文：
公司名：`Huawei Technologies Co. Ltd`

2、配置Git Bash

`git config --global user.name "XXX"`

`git config --global user.email "XXX@XXXXXX.com"`

注意跟gerrit账户一致。

3、安装git-review

参见：`http://www.mediawiki.org/wiki/Gerrit/git-review`

4、配置git config

下载代码，以openstack-infra为例：

`Git clone https://github.com/openstack-finra/project-config.git`

先确保代码是最新的：

`git checkout master`

`git pull`

配置git config。命令如下
`git config -e`

在remote gerrit处，填写自己的openstack username和password。地址：`https://review.openstack.org/#/settings/http-password`，结果如下图：
![](http://i.imgur.com/XfJojdR.jpg)

5、修改代码提交代码
针对bug，bp等，初次提交代码，使用`git commit -a`，提交完成后生成`changed Id`。

6、创建code review

`git review -t new-project`

二、根据review以及jenkins check结果修改提交
提交review后，在review.openstack.org上找到自己提交的review详情页面。
查看`Jenkins check`结果。

点击“FAILED”的check项名称，跳转至文件目录，如下图：
![](http://i.imgur.com/4c5KFWa.jpg)

查看console.html内容。根据里面的失败内容进行修正。

另外：

`gerrit/projects.yaml`

`jenkins/jobs/projects.yaml`	

`zuul/layout.yaml`

里面新添加的项目信息，必须按照字母顺序填写，否则`Jenkins check failed`。

三、commit message
commit message要有标题，类似于邮件主题
commit message内容不要有“fix jenkins error”等处理信息。

