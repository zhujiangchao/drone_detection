## 12.13

paper note: 
add pub key to github.com

## 12.14
install anaconda3 and cuda-11.1(not finished)
read the paper(Real-Time Seamless Single Shot 6D Object Pose Prediction)

## 12.15
system crashed and reinstalled it
use conda create a virtual env and install some packages

## 12.16
finish the configuration of the virtual environment
download the data needed for  test
test the single object recognition(the result in test_rest.dot)
valid_multi.py has some problem with loading weigts file etc
next few days I will go back to school for classes
when I go back to the lab, I will try to solve the loading problem.

![image-20201220160932595](picts/image-20201220160932595.png)

## 12.21

完成了bounding box的绘制

![image-20201221230653119](picts/image-20201221230653119.png)

## 12.22

本文数据集的标注

1. 获得3D bouding box  本文使用LINEMOD数据集提供的3D object model得到的
2. 定义3D bounding box的8个角和形心，8个corner的坐标为[[min_x, min_y, min_z], [min_x, min_y, max_z], [min_x, max_y, min_z], [min_x, max_y, max_z], [max_x, min_y, min_z], [max_x, min_y, max_z], [max_x, max_y, min_z], [max_x, max_y, max_z]]，形心坐标为[0,0,0]
3. 将3D关键点投影到2D，需要用到相机的内在校准矩阵，以及真实的旋转和平移。旋转和平移需要自己设法测量，文章作者给的提示参见这篇 [paper](http://cmp.felk.cvut.cz/~hodanto2/data/hodan2017tless.pdf)的3.1节，但感觉对无人机不适用
4. 计算围绕物体的2D rectangle box，本文通过拟合一个紧紧包含8个corner投影点的rectangle来获得2D box
5. 制作label。本文label长度为21，第一个是class label，后面9对是形心和8个corner的坐标，最后两个是x和y的变化范围（即坐标中最大值与最小值差）

### 12.30

思考的一些问题：

​       论文没有明确给出pnp算法所用到的坐标系，由于pnp算法需要用到三维空间中的真实坐标来估计相机的位姿，如果以大地（外界环境）作为世界系坐标，很难获得每张图片的关键点对应的3维坐标。如果以物体建立世界坐标系，那么就可以获得3维坐标了，这个可能是大家都默认的吧，很多博客上都没有指出世界坐标系是哪个，不过有几篇博客明确指出了世界坐标系是以物体建立的，这篇博客https://www.aiuai.cn/aifarm909.html是关于头部位置预测的，明确指出了世界坐标系的原点是鼻子。

另外，熟悉了下python对视频的处理，尝试从视频中逐帧识别，无法找到与论文训练集相关的视频，故只能用其数据集先进行视频的合成。

