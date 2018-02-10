一个基于 Docker 的 LNMPA 环境
---

### 使用

- 克隆或下载源码

	```bash
	➜  git clone https://github.com/whorusq/docker-lnmpa.git lnmpa
	```

- 替换 www 目录

	```bash
	➜  cd lnmpa 
	➜  mv www www_bak
	➜  ln -s /home/user1/www www
	```

- 初始化启动

	```bash
	# 此过程将初始化 php_apache、mysql、nginx，并前台启动服务
	➜  docker-compose up
	```

	> 如果整个过程出现 error ，初始化将终止，可针对错误信息进行调整。
	>
	> 正常启动后，目录下将生成 
	>
	> - ./data MySQL 数据文件 
	> - ./log 所有日志文件

- 后台启动

	```bash
	➜  docker-compose up -d
	```