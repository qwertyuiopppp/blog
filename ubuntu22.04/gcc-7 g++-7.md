# ubuntu 22.04 安装gcc-7、g++-7
```
vim /etc/apt/sources.list
deb [arch=amd64] http://archive.ubuntu.com/ubuntu focal main universe
apt-get update
apt-get -y install gcc-7 g++-7
update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 60 \
                         --slave /usr/bin/g++ g++ /usr/bin/g++-7 
update-alternatives --config gcc
gcc --version
g++ --version
```