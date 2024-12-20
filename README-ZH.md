# EASY INSTALL

[easy-install.py](easy-install.py) 是一键安装 frappe 产品的脚本文件。中文文档不会详细介绍该脚本的用途，只会介绍最快上手的路径。

#### 前置要求
1. 首先你的系统需要有最新的 python 环境。
2. `Docker`在系统是 linux 时也不是必须的，因为脚本会自动安装 Docker。但你的系统时 `windows` 或则 `macos` 时，则需要你提前准备好 Docker
3. `git` 最好有

#### 获取脚本
1. `git clone`
2. 下载 zip
3. 进入 clone 目录，或则解压 zip 后的目录，然后继续以下内容

#### 脚本功能

该脚本有生产环境部署、开发环境部署、构建镜像、更新等功能。你可以通过下面命令获得帮助

```sh
$ python3 easy-install.py --help
```

通过下面命令你可以获得某个子命令的帮助

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

该脚本会部署一个不使用前端代理的生产环境，如果你需要使用 `ctraefik` 作为代理，并且自动获取 https 证书的环境，则如下：

```sh
$ python3 easy-install.py deploy \
    --image your_custom_image\
    --version latest\   
    --project your_project\ 
    --email your_email\ 
    --sitename your_site 
```

如果希望部署同时安装 app 到站点上，则如下传入会安装 `drive` 和 `insights`，否则默认安装：`drive`, `insights`, `dataq`

```sh
$ python3 easy-install.py deploy \
    --image your_custom_image \
    --version latest\   
    --project your_project\
    --email your_email\ 
    --sitename your_site\ 
    --app drive,insights
```

**镜像构建**

```sh
$ python3 easy-install.py build \
	--tag=your_custom_image:latest
```

以上会默认构建有 `drive`, `dataq`，`insights` 的镜像，如果你需要自定义自己的镜像，请自行修改 `frappe_docker/development/apps-example.json`，或则指定自己的 apps.json，并传入该文件路径，如下：

```sh
$ python3 easy-install.py build \
	--tag=your_custom_image:latest \
	--apps-json=apps.json \
```


**镜像构建并发布**

作者不建议你这样做，还是建议 镜像构建 -> 推送镜像 -> 生产环境部署 这三部分开进行，当然如果你实在要一步完成，则如下，详细内容见 [英文文档](README.md)

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