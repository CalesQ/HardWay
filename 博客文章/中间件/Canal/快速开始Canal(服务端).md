@[TOC]
## 准备

* 对于自建 MySQL , 需要先开启 Binlog 写入功能，配置 binlog-format 为 ROW 模式，my.cnf 中配置如下：
	
		[mysqld]
		log-bin=mysql-bin # 开启 binlog
		binlog-format=ROW # 选择 ROW 模式
		server_id=1 # 配置 MySQL replaction 需要定义，不要和 canal 的 slaveId 重复
		
	- 注意：针对阿里云 RDS for MySQL , 默认打开了 binlog , 并且账号默认具有 binlog dump 权限 , 不需要任何权限或者 binlog 设置,可以直接跳过这一步

* 授权 canal 链接 MySQL 账号具有作为 MySQL slave 的权限, 如果已有账户可直接 grant：
		
		CREATE USER canal IDENTIFIED BY 'canal';  
		GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
		-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
		FLUSH PRIVILEGES;
		
## 启动

* 下载 canal, 访问 release 页面 , 选择需要的包下载, 如以 1.0.17 版本为例

		wget https://github.com/alibaba/canal/releases/download/canal-1.0.17/canal.deployer-1.0.17.tar.gz
		
* 解压缩

		mkdir /tmp/canal
		tar zxvf canal.deployer-$version.tar.gz  -C /tmp/canal
		
	- 解压完成后，进入 /tmp/canal 目录，可以看到如下结构：
		
			drwxr-xr-x 2 jianghang jianghang  136 2013-02-05 21:51 bin
			drwxr-xr-x 4 jianghang jianghang  160 2013-02-05 21:51 conf
			drwxr-xr-x 2 jianghang jianghang 1.3K 2013-02-05 21:51 lib
			drwxr-xr-x 2 jianghang jianghang   48 2013-02-05 21:29 logs
			
* 配置修改

		vi conf/example/instance.properties
		
		## mysql serverId
		canal.instance.mysql.slaveId = 1234
		#position info，需要改成自己的数据库信息
		canal.instance.master.address = 127.0.0.1:3306 
		canal.instance.master.journal.name = 
		canal.instance.master.position = 
		canal.instance.master.timestamp = 
		#canal.instance.standby.address = 
		#canal.instance.standby.journal.name =
		#canal.instance.standby.position = 
		#canal.instance.standby.timestamp = 
		#username/password，需要改成自己的数据库信息
		canal.instance.dbUsername = canal  
		canal.instance.dbPassword = canal
		canal.instance.defaultDatabaseName =
		canal.instance.connectionCharset = UTF-8
		#table regex
		canal.instance.filter.regex = .\*\\\\..\*
		
	- canal.instance.connectionCharset 代表数据库的编码方式对应到 java 中的编码类型，比如 UTF-8，GBK , ISO-8859-1
	- 如果系统是1个 cpu，需要将 canal.instance.parser.parallel 设置为 false	

* 启动
		sh bin/startup.sh

* 查看server日志
		vi logs/canal/canal.log</pre>

* 查看instance的日志
		2013-02-05 22:45:27.967 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## start the canal server.
		2013-02-05 22:45:28.113 [main] INFO  com.alibaba.otter.canal.deployer.CanalController - ## start the canal server[10.1.29.120:11111]
		2013-02-05 22:45:28.210 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## the canal server is running now ......

* 关闭
		sh bin/stop.sh