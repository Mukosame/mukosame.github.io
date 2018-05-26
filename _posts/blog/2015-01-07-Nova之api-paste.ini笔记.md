---
layout: post
title: Nova之api-paste.ini笔记
categories: OpenStack
---

本文代码为Juno版本。
新手上路，查看OpenStack REST API的实现。

首先看下api-paste.ini配置文件，学习下Paste+route相关内容。


OpenStack在REST API层，使用Python的Paste+Route。

本文以Nova为例，记录学习OpenStack的过程。

1、在/etc/nova/目录下查看api-paste.ini文件。该文件即描述Nova API相关配置。

一个配置文件后缀为ini，内容被分为很多段（section），PD只关心带有前缀的段，比如`[app:main]`或者`[filter:errors]`，总的来说，一个section的标识就是`[type:name]`,不是这种类型的section将会被忽略。
一个section的内容是以键=值来标示的。#是一个注释。在段的定义中，有以下几类：

`[app:main]`:定义WSGI应用，main表示只有一个应用，有多个应用的话main改为应用名字

`[server:main]`:定义WSGI的一个server。

`[composite:xxx]`：表示需要将一个请求调度定向（dispatched）到多个,或者多种应用上。以下是一个简单的例子，例子中，使用了composite，通过urlmap来实现载入多应用。
`[fliter:]`：定义“过滤器”，将应用进行进一步的封装。

`[DEFAULT]`：定义一些默认变量的值

详细内容参考：
[http://www.cnblogs.com/Security-Darren/p/4087587.html](http://www.cnblogs.com/Security-Darren/p/4087587.html)

----------

接下来详细看下api-paste.ini文件：

    [composite:osapi_compute]
    use = call:nova.api.openstack.urlmap:urlmap_factory
    /: oscomputeversions
    /v1.1: openstack_compute_api_v2
    /v2: openstack_compute_api_v2
    /v2.1: openstack_compute_api_v21
    /v3: openstack_compute_api_v3

以上Section定义了一个名为`osapi_compute`的composite,`use=call:nova.api.openstack.urlmap:urlmap_factory`
指定了该composite app的入口为`nova.api.openstack.urlmap`中的`urlmap_factory`。该方法的入参是一段REST API的URL。
接着`/:, /v1.1:, /v2:, /v3:`分别指定了该字符串开头的URL使用不同的app处理。如`/v3`使用`openstack_compute_api_v3`处理。


----------

    [composite:openstack_compute_api_v21]
    use = call:nova.api.auth:pipeline_factory_v21
    noauth = request_id faultwrap sizelimit noauth osapi_compute_app_v21
    keystone = request_id faultwrap sizelimit authtoken keystonecontext osapi_compute_app_v21
以上composite即v2.1版本API的处理composite app。该composite使用到`pipeline_factory`。pipeline是为app指定一串filter的一种用法。如noauth对应的pipeline即为`request_id faultwrap sizelimit noauth osapi_compute_app_v21`,最后一个`osapi_compute_app_v21`为业务处理的app。前面`request_id faultwrap sizelimit noauth`都为filter。靠近app的filter先执行。

----------

    [app:osapi_compute_app_v21]
    paste.app_factory = nova.api.openstack.compute:APIRouterV21.factory
声明一个app的入口除了使用`use=XXX`外，还有`paste.xxx_factory`的方式可以用来指定application的入口,其中paste.xxx_factory称为protocol。

注：当前比较常用的potocol 有：`paste_app_factory`，`paste.filter_factory`，`paste.compsite_factory`，`paste.filter_app_factory`, `paste.server_factory`，用来分别上述的6种section指定入口。



----------
总结下Nova API wsgi app的调用关系图

[<img src="{{ site.baseurl }}/images/openstack/api-paste.ini调度.jpg"/>]({{ site.baseurl }}/)




    