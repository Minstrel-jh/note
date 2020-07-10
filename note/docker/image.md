[docker]: /note/docker/README.md

# 镜像

[返回][docker]

## 列出本地镜像

```bash
docker images
```

* REPOSITORY：表示镜像的仓库源
* TAG：镜像的标签
* IMAGE ID：镜像ID
* CREATED：镜像创建时间
* SIZE：镜像大小

## 查找镜像

```bash
docker search <镜像名>
```

## 下载镜像

```bash
docker pull <镜像名>
```

## 删除镜像

```bash
docker rmi <镜像名>
```
