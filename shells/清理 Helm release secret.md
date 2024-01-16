每一次 Helm upgrade 都会在命名空间下新生成一个 secret，长久积累下来非常占资源，可以使用此脚本批量清理：
```bash
#!/bin/bash
IFS=$'\n'
for line in $(kubectl get secrets -A | awk '$1 ~ /service/ && $2 ~ /sh\.helm\.release\.v1/{print $1, $2}');do
    ns=$(echo ${line} | awk '{print $1}');
    secret_name=$(echo ${line} | awk '{print $2}');
    echo "namespace: ${ns}, secret name: ${secret_name}";
    kubectl delete secret ${secret_name} -n ${ns};
done
```