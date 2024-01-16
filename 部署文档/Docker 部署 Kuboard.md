> 官方文档：<https://kuboard.cn/install/v3/install-built-in.html>。

```bash
sudo docker run -d \
  --restart=unless-stopped \
  --name=kuboard \
  -p 8001:80/tcp \
  -p 10081:10081/tcp \
  -e KUBOARD_ENDPOINT="http://192.168.2.231:80" \
  -e KUBOARD_AGENT_SERVER_TCP_PORT="10081" \
  -v /data/kuboard:/data \
  swr.cn-east-2.myhuaweicloud.com/kuboard/kuboard:v3
```