官方部署文档：<https://docs.gitlab.cn/jh/install/>。
一键部署：
```bash
GITLAB_DATA_DIR='/data/gitlab'
docker run --detach \
  --privileged=true \
  --hostname <访问域名> \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_DATA_DIR/config:/etc/gitlab \
  --volume $GITLAB_DATA_DIR/logs:/var/log/gitlab \
  --volume $GITLAB_DATA_DIR/data:/var/opt/gitlab \
  --shm-size 256m \
  registry.gitlab.cn/omnibus/gitlab-jh:latest
```
备份脚本：
```bash
#!/bin/bash
# 备份恢复文档：
# https://docs.gitlab.cn/jh/raketasks/backup_restore.html
set -e
set -x

/usr/bin/docker exec gitlab gitlab-backup create
[ $? -eq 0 ] && echo "创建备份文件成功"

backupDir="/data/gitlab/data/backups"
backupFile=`ls -t ${backupDir} | head -n1`
backupFilePath="${backupDir}/${backupFile}"
echo "备份文件路径: ${backupFilePath}"
/usr/local/bin/ossutil cp ${backupFilePath} oss://devops/backup/gitlab/${backupFile}
[ $? -eq 0 ] && echo "上传备份文件到 oss 成功" && rm -rf ${backupDir}/*
```
> 此脚本是使用 `ossutil` 将备份文件上传到阿里云 oss 以备份，所以 `ossutil` 需自行安装配置好。