# 工作记录

## 12.20 

阅读论文

https://openaccess.thecvf.com/content_CVPR_2020/papers/He_PVN3D_A_Deep_Point-Wise_3D_Keypoints_Voting_Network_for_6DoF_CVPR_2020_paper.pdf

### 估算步骤

1. 输入RGBD图像(a)

2. 使用deep Hough voting network 来预测所选3D关键点的平移量(b)

3. 同一物体上的每一点对所选关键点进行投票并且聚类的中心被选为predicted keypoint(c)

4. 用最小二乘拟合方法估计6D位姿参数(d)-(e)

   

<img src="picts/image-20201220162439025.png" alt="image-20201220162439025" style="zoom:67%;" />



### 成果

在YCB （包含21个YCB对象）和LineMOD 数据集上表现卓越

建立了基于3D关键点的Hough voting network



### 实现细节

3D关键点提取

<img src="picts/image-20201220163733320.png" alt="image-20201220163733320" style="zoom:67%;" />





<img src="picts/image-20201220163901648.png" alt="image-20201220163901648" style="zoom:67%;" />





<img src="picts/image-20201220164127351.png" alt="image-20201220164127351" style="zoom:67%;" />



### 概念区分

instance semantic segmentation:

<img src="picts/image-20201220182742479.png" alt="image-20201220182742479" style="zoom:67%;" />

semantic segmentation

<img src="picts/image-20201220182807510.png" alt="image-20201220182807510" style="zoom:67%;" />

object localization

<img src="picts/image-20201220182839279.png" alt="image-20201220182839279" style="zoom:67%;" />

image classification
 <img src="picts/image-20201220182902148.png" alt="image-20201220182902148" style="zoom:67%;" />





## 12.23

### 配置环境  install pytorch1.0.1 with python3.7,cuda9.0

昨天直接扒pytorch官网提供的命令

```
# CUDA 9.0
conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=9.0 -c pytorch
```

结果晚上9点半走的时候pytorch还没下载完，今天一看果然失败了。不知道为啥科学上网了还是慢得吓人

于是今天重新配环境

#### 首先添加镜像源

```
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

#### 安装pytorch

后面不要添加-c pytorch，否则会从默认的地址下载pytorch，速度会很慢。

```
conda install pytorch==1.0.1 torchvision==0.2.2 cudatoolkit=9.0
```

运行下面的代码，没有报错就是安装成功了。

```
import torch
import torchvision
from torchvision import transforms as trans
```

又要去排练了，希望今天能好/(ㄒoㄒ)/~~



## 12.24

O(∩_∩)O~~今天成了！

<img src="readme.assets/image-20201224130219646.png" alt="image-20201224130219646"  />

 

### Install CUDA9.0

**报错1**

![image-20201224134559150](readme.assets/image-20201224134559150.png)

解决办法

```
unset all_proxy    
unset ALL_PROXY
pip install pysocks
source ~/.bashrc
```



**报错2**

![image-20201224134708959](readme.assets/image-20201224134708959.png)

解决办法

```
pip install pyyaml
```



**报错3**

![image-20201224135644664](readme.assets/image-20201224135644664.png)

解决办法

```
pip install 包名 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

#opencv_contrib_python==3.4.2.16

**失败！！！**



### pytorch1.5+cuda10.2

看到PVN3D github里提供了pytorch1.5的方案，遂弃暗投明（希望是明/(ㄒoㄒ)/~~）



通常，Ubuntu 18.04系统上安装的GCC和G++ 版本比较高，用下面的命令来查看电脑上的GCC和G++版本：

```
gcc --version
g++ --version
```

我的电脑上安装的版本是7.5，因此需手动降级

```
sudo apt-get install gcc-4.8
sudo apt-get install g++-4.8
cd /usr/bin
ls -l gcc*
```

对gcc和g++进行备份，并重新链接：

```
sudo mv gcc gcc.bak
sudo ln -s gcc-4.8 gcc
sudo mv g++ g++.bak
sudo ln -s g++-4.8 g++
```



=.= 今天废了 以上



## 12.25

重启电脑后死在密码输入页面不能动，求助学长好papapa一顿操作终于能进入图像界面了，现在电脑是没有驱动和cuda的状态。

pytorch1.0.1装上了，配好了requirements.txt的依赖包

接下来要 install python-pcl和PointNet++



## 12.30

在学长的帮助下配好了pytorch1.7+cuda10.2的环境

下载好了Linemod数据集，有近9G，解压的时候要使用（大于2G时）

```
sudo apt-get install p7zip-full
 7z x file.zip -r -o /home/xx 
 #x表示压缩文件，并且按原始目录
 #-r表示递归所有子文件夹
 #-o表示解压到指定的目录，-o后没有空格
```

若要压缩文件

```
7z a -t7z -r file.zip /home/manager/*
#a代表添加文件/文件夹到压缩包
#-t指定压缩类型，一般为7z
#/home/manager/* 是要压缩的目录，＊是表示该目录下所有的文件
```



运行到这步时报错

```
python3 -m train.train_linemod_pvn3d --cls ape
```

![image-20201230193509396](readme.assets/image-20201230193509396.png)

可是在这个环境下

![image-20201230193540262](readme.assets/image-20201230193540262.png)

是正常的



a啊呜 原来python和sudo python下版本不一样啊



然后还有一个问题是链接的时候 ln -s 接文件的地址包括文件名 后面是链接的包



最后也终于完成ape和benchvise的demo，接下来的任务是把图片做成视频流然后看检测的帧率！

