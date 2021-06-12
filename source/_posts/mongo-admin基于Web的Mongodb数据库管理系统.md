---
title: mongo-admin基于Web的Mongodb数据库管理系统
date: 2021-05-30 12:22:47
tags:

---

#### 1.基本介绍

该项目是基于Web的Mongodb数据库管理工具。

演示地址：http://47.98.136.116:8080/

![](https://supers1.oss-cn-hangzhou.aliyuncs.com/20210526220734.png)
<!--more-->
![](https://supers1.oss-cn-hangzhou.aliyuncs.com/20210526220756.png)![](https://supers1.oss-cn-hangzhou.aliyuncs.com/20210526220823.png)![](https://supers1.oss-cn-hangzhou.aliyuncs.com/20210526220907.png)

#### 2. 功能点

1. github第三方权限认证
2. 连接Mongodb数据库（可以切换数据的连接单连接）
3. 展示Mongodb数据库内的数据库信息（**过滤admin和local数据库**）
4. 展示Mongodb数据库内的集合信息（**过滤系统集合system.***)
5. 展示集合内的所有文档信息
6. **插入文档**，**更新文档**，**删除文档**，**导出文档**
7. 嵌入式的mongo shell命令行工具CodeMirror（**支持$find, $sort等**）
8. Gridfs文件存储，支持**下载，删除**

#### 3. 如何使用

1. `git clone git@github.com:ammi3/mongo-admin.git`
2. 更改**application.yml**内的mongodb数据库连接信息
3. 前往Github-settings-Developer settings-OAuth-apps创建一个App，将Ip换成127.0.0.1

<img src="https://supers1.oss-cn-hangzhou.aliyuncs.com/20210530122953.png" style="zoom:50%;" />

4. 在RestAuthController.java更换Client Id与Client secrets
5. 启动
