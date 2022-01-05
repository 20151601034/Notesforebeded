# 交叉编译

## 验证过程一栏：

ssh [mm@10.8.104.64](mailto:mm@10.8.104.64) ，密码mm

安装ubuntu docker环境

 

 

## 运行当前的ubuntu的镜像环境，最新

 

`docker run -it --name=``”lidarzzy”`

 ` docker run -it --name="ridar_test" `artifactory.momenta.works/docker-msd/tian-mu-shan/orin_ubuntu20.04:v2.0.4 

 

## 拉取当前模块  

git clone https://devops.momenta.works/Momenta/maf/_git/maf_perception_lidar -b dev_orin --recursive 

git clone https://devops.momenta.works/Momenta/mf/_git/momenta_cmake

git checkout 0ba4b54be0114a4670edb8564bf3a26815b4c576 拉取对应分支名称

git pull

 遇到momenta cmake中环境的交叉编译库缺失的问题，直接使用momenta_cmake版本切换到



## 完成交叉编译

 Cmake 方式

cmake -DCMAKE_TOOLCHAIN_FILE=./orin.cmake -DCMAKE_INSTALL_PREFIX=install ..

建立orin.cmake 



tar cvfz code.tar.gz  目录路径



# mfrmster节点（通信模块）

## 拷贝代码到跳板机momenta@10.8.104.75

tar cvfz code.tar.gz  目录路径  目标名称

从编译环境的镜像中发送消息：将代码发到跳板机：

scp + momenta@10.8.104.75：目标路径 





## 从跳板机上拷贝到目标板子中

ssh mm@198.18.36.9

cd framwork_B

cd yyl

chown mm yyl 

![image-20211231141552639](C:\Users\yeyulin\AppData\Roaming\Typora\typora-user-images\image-20211231141552639.png)

# mfmaster节点信息

## 将mfmaster加到环境Path中。

~~~shell
source ../../yangg/mfr_integration/build/mfr

source ../../yangg/mfr_integration/build/mfr_setup.bash
~~~



运行端口，监听端口10022，完成在mfr master -p 10022

启动master节点： 

~~~shell
mfrmaster -p 10022
mfrmaster 
~~~

source ../../yangg/mfr_integration/build/mfr_setup.bash

## 启动某一个模块



export LD_LIBRARY_PATH=$LD_LIBRARY_PATH：/usr/local/lib

mfrlaunch file -c lidar_perception_mfr_node_debug.yaml -m mfrrpc://127.0.0.1:10022

![image-20211231131410653](C:\Users\yeyulin\AppData\Roaming\Typora\typora-user-images\image-20211231131410653.png)

PS：缺失某个文件的时候怎么找：根据相应的库函数找到 

![image-20211231142215926](C:\Users\yeyulin\AppData\Roaming\Typora\typora-user-images\image-20211231142215926.png)

![image-20211231133052106](C:\Users\yeyulin\AppData\Roaming\Typora\typora-user-images\image-20211231133052106.png)

# ROS-dev播报

## 拉取镜像

安装ROS-dev容器 安装git节点

git clone -b lk/lotus-maf2.2  https://devops.momenta.works/Momenta/msd/_git/ros-dev

git submodule update --init --recursive ：此处防止没有拉完全。







![](file:///C:/Users/yeyulin/Documents/WXWork/1688855652288921/Cache/Image/2021-12/企业微信截图_164087789128.png)![img](file:///C:/Users/yeyulin/Documents/WXWork/1688855652288921/Cache/Image/2021-12/企业微信截图_1640878123383(1).png)![img](file:///C:/Users/yeyulin/Documents/WXWork/1688855652288921/Cache/Image/2021-12/企业微信截图_16408782341015.png)1. 试水了一下这个 ros_dev 里面可以直接起ros_mfr_adaptor 节点 很方便 
\2. 学姐你可以去这个分支 里面需要的代码已经下载好了（你这边只需要maf_common 和 ros_mfr_adapter） 不需要自己再拉代码
\3. 如果要自己拉代码 需要注意 git lfs 版本 最好是2.10 不能过高 要不然maf_common拉不完整（二进制文件格式问题 有bug)

## 设置环境变量

\4. 因为你启动ros_mfr_adaptor的时候 要连到orin上去 所以在roslaunch之前 要先设置一个环境变量
 export MFR_MASTER_URI=mfrrpc://198.18.36.9:10022 , 这个环境变量会告诉你启动的ros_mfr_adaptor节点 要去注册是 mfrmaster 地址为：198.18.36.9 ，端口为 10022
这样 你在台架上面发出的消息 才能发送到orin板子上

~~~
ros@momenta-ThinkPad-T14-Gen-2i.master> export MFR_MASTER_URI=mfrrpc://198.18.36.9:10023
ros@momenta-ThinkPad-T14-Gen-2i.master> export ROS_MASTER_URT=http://localhost:11224
~~~





## 修改json文件



lidar_velodyne_adaptor.json 文件路径在 /home/ros/catkin_ws/src/ros_mfr_adaptor/resource/lidar_velodyne_adaptor.json

source 一下/home/ros/catkin_ws/src/ros_mfr_adaptor/resource。

后面就按照它的教程 改一下这个json配置文件

![img](file:///C:/Users/yeyulin/Documents/WXWork/1688855652288921/Cache/Image/2021-12/企业微信截图_1640879143174.png)

然后执行roslaunch ros_mfr_adaptor lidar_velodyne_adaptor lidar_velodyne_adaptor.launch 就可以了
这样 节点就会启动起来

[(62条消息) rosrun和roslaunch命令比较_shuijinghua的博客-CSDN博客_rosrun](https://blog.csdn.net/weixin_38145317/article/details/83862907)

## ros-dev 中cmake文件

ros-dev里面用的是 catkin_make 来编译
进入catkin_ws文件夹 ：cd /home/ros/catkin_ws
执行编译指令：在catkin_ws文件夹下执行catkin_make
就完成ros_mfr_adapter的编译了



![image-20211231151702369](C:\Users\yeyulin\AppData\Roaming\Typora\typora-user-images\image-20211231151702369.png)







----首先从source文件夹里面找到.json文件，并且source一下，然后将其

 find / -name "lidar_velodyne_adaptor.json"

 /home/ros/catkin_ws/src/ros_mfr_adaptor/launch/lidar_velodyne_adaptor.json   / 19:08:34

source /home/ros/catkin_ws/devel/setup.bash 安装setup

source devel/setup.zsh



# 播包

momenta@10.8.104.75:/mnt/usbhd1/wzq/wzq_rb_small_321_30s.bag 

