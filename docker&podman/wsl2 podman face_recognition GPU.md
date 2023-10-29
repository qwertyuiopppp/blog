# 下载face_recognition dockerfile
我开始用的docker 但是docker太吃内存了程序跑着跑着就把docker搞崩了，害得我下载的docker镜像都不见了贼烦，后面使用podman发现内存小了很多，且podman兼容docker的命令还不错，可以使用docker的生态


1. 新建一个文件夹face_recognition_base创建Dockerfile文件，也可以去 https://github.com/ageitgey/face_recognition/blob/master/docker/gpu/Dockerfile 下载官方给出的Dockerfile需要修改，不能直接用

    我自己的Dockerfile如下
    ```dockerfile
    FROM nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04  AS compile

    # Install dependencies
    ENV DEBIAN_FRONTEND=noninteractive
    RUN apt-get update -y && apt-get install -y \
        git \
        cmake \
        libsm6 \
        libxext6 \
        libxrender-dev \
        python3 \
        python3-pip \
        python3-venv \
        python3-dev \
        python3-numpy \
        build-essential \
        gfortran \
        wget \
        curl \
        graphicsmagick \
        libgraphicsmagick1-dev \
        libatlas-base-dev \
        libavcodec-dev \
        libavformat-dev \
        libgtk2.0-dev \
        libjpeg-dev \
        liblapack-dev \
        libswscale-dev \
        pkg-config \
        software-properties-common \
        zip \
        && apt-get clean && rm -rf /tmp/* /var/tmp/*

    RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu focal main universe" >> /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get -y install gcc-7 g++-7
    RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 60 \
                            --slave /usr/bin/g++ g++ /usr/bin/g++-7
    RUN update-alternatives --config gcc

    # Virtual Environment
    ENV VIRTUAL_ENV=/opt/venv
    RUN python3 -m venv $VIRTUAL_ENV
    ENV PATH="$VIRTUAL_ENV/bin:$PATH"

    # Scikit learn
    RUN pip3 install --upgrade pip && \
        pip3 install scikit-build

    # Install dlib
    ENV CFLAGS=-static
    RUN git clone  --single-branch https://ghproxy.com/https://github.com/davisking/dlib.git dlib/ && \
        mkdir -p /dlib/build && \
        cmake -H/dlib -B/dlib/build -DDLIB_USE_CUDA=1 -DUSE_AVX_INSTRUCTIONS=1 && \
        cmake --build /dlib/build && \
        cd /dlib && \
        python3 /dlib/setup.py install --set BUILD_SHARED_LIBS=OFF

    # Install face recognition
    RUN pip3 install face_recognition -i https://pypi.tuna.tsinghua.edu.cn/simple

    ```


2. 执行命令
    ``` shell
    podman build -t my-face-recognition-base .
    ```

3. 构建成功后查看镜像
    ``` shell
    podman images
    ```


4. 根据镜像运行个容器
    ```shell
    podman run -i --name my-face-recognition  -t my-face-recognition-base /bin/bash
    ```

5. 验证
    ``` shell
    python3
    ```
    ```python
    import dlib
    dlib.DLIB_USE_CUDA
    ```
    结果为True表示成功，按ctrl+p+q退出容器

