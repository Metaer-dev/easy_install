# EASY INSTALL

[easy-install.py](easy-install.py) 是一键安装 frappe 的脚本文件。中文文档不会详细介绍该脚本的用途，只会介绍最快上手的路径，更详细内容请查看 [英文文档](README.md)。

***

#### 几个问题

先集中回答几个有关 `deploy` 问题：

1. 本脚本已最少依赖 github 了，但是 gitee 有个问题没法解决。所以如果是 github 连接问题：进入脚本目录（默认是 `easy_install`）然后 `git clone -b metaer https://gitee.com/iniself/frappe_docker.git`
2. `deploy` 用到的 compose 文件大量使用了腾讯镜像，但这些镜像目前属于私有镜像。所以查找 `overrides` 目录下有 `tencent` 的镜像并修改成 dockerhub 镜像。
3. `deploy`成功后访问网站出错。该脚本默认暴露的是 `8080`。假设站点是`dev.localhost`，那么访问的地址应该是`dev.localhost:8080`
4. `deploy`默认会给站点安装`drive`，`dataq`，`insights`。但目前`insights`会影响站点初始化，所以请带 `--app drive,dataq` 进行部署。
```sh
$ python3 easy-install.py deploy \
    --image your_custom_image \
    --version latest\   
    --project your_project\
    --email your_email\ 
    --sitename your_site\ 
    --app drive\
    --app dataq
```
站点初始化成功后，可以再进入 backend 容器进行 `insights` 的安装：`bench --site your_site install-app insights`

***

#### 前置要求
1. 首先你的系统需要有最新的 Python 环境。
2. `docker`在系统是 linux 时也不是必须的，因为脚本会自动安装 docker。当系统是 `windows` 或则 `macos` 时，则需要提前准备好 docker
3. `git` 最好有

#### 获取脚本
1. `git clone` 或则下载 zip
3. 进入 clone 目录，或则解压 zip 后的目录，然后继续以下内容

#### 脚本功能

该脚本有生产环境部署、开发环境部署、构建镜像、更新等功能。可以通过下面命令获得帮助：

```sh
$ python3 easy-install.py --help
```

通过下面命令可以获得某个子命令的帮助

```sh
$ python3 easy-install.py --deploy --help
```

**生产环境部署**

```sh
$ python3 easy-install.py deploy\ 
    --image your_custom_image\ 
    --version latest\   
    --project your_project\ 
    --email your_email\ 
    --sitename your_site \
    --no-ssl
```

该脚本会部署一个不使用前端代理的生产环境，如果需要使用 `ctraefik` 作为代理，并且自动获取 https 证书的环境，则如下：

```sh
$ python3 easy-install.py deploy \
    --image your_custom_image\
    --version latest\   
    --project your_project\ 
    --email your_email\ 
    --sitename your_site 
```

如果希望部署后同时安装 app 到站点上，则如下的传入会安装 `drive` 和 `insights`，否则默认安装：`drive`, `insights`, `dataq`

```sh
$ python3 easy-install.py deploy \
    --image your_custom_image \
    --version latest\   
    --project your_project\
    --email your_email\ 
    --sitename your_site\ 
    --app drive\
    --app insights
```

**镜像构建**

```sh
$ python3 easy-install.py build \
	--tag=your_custom_image:latest
```

以上会默认构建有 `drive`, `dataq`，`insights` 这三个 app 的镜像，如果需要定义自己 app 的镜像，请自行修改 `frappe_docker/development/apps-example.json`，或则指定自己的 apps.json，并传入该文件路径，如下：

```sh
$ python3 easy-install.py build \
	--tag=your_custom_image:latest \
	--apps-json=apps.json \
```


**镜像构建并发布**

不建议这样做，最佳实践还是 `镜像构建 -> 推送镜像 -> 生产环境部署` 分开进行，当然如果实在要一步完成，则如下，详细内容见 [英文文档](README.md)

```sh
$ python3 easy-install.py build \
	--tag=your_custom_image:latest \
	--push \
	--image=your_custom_image \
	--version=latest \
	--deploy \
	--project=your_project \
	--email=your_email \
	--apps-json=apps.json \
	--app=wiki
```

**项目更新**

中文略，见 [英文文档](README.md)