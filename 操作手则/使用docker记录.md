# 完整的交叉编译环境名称

## 完整ubuntu镜像

root@afec67894f92 名称： ridarzzy (afec67894f92)

#### Image

artifactory.momenta.works/docker-msd/tian-mu-shan/orin_ubuntu20.04:v2.0.5 (92849982f7af)



编译的内容在home/ros/mfr_integration



## 交叉编译工具链

安装交叉编译工具链。

set(CMAKE_SYSTEM_NAME Linux)

set(CMAKE_SYSTEM_PROCESSOR aarch64)



set(ORIN_SDK /root/.iso_compiler/v2/gcc/aarch64-9.3)

set(CMAKE_C_COMPILER ${ORIN_SDK}/bin/aarch64-buildroot-linux-gnu-gcc)

set(CMAKE_CXX_COMPILER ${ORIN_SDK}/bin/aarch64-buildroot-linux-gnu-g++)



SET(CMAKE_FIND_ROOT_PATH ${ORIN_SDK}/aarch64-buildroot-linux-gnu/sysroot)

 

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)



set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)

set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

set(BOOST_ROOT ${ORIN_SDK}/thirdparty)

​    cmake -DCMAKE_TOOLCHAIN_FILE=./orin.cmake -DCMAKE_INSTALL_PREFIX=install ..
