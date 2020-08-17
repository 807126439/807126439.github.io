### docker pull X509证书问题



#### docker pull docker-hub.tools.huawei.com镜像报错，修复办法(x509: certificate is valid for avenue3, not docker-hub.tools.huawei.com)



从报错原因上面来看：主要是因为该仓库地址：没有通过某种认证（具体原因是因为我们docker 默认是不支持使用http协议和私有仓库之间进行通信，但是搭建的私有藏库默认是没有配置docker.domain.com颁发的SSL证书的，所以需要在docker的客户端做一下特殊的配置，告诉docker我们私有仓库站点是可以进行通信的）



**过程分析：**运行 docker info 命令，发现在Insecure Registries 以下字段没有我们的docker仓库，导致使用docker pull命令的时候需要进行验证，导致基础镜像下载失败 



**解决办法**（将docker-hub.tools.huawei.com添加到信任仓库）：

找到如下配置文件：

/etc/docker/daemon.json

如果没有，则自行创建文档

进行如下修改（增加需要免认证的仓库地址）：

{"insecure-registries":["sz-docker-public.szxy1.artifactory.cd-cloud-artifact.tools.huawei.com","docker-hub.tools.huawei.com"]}

修改后进行保存。然后使用以下命令进行重启docker服务

service docker restart

docker服务重启之后，使用

docker info

查看可以看到Insecure Registries中新增了如下仓库，说明已经创建成功：



再次使用 docker pull命令可以看到已经可以正常下载镜像到本地



！！！实际进行！！！发现没有用处。。。。原因估计为系统版本或者docker版本不对，daemon.json并没有读取配置



### 经测试使用方法二

修改：/etc/systemd/system/docker.service

**--insecure-registry XXXXXXXX**

**重启docker：**

**systemctl daemon-reload**

**service docker restart**

问题解决
