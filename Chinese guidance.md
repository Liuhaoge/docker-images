# 开箱即用的AI和数据科学平台
1. QPod将必要的安装包与开发环境封装为docker镜像，您无需进行环境配置即可开启一个AI和数据科学的工程，同时能够方便地复制与分享您的工程数据与研究结果。
2. Qpod can easily scale-up and scale-out your algorithms and key innovations - QPod help you move forward smoothly from the development stage to deployment stage by re-using these images to either to provide RESTful APIs or orchestrate map/reduce operations on big data.
3. Qpod具备一系列拥有交互计算环境的Docker镜像，您可以在Qpod中的Jupyter Notebook中运行Python, R, OpenJDK, NodeJS, Go, Julia, Octave等语言。QPod也支持运行VS Code, R-Studio等工具。

# 使用手册
0. 安装docker
    -  CPU用户：请安装 docker-ce ( [Linux](https://hub.docker.com/search/?offering=community&type=edition&operating_system=linux) | [macOS](https://download.docker.com/mac/stable/Docker.dmg) | [Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) ) 或 [docker-ee](https://hub.docker.com/search/?offering=enterprise&type=edition) 
    -  GPU用户：需使用Linux服务器，在安装Docker之后还需安装[NVIDIA driver](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#how-do-i-install-the-nvidia-driver)与最新版的[nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#quickstart)
1. 选择功能集合与安装地址
    -  从主页的QPod功能集合中选择一项下载，若磁盘空间与网速满足要求则推荐CPU用户选择`full`，GPU用户选择`full-cuda`
    -  选择安装的基础目录，请使用绝对路径（如`/root`,`/User/me`,`D:/work`）
2. 开始使用容器

    根据您的操作系统运行下方脚本，将其中的`IMG`与`WORKDIR`改为自己的安装配置。运行过程中关闭Jupyter以及其他占用8888或9999端口的程序。
    
    Linux与macOS用户，在bash或终端中运行：
    ```
    IMG="qpod/qpod:full"
    WORKDIR="/root"  # <- macOS change this to /Users/your_user_name

    docker run -d --restart=always \
        --name=QPod \
        --hostname=QPod \
        -p 8888:8888 -p 9999:9999 \
        -v $WORKDIR:/root \
        $IMG
    sleep 10s && docker logs QPod 2>&1|grep token=
    ```
   ⚠️ 使用带有`nvidia-docker`的NVIDIA GPU时请注意:
    - 👉 确保Docker版本在19.03以上并能够运行`nvidia-smi`指令
    - 👉 在`docker run`指令中增加一项: --gus all (位于--restart=always之后，旧nvidia-container版本中使用 --runtime nvidia)
    - 👉 使用IMG="qpod/qpod:full-cuda"或其他支持cuda的镜像

    Windows用户，在终端或CMD中运行：
    ```
    SET IMG="qpod/qpod:full"
    SET WORKDIR="D:/work"

    docker run -d --restart=always ^
        --name=QPod ^
        --hostname=QPod ^
        -p 8888:8888 9999:9999 ^
        -v %WORKDIR%:/root ^
        %IMG%
    timeout 10 && docker logs QPod 2>&1|findstr token=
    ```
3. 等待几分钟后进行登录

    以上指令会下载docker镜像，建立一个名为QPod的docker容器，并生成一条含有48位十六进制密钥的URL地址。
    
    复制`?token=`之后的密钥，进入[http://localhost:8888](http://localhost:8888)或[http://ip-address:8888](http://ip-address:8888)并粘贴密钥，即可开始使用。

# 附加信息
## FAQ
关于FAQ系列问题请参加[此处](https://github.com/QPod/docker-images/wiki)
## 硬件
镜像需建立在`ubuntu:latest`且只能在×86平台测试，ppc64le平台预计会进行少量修改。
## 安装包管理
不建议使用`conda`来安装lib或package，鉴于conda无法复用已有的系统库且无法提供稳定的Liunx系统存储库。
## 定制化
若发现有系统库、Python模块或R包的缺失，可在`work`文件夹的`install_XX.list`中进行补充。



