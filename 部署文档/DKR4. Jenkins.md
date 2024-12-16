> Dockerhub 官方仓库：<https://hub.docker.com/r/jenkins/jenkins>。

创建数据目录，容器中 Jenkins 进程使用的 uid 是 1000，所以要修改下数据目录的属主属组：
```sh
$ mkdir ./jenkins-data && chown -R 1000.1000 ./jenkins-data
$ docker run -d --name jenkins \
	-p 8080:8080 \
    -p 50000:50000 \
	-v ./jenkins-data:/var/jenkins_home \
	jenkins/jenkins:2.489
```

进容器查看管理员（`admin`）密码：
```sh
$ docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```