```bash
docker run --name minio \
    -d \
    --env MINIO_ROOT_USER="root" \
    --env MINIO_ROOT_PASSWORD="admin123" \
    --publish 9000:9000 \
    --publish 9001:9001 \
    --volume /data/minio:/data \
    bitnami/minio:latest
```
> [查看镜像版本列表](https://hub.docker.com/r/bitnami/minio/tags)