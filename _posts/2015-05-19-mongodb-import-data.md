---
layout: post
title: mongodb数据导入
share: true
comments: true
imagefeature:
tags: [工作,mongodb]
category: work,mongodb
description: "Import data to mongodb"
---

任务：从Oracle中导出一份用户数据作初始化mongodb。

<!--more-->

##数据准备

* 写好sql放进Oracle中执行，导出格式选择json,csv
		
		customer_json.dat
		customer_csv.dat
		
* 为了方便查询、兼容现有程序，修改字段名称.

	**json:**

	![][1]

	**csv:**

	![][2]
	
##使用json格式

		./mongoimport -p password -u username -d workDB -c customer  customer_json.dat
		
##使用csv格式
	
		./mongoimport -p password -u username -d workdb -c customer --type csv --headerline --file customer_csv.dat 
	
##验证
	use workdb
	db.customer.stats()
	
![][2]

[1]: http://jeffreywei.github.io/assets/posts/2015-05/mongodb_import_0.png "mongodb_import_0"

[2]: http://jeffreywei.github.io/assets/posts/2015-05/mongodb_import_1.png "mongodb_import_1"
	
[3]: http://jeffreywei.github.io/assets/posts/2015-05/mongodb_import_1.png "mongodb_import_2"







