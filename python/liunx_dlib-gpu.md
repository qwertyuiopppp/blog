需要装好cuda 和cudnn

我用的系统是wsl ubutun22.04 gcc版本太高 需要将gcc降成7.5的才可以
否则会报错误CUDA was found but your compiler failed to compile a simple CUDA program so dlib isn't going to use CUDA

echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu focal main universe" >> file.txt
```shell
echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu focal main universe" >> /etc/apt/sources.list
apt-get update
apt-get -y install gcc-7 g++-7
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 60 \
                         --slave /usr/bin/g++ g++ /usr/bin/g++-7 
update-alternatives --config gcc
gcc --version
g++ --version
```
一切准备好后开始下载编译

``` shell
git clone https://github.com/davisking/dlib.git
cd dlib
mkdir build
cd build
cmake .. -DDLIB_USE_CUDA=1 -DUSE_AVX_INSTRUCTIONS=1
cmake --build .
cd..
python3 setup.py install
```

验证
``` shell
python3
```
```python
import dlib
dlib.DLIB_USE_CUDA
```

在这一步输出true，ok啦！
