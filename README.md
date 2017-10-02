# opencv_3.0.0
## 说明
cuda8.0+opencv_3.0.0
修改了部分原版的opencv3.0以便和cuda8.0兼容
- modules/cudalegacy/src/graphcuts.cpp

```c++
// 注释下面这句
#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER)
// 改为下面这句
// GraphCut has been removed in NPP 8.0
#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER) || (CUDART_VERSION >= 8000)

```
- /home/rgh/software/opencv-3.0.0/3rdparty/ippicv/downloads/linux-xxx中自带了ippicv_linux_20141027.tgz，自己下载太慢，xxx可能有变，拷贝到对应目录就好

## 编译

- 安装依赖

```bash
sudo sh dependencies.sh
```

- 编译

```bash
mkdir build
cd build
# cmake此处就会卡主下载上面说的ippicv_linux_20141027.tgz，把这个文件拷贝到你对应的opencv-3.0.0/3rdparty/ippicv/downloads/linux-xxx中
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..
make -j 16
sudo make install
sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig
```
- 查看opencv版本

```bash
pkg-config --modversion opencv
# 显示3.0.0，为OK
```
