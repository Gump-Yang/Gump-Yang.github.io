# lab2 

## 利用 hexo 在 Github上搭建一个自己的博客

教程：https://zhuanlan.zhihu.com/p/392994381



# lab6

## 利用docker 运行自己的镜像和容器

## 一、使用官方镜像运行容器

### 1. 拉取Nginx官方镜像

执行以下命令下载最新版Nginx镜像：

bash

复制

```bash
docker pull nginx:latest
```

### 2. 运行测试容器

bash

复制

```bash
docker run -d -p 8080:80 --name test-nginx nginx
```

参数说明：

- `-d`：后台运行容器
- `-p 8080:80`：将主机8080端口映射到容器80端口
- `--name test-nginx`：容器命名为test-nginx

### 3. 验证部署

打开浏览器访问：
http://localhost:8080
应看到Nginx欢迎页面

### 4. 容器管理命令

bash

复制

```bash
# 查看运行中的容器
docker ps

# 查看容器日志
docker logs test-nginx

# 停止容器
docker stop test-nginx

# 删除容器
docker rm test-nginx
```

------

## 二、构建自定义镜像部署博客

### 1. 准备项目文件

工程目录结构：

```

E:\CloudComputing
├─Gump-Yang.github.io
|          ├─index.html
|          ├─README.md
|          ├─js
|          | ├─jquery-3.6.4.min.js
|          | └script.js
|          ├─fancybox
|          |    ├─jquery.fancybox.min.css
|          |    └jquery.fancybox.min.js
|          ├─css
|          |  ├─style.css
|          |  ├─images
|          |  |   └banner.jpg
|          ├─archives
|          |    ├─index.html
|          |    ├─2025
|          |    |  ├─index.html
|          |    |  ├─03
|          |    |  | └index.html
|          ├─2025
|          |  ├─03
|          |  | ├─12
|          |  | | ├─lab
|          |  | | |  └index.html
|          ├─.git
|          |  ├─config
|          |  ├─description
|          |  ├─HEAD
|          |  ├─index
|          |  ├─packed-refs
|          |  ├─refs
|          |  |  ├─tags
|          |  |  ├─remotes
|          |  |  |    ├─origin
|          |  |  |    |   └HEAD
|          |  |  ├─heads
|          |  |  |   └main
|          |  ├─objects
|          |  |    ├─pack
|          |  |    |  ├─pack-06fe237be52c1fbe8aca758dffae13ae8b6d140e.idx
|          |  |    |  ├─pack-06fe237be52c1fbe8aca758dffae13ae8b6d140e.pack
|          |  |    |  └pack-06fe237be52c1fbe8aca758dffae13ae8b6d140e.rev
|          |  |    ├─info
|          |  ├─logs
|          |  |  ├─HEAD
|          |  |  ├─refs
|          |  |  |  ├─remotes
|          |  |  |  |    ├─origin
|          |  |  |  |    |   └HEAD
|          |  |  |  ├─heads
|          |  |  |  |   └main
|          |  ├─info
|          |  |  └exclude
|          |  ├─hooks
|          |  |   ├─applypatch-msg.sample
|          |  |   ├─commit-msg.sample
|          |  |   ├─fsmonitor-watchman.sample
|          |  |   ├─post-update.sample
|          |  |   ├─pre-applypatch.sample
|          |  |   ├─pre-commit.sample
|          |  |   ├─pre-merge-commit.sample
|          |  |   ├─pre-push.sample
|          |  |   ├─pre-rebase.sample
|          |  |   ├─pre-receive.sample
|          |  |   ├─prepare-commit-msg.sample
|          |  |   ├─push-to-checkout.sample
|          |  |   ├─sendemail-validate.sample
|          |  |   └update.sample
```

### 2. 创建Dockerfile

在博客根目录新建`Dockerfile`文件：

dockerfile

复制

```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

文件说明：

- 使用轻量级Alpine版Nginx
- 将当前目录所有文件复制到Nginx默认静态资源目录
- 声明容器暴露80端口

### 3. 构建镜像

在PowerShell中执行：

bash

复制

```bash
# 进入项目目录
cd E:\CloudComputing\Gump-Yang.github.io

# 构建镜像（注意最后的点）
docker build -t my-blog:1.0 .
```

### 4. 运行自定义容器

bash

复制

```bash
docker run -d -p 8081:80 --name my-blog-container my-blog:1.0
```

访问地址：
http://localhost:8081
