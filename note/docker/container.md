[docker]: /note/docker/README.md

# 容器

[返回][docker]

## 列出容器

```bash
docker ps [OPTIONS]

OPTIONS:
-a, --all                                               展示所有容器(默认只显示运行的)
```

## 创建容器

```bash
docker run
```

## 开启/停止

```bash
docker start <container>
docker stop <container>
```

## 删除容器

```bash
docker rm [OPTIONS] <container>

OPTIONS:
-v                                                      删除关联的卷
```

## 进入容器内部

```bash
docker exec -it <container> bash                        进入容器
```

## 显示容器的信息

```bash
docker inspect <container>
```
