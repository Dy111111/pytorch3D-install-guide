# 一、说明及准备工作

最近在安装pytorch3D的时候遇到了很多问题，查了很多博客，但发现讲的都不太全，所以特将自己的及收集到的安装过程经验总结如下，本教程应该是目前最详细全面的安装教程了。我是在Anaconda中虚拟环境下安装的。
## 1.1准备工作
官方安装教程如下：[https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md](https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md)，完全按照这个教程安装可能会遇到很多问题，因此需要补充一些细节。安装这个的前提是已经安装了pytorch。
## 1.2相关包的安装
总共需要安装的包有：
- fvcore
- iopath
- cub
- scikit-image
- black
- usort
- flake8
- matplotlib
- tdqm
- jupyter
- imageio
- plotly
- opencv-python

这里面除了cub需要单独安装（后面会单独讲），其他都可以用conda直接安装。推荐用conda安装，因为我没用其他方法试过，所以不确定不用conda会出现哪些问题。具体安装过程如下：
- 首先打开cmd命令窗口，然后**创建并激活conda虚拟环境**

```powershell
conda create -n pytorch-gpu python=3.9
conda activate pytorch3d
```
- 然后依次执行下面的代码（一行一行来，复制一行粘贴然后回车等待安装完成再继续下一行，直到全部安装完成）
```powershell
conda install -c fvcore -c iopath -c conda-forge fvcore iopath
conda install jupyter
pip install scikit-image matplotlib imageio plotly opencv-python
pip install black usort flake8 flake8-bugbear flake8-comprehensions
```
<br><br>
## 1.3 cub安装配置

cub与cuda toolkit对应关系如下（图片截自[https://github.com/NVIDIA/cub](https://github.com/NVIDIA/cub)）：
![image](https://img-blog.csdnimg.cn/1a232d798ecd43499e3a28256c6e1503.png width='50%')

根据自己的cuda tookit版本选择对应的cub realase版本下载，下载地址为[https://github.com/NVIDIA/cub/releases](https://github.com/NVIDIA/cub/releases)
- 如下图所示，点击下载解压到自己想安装的位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd61ffb6202c4d6bbde2317b0f16a9e4.png#pic_center =800x)
- 解压后，添加设置环境变量，变量名CUB_HOME，变量值即为刚才解压的cub的文件路径

![在这里插入图片描述](https://img-blog.csdnimg.cn/6e446cadc89342718b9fd717f68fd541.png#pic_center =600x )
 - 设置完即安装完成。
<br><br>
 ## 1.4 MinGW安装
我是按照一些博客教程的步骤安装了这个，但我也不确定是不是一定需要安装，以防万一大家还是安装一下比较好。
- 具体安装过程可以参考这篇博客[http://c.biancheng.net/view/8077.html](http://c.biancheng.net/view/8077.html)
<br><br>
# 二、pytorch3D安装
## 2.1首先下载解压pytorch3D到想要安装的位置
下载地址:[https://github.com/facebookresearch/pytorch3d/releases](https://github.com/facebookresearch/pytorch3d/releases)注意版本要与pytorch对应，每个版本的Pytorch3d下有注明其适用的pytorch版本。
- 点击下载，然后解压到相应位置即可，如果是cuda虚拟环境下运行，建议下载到虚拟环境的Lib\site-packages目录下，比如我的虚拟环境是torch-gpu，我就把pytorch3D放到D:\software\anaconda3\envs\torch-gpu\Lib\site-packages里面，然后将解压的文件夹重命名为pytorch3D
![在这里插入图片描述](https://img-blog.csdnimg.cn/47d96599079a4d42bc5d3715cc334e96.png#pic_center =800x)
![在这里插入图片描述](https://img-blog.csdnimg.cn/31b527bb7000469cb04a546abd8d011d.png#pic_center =800x)

## 2.2 更改相关文件
打开pytorch3D文件夹，找到setup.py文件，打开，将extra_compile_args = {“cxx”: [“-std=c++14”]} 修改为: extra_compile_args = {“cxx”: []};
![在这里插入图片描述](https://img-blog.csdnimg.cn/b45442a5d939400985bed7b01ec43e83.png#pic_center =500x)
## 2.3 安装pytorch3D
- 安装VS2019，以管理员身份打开下图所示的**x64 Native Tools Command Prompt for VS 2019**终端，然后cd到pytorch3d解压后的目录路径里，然后激活虚拟环境。最好用2019，VS2022可能会出一些问题。

![在这里插入图片描述](https://img-blog.csdnimg.cn/6075c85d7fb14ef896eb8780740de0ec.png#pic_center =400x )

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee822b269b4f449588445af487596aa2.png#pic_center =900x)
- 在窗口依次输入执行下面的命令：

```powershell
set DISTUTILS_USE_SDK=1
set PYTORCH3D_NO_NINJA=1
```
- 最后执行安装pytorch3D的代码

```powershell
python setup.py install
```
如果一切顺利，那么等待执行完成，pytorch就安装成功了。
也有可能会遇到一些错误，下面是一些常见错误的总结和解决方法。
<br><br>
# 三、安装时可能遇到的问题
## 3.1 遇到如下报错解决方法
```powershell
C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/include\cub/device/dispatch/dispatch_segmented_sort.cuh(338): error: invalid combination of type specifiers
C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/include\cub/device/dispatch/dispatch_segmented_sort.cuh(338): error: expected an identifier
C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/include\cub/device/dispatch/dispatch_segmented_sort.cuh(379): error: expected a member name

3 errors detected in the compilation of "D:/research/code/pytorch3d/pytorch3d/csrc/pulsar/cuda/renderer.backward.gpu.cu".
renderer.backward.gpu.cu
```
### 解决方法：
- **该错误原因可能是cub版本不正确，重新下载其他版本的然后配置好就行**，我遇到的就是这个问题，刚开始下载的是1.10的后来重新下载了1.71的版本把1.10的替换掉就解决了。
## 3.2遇到如下报错及解决方法
```powershell
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.7\include\thrust\system\cuda\config.h(78): fatal error C1189: 
#error: The version of CUB in your include path is not compatible with this release of Thrust. 
CUB is now included in the CUDA Toolkit, so you no longer need to use your own checkout of CUB. 
Define THRUST_IGNORE_CUB_VERSION_CHECK to ignore this. ball_query.cu
```
### 解决方法：
- 编辑位于 "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.7\include\thrust\system\cuda\config.h"的config.h文件。Ctrl+F搜索 `#ifndef THRUST_IGNORE_CUB_VERSION_CHECK` 然后在它前面加上一行 `#define THRUST_IGNORE_CUB_VERSION_CHECK` 代码.该解决方法参考了[https://github.com/facebookresearch/pytorch3d/issues/1299](https://github.com/facebookresearch/pytorch3d/issues/1299)

## 3.3遇到如下报错及解决方法
```powershell
subprocess.CalledProcessError: Command ‘[‘ninja‘, ‘-v‘]‘ returned non-zero exit status 1.
...
...
File "D:\Programs\python3.6.8\lib\site-packages\torch\utils\cpp_extension.py", line 1529, in _run_ninja_build
raise RuntimeError(message)
RuntimeError: Error compiling objects for extension
```
### 解决方法：
- **编辑torch\utils文件夹下的cpp_extension.py文件**，conda虚拟环境下torch一般位于虚拟环境\Lib\site-packages中,比如我的cpp_extension.py文件位于**D:\software\anaconda3\envs\torch-gpu\Lib\site-packages\torch\utils**。Ctrl+F搜索 'ninja' 然后将['ninja','-v']改成['ninja','--version']即可

### 到这里基本上应该能解决大部分问题，如果还有其他问题，就有可能是前面的步骤没做好，或者是我也没遇到过的问题。
### 最后，祝大家好运 (｡◕ˇ∀ˇ◕)
