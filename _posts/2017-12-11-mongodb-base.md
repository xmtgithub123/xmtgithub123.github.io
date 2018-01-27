---
layout: post
title: "MongoDB 简单命令行操作"
author: "MengTing Xu"
category: db
tags: [documentation,sample]
image:
  feature: cutting.jpg
  teaser: cutting-teaser.jpg
  credit:
  creditlink: ""
---

首先是下载Mongodb进行安装、配置、启动。网上都是安装与配置教程，这里就不多说，直接做个mongodb的基本命令行的笔记。

通过某些配置后，可以在cmd中输入services.msc打开服务面板。手动开启Mongodb服务，如下：

<img src="http://ozc5dgoun.bkt.clouddn.com/mongodb%E5%BC%80%E5%90%AF.png" alt="">

正在开启中...

<img src="http://ozc5dgoun.bkt.clouddn.com/mongodb%E5%90%AF%E5%8A%A8%E4%B8%AD.png" alt="">

开启之后，cmd中输入mongo启动mongodb ，开启mongo终端，这时候你就可以输入简单的命令来对mongodb进行操作。

<img src="http://ozc5dgoun.bkt.clouddn.com/mongodbBin.png" alt="">

### 1.创建数据库

如果数据库存不存在，则创建数据库，否则切换到指定的数据库：
<pre>
	> use xmtdb
	switched to db xmtdb
</pre>

### 2.删除数据库

删除当前数据库，默认为test
<pre>db.dropDatabase()</pre>

例：删除指定数据库xmtdb

1.查看所有数据库

<pre>> show dbs</pre>

2.切换到数据库xmtdb

<pre>> use xmtdb</pre>

3.执行删除命令:

<pre>> db.dropDatabase()</pre>

4.最后，通过show dbs命令来查看数据库是否删除成功了


### 3.查看数据库

<pre>> show dbs</pre>

<span style="color:red;">注：</span>
刚新创建的数据库xmtdb,如果查看数据库xmtdb时，并没有在数据库列表中，要显示它，则需要向xmtdb数据库插入一些数据。

<pre>
> db.xmtdb.insert({"name":"嘻嘻哈哈徐小婷"})
WriteResult({"nInserted":1})
//这时候 > show dbs 时就有刚新创建的数据库
</pre>

### 4.创建集合

<pre>
方法1. db.createCollection('xmt')
方法2. db.xmt.insert({name:'xmt'})
</pre>

### 5.查看集合

<pre>> show collections</pre> 
在数据库xmtdb中，插入一个集合xmt(新建一个集合=新建一个表)

### 6.删除集合

删除xmt这个集合

<pre>> db.xmt.drop()</pre>

### 7.向集合中插入文档

<pre>
>db.xmt.insert({userName:'anna',userAge:20,class:{name:'classA',numb:20}})
>db.xmt.insert({userName:'tj',userAge:25,class:{name:'classB',numb:30})
</pre>

### 8.删除文档 - 删除集合中的userName 为 anna的一条数据

<pre>db.xmt.remove({userName:'anna'})</pre>

### 9.更新文档 - 修改xmt集合中名字为anna的数据用户年龄改为25

<pre>db.xmt.update({userName:'anna'},{$set:{userAge:25}})</pre>

* 修改子文档里面的内容 - 修改用户名为anna的那条数据的班级名称为ClassC

<pre>db.xmt.update({'userName':'anna'},{$set:{'class.name':'ClassC'}})</pre>

### 10.查看文档

(1).查看某个集合的内容

<pre>db.xmt.find()</pre>

(2).查看格式化后的数据

<pre>db.xmt.find().pretty()</pre>

(3).查看第一条数据

<pre>db.xmt.findOne()</pre>

(4).查看子文档

<pre>db.xmt.find({'class.name':'classB'})</pre>

(5).查找年龄大于20的数据

<pre>db.xmt.find({age:{$gt:20}})</pre>

(6).查看年龄小于20的数据

<pre>db.xmt.find({age:${lt:20}})</pre>

具体可以查看菜鸟文档[http://www.runoob.com/mongodb](http://www.runoob.com/mongodb)




