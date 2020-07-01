---
title: docker笔记
date: 2020-07-01 00:15:56
tags:
---

## docker从入门到实践笔记

主要概念：镜像（	Image	） 容器（	Container	） 仓库（	Repository	）

### 1.获取镜像

	docker	pull	[选项]	[Docker	Registry	地址[:端口号]/]仓库名[:标签]
	docker	image	ls
	docker	image	rm	[选项]	<镜像1>	[<镜像2>	...]
	docker	exec	-it	webserver	bash 可进入容器修改内容
	docker	diff	webserver 
	docker	commit	[选项]	<容器ID或容器名>	[<仓库名>[:<标签>]]
	docker	import 获取解压缩文件作为第一层镜像
	docker	save	alpine	-o	filename 保存镜像文件
	docker	load 读取镜像文件

<!-- more -->

### 2.Dockerfile
	FROM	指定基础镜像
	RUN	<命令>	
	RUN	["可执行文件",	"参数1",	"参数2"]
	每个指令会创建一层,指令多用&&写一起
	
	docker	build	[选项]	<上下文路径/URL/->
		-例如 -t nginx:v3指定镜像名称
		上下文用. 上下文会上传
		dockerFile中路径都是上下文路径
		docker build 还可以构成远程和压缩包文件
	
	COPY	[--chown=<user>:<group>]	<源路径>...	<目标路径>		
	COPY	[--chown=<user>:<group>]	["<源路径1>",...	"<目标路径>"]
	可使用通配符
	COPY	--from=nginx:latest	/etc/nginx/nginx.conf	/nginx.conf从其他镜像复制文件

	
	ADD与COPY一样,但是ADD可以下载远程文件和解压压缩包
	
	CMD
		shell		格式：	CMD	<命令>		
		exec		格式：	CMD	["可执行文件",	"参数1",	"参数2"...]	
		参数列表格式：	CMD	["参数1",	"参数2"...]	。在指定了		
		ENTRYPOINT		指 令后，用		CMD		指定具体的参数
		
	ENTRYPOINT
		和CMD相同,存在ENTRYPOINT,docker run 输入指令为 ENTRYPOINT的参数,可用于启动动态指定命令
	
	ENV设置环境变量
		ENV	<key>	<value>		
		ENV	<key1>=<value1>	<key2>=<value2>...
		后续文件或容器都能用,文件用$XXX
	ARG设置环境变量,构建变量,容器内失效
		ARG	<参数名>[=<默认值>]
		
	VOLUME定义匿名卷,此目录数据不写入容器存储层
		VOLUME	["<路径1>",	"<路径2>"...]		
		VOLUME	<路径>
	
	EXPOSE	声明端口无实际作用,仅使用随机端口时优先使用
	
	WORKDIR	指定工作目录
		以后RUN各层起始目录转为工作目录,否则RUN每层为一容器,目录不记录
	
	USER切换用户
		USER	<用户名>[:<用户组>]
		
	ONBUILD
		ONBUILD		是一个特殊的指令，它后面跟的是其它指令，比如
		RUN	,		COPY		等， 而这些指令，在当前镜像构建时并不会被执行。
		只有当以当前镜像为基础镜像，去 构建下一级镜像的时候才会被执行
	
	manifest
		版本相关
		
### 3.docker容器操作
	docker run 
		-i 保持输入-t 打开终端 -d 后台运行,不输出到宿主机
		流程:1.检查镜像,没有则下载
			2.启动容器
			3.分配文件系统,挂载读写层
			4.网桥接口中桥接接口到容器
			5.地址池配置ip给容器
			6.执行应用程序
			7.终止
			
	docker	container	start 启动终止容器restart
	docker	container	ls  -a查看所有
	docker	container	log 查看日志
	docker	container	stop终止容器
	docker attach 进入容器exit会终止
	docker exec 进入容器exit不会终止,加-it参数
	docker export导出容器,丢失历史记录
	docker import导入容器
	docker	container	rm 删除-f 强制删除
	docker container prune 清除所有终止状态的容器
### 4.仓库
	Repository 仓库
	Registry 注册服务器,包含多个仓库,仓库包含多个镜像
	docker login登录docker hub
	docker search --filter=stars=N
	docker pull
	docker push
	docker hub有自动构建功能
	
	私有仓库
		使用registry镜像
		
		Nexus3配合nginx
			
### 5.docker数据管理
主要分：
>1.数据卷
>2.挂载主机目录
>3.内存
		
	创建数据卷
	docker	volume	create	my-vol
	docker	volume	ls
	docker	volume	inspect	my-vol 查看详细信息
	run使用--mount source=my-vol,target=/webapp		标记来将		数据卷		挂载到容器 里
		type=bind,也可以设置挂载本地目录,和-v不同,没目录会报错
	docker	inspect	web 查看web容器详细信息,mounts下有数据卷信息
	docker	volume	rm	my-vol
	docker	volume	prune	清理数据卷
	
### 6.网络管理
	-p 和-P 映射端口,没有后续则随机映射
	docker port 可以查看端口
	容器互联
	docker网络
	docker	network	create	-d	bridge	my-net
	-d bridge指定类型还有overlay用于Swarm	mode
	连接网络
	run --network	my-net指定网络
	
### 7.Docker	Compose
	在容器中使用mount命令可以看到挂载信息
	定位是	「定义和运行多个	Docker	容器的应用（Defining	and			running multi-container	Docker	applications）」，其前身是开源项目	Fig
	python编写
	docker-compose	--version
	docker-compose	up 运行compose项目
	-f指定文件 -p指定项目名 --x-networking使用Docker的可拔插网络后端特性
	--x-network-driver Driver指定网络后端的驱动，默认为bridge
	--verbose 输出调试信息
	docker-compose	build	[options]	[SERVICE...] 构建
	--force-rm		删除构建过程中的临时容器。
	--no-cache		构建镜像过程中不使用	cache（这将加长构建过程）
	--pull		始终尝试通过	pull	来获取更新版本的镜像
	
	config 验证文件格式
	down停止启动容器
	exec进入容器
	help
	images列出Compose	文件中包含的镜像
	kill
	logs
	pause暂停容器
	port打印某个容器端口所映射的公共端口
	ps列出目前容器
	pull拉取依赖镜像
	push推送依赖镜像到docker
	restart
	rm删除所有停止的服务容器 -f强制直接删除 -v删除挂载的卷
	run指定服务执行命令*****P243需要再看
	scale service=num指定启动容器个数
	start 启动已存在
	stop
	top查看各个容器内运行的进程
	unpause恢复暂停中的服务
	up全能启动注意看参数
	version
	
	模板	docker-compose.yml P247
### 8.docker swarm集群
	可使用docker machine进行测试
	docker swarm init 初始化
	docker	swarm	join加入集群
	管理节点docker node ls 查看集群
	docker service管理集群服务
	例子:docker	service	create	--replicas	3	-p	80:80	--name	nginx	nginx :1.13.7-alpine
	docker service ps 来查看某个服务的详情
	docker service logs	查看日志
	docker service scale nginx=5容器数量伸缩
	docker  service rm 移除swarm某个服务

	swarm使用compose文件,使用docker stack命令
	docker stack deploy -c 文件 服务
	docker stack ls查看服务
	docker stack down移除服务
	
	docker volume rm 移除数据卷
	
	敏感数据可使用|docker secret create XXX
	docker secret ls
	
	管理配置数据可用docker config
	docker config createredis.conf redis.conf
	docker config ls
	使用:--config source=redis.conf,target=/etc/redis.conf

	服务升级docker service update
	docker service rolback
### 9.docker网络实现
	--net=bridge 默认值,连接到默认的网桥
	--net=host 将容器网络放置在主机上,可以影响主机--privileged=true允许配置主机的网络
	--net=container:name or id 将容器放到一个已存在的网络栈中,和存在的容器共享ip和端口
	--net=none,不进行配置,可以自己配置连接到docker0
	
### 10.etcd	类似zookeeper

### 11.Kubernetes

	常见总结
		清理临时镜像文件docker image prune
		查看镜像支持的环境变量docker run IMAGE env
		本地镜像文件地址/var/lib/docker/	
		批量清理停止的容器docker container prune
		获取容器PID
			docker inspect --format {% raw %}'{{ .State.Pid }}'{% endraw %}	<CONTAINER ID	or	NAME>
		获取容器IP 
			docker inspect --format {% raw %}'{{ .NetworkSettings.IPAddress }}'{% endraw %} <CONT AINER ID or NAME>
		固定容器ip
			docker	network	create	-d	bridge	--subnet	172.25.0.0/16	my-net
			docker	run	--network=my-net	--ip=172.25.3.3	-itd	--name=my-con tainer	busybox
		退出交互容器
			ctrl p/q   c会终止容器
		端口不能使用
			dockerfile 没expose开放端口
			启动指定publishAllPort = true
		docker配置文件		/etc/docker/daemon.json
		可以开启debug模式
