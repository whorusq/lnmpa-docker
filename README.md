一个基于 Docker 的 LNMPA 环境
---

> 这是一个基于 Dokcer 的 LNMPA（Nginx + PHP + Apache + MySQL） 环境，借助 Docker compose 进行编译、管理，可用于本地开发及线上部署。
>
> 因为 Docker 的限制，推荐服务器系统使用 CentOS7+、Ubuntu16.04 Server +，关于 Dokcer 的安装、设置，详见 [https://github.com/whorusq/docker-learning](https://github.com/whorusq/docker-learning)

### 1. 目录结构

```
.
├── README.md
├── build   				 	<---------- 各服务 Dockerfile 文件
│   ├── mysql
│   │   └── Dockerfile
│   ├── nginx
│   │   └── Dockerfile
│   └── php_apache
│       ├── Dockerfile
│       └── source.list      	<---------- 阿里云软件更新源
├── conf
│   ├── apache
│   │   ├── apache2.conf     	<---------- Apache 服务配置
│   │   ├── ports.conf       	<---------- 监听端口配置
│   │   ├── sites-available  	<---------- Apache 站点配置目录
│   │   │   └── www.conf
│   │   └── sites-enabled    	<---------- 映射站点配置
│   │       ├── README.md
│   │       └── www.conf -> ../sites-available/www.conf
│   ├── mysql                	<---------- MySQL 自定义配置目录
│   │   └── mysql.env
│   ├── nginx
│   │   ├── nginx.conf       	<---------- Nginx 服务配置
│   │   └── vhosts           	<---------- Nginx 站点配置目录
│   │       ├── README.md
│   │       └── www.conf
│   └── php                  	<---------- PHP 自定义配置目录
│       └── php.ini
├── data                     	<---------- MySQL 数据文件目录，初始化启动后自动生成
├── docker-compose.yml       	<---------- ❗️❗️❗️Docker compose 配置文件，可根据需要修改软件版本、映射目录等
├── log                      	<---------- 日志文件目录，当产生日志时自动生成
└── www -> /Users/user1/www  	<---------- 映射本地 www 根目录
```

### 2. 使用

2.1. 克隆或下载源码

```bash
➜  git clone https://github.com/whorusq/docker-lnmpa.git lnmpa
```

2.2. 替换 www 目录

```bash
➜  cd lnmpa
➜  mv www www_bak
➜  ln -s /home/user1/www www
```

2.3. 初始化启动

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

其它常用操作命令：

```bash
# 后台启动
➜  docker-compose up -d

# 停止服务
➜  docker-compose stop

# 完全移除容器
➜  docker-compose down

# 查看启动的容器情况
➜  docker-compose ps

# 查看容器输出日志
➜  docker logs $container_name
```