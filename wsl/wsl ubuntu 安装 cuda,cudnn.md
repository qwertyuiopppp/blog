
# 安装cuda
1. <a href = 'https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local'>下载地址</a>

1. 根据base Installer 安装cuda

2. 配置环境
```shell
echo "export PATH=/usr/local/cuda/bin:$PATH" >> ~/.zshrc
echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PAT" >> ~/.zshrc
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PAT
source ~/.zshrc 
```

1. 查看成功
```shell
nvcc -V
```
![image](https://github.com/qwertyuiopppp/images_repository/blob/main/1.png?raw=true)

```shell
nvidia-smi
```

wsl2启动时无法添加自启动和加载环境变量的解决办法
因为每次linux启动，都会去执行.bashrc，所以直接在linux启动时添加在启动语句里面，让他在启动后配置dockerhost，并生效环境变量

在.bashrc最后一行添加
source .bashrc

![image](https://github.com/qwertyuiopppp/images_repository/blob/main/2.png?raw=true)
出错了

1. 解决

``` shell
cp /usr/lib/wsl/lib/nvidia-smi /usr/bin/nvidia-smi
chmod ogu+x /usr/bin/nvidia-smi
```

![image](https://github.com/qwertyuiopppp/images_repository/blob/main/3.png?raw=true)

# 安装cudnn

1. <a href='https://developer.nvidia.com/rdp/cudnn-archive'>下载地址</a>

2. 我在Windows中下载的 需要拷贝到wsl中
``` shell
cp /mnt/c/Users/zjm/Downloads/cudnn-linux-x86_64-8.9.4.25_cuda11-archive.tar.xz ./
tar -xvf cudnn-linux-x86_64-8.9.4.25_cuda11-archive.tar.xz
sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

## 验证是否可以GPU加速
```shell
pip3 install torch torchvision torchaudio
```

进入python输入
```python
import torch
print(torch.cuda.is_available())
```

输出为 True 即表示 CUDA GPU 加速成功


