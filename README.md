# How to install Caffe on a Redhat Linux Server (lionxv.rcc.psu.edu) LOCALLY without SUDO/ROOT access



# ~/.bashrc and ~/.bash_profile look like as:

## .bashrc
## Source global definitions
<p>if [ -f /etc/bashrc ]; then </p>
<p>       . /etc/bashrc </p>
<p>fi</p>
## User specific aliases and functions
HOME=/gpfs/home/fuf111
export PATH=$HOME/usr/bin/:$HOME/usr/include/:$HOME/usr/lib/:$PATH
export PATH=$HOME/usr/python/Python-2.7.12/:$PATH
export LD_LIBRARY_PATH=$HOME/usr/lib/:$HOME/usr/protobuf/src/.libs:$LD_LIBRARY_PATH
export PKG_CONFIG_PATH=$HOME/usr/lib/pkgconfig:$PKG_CONFIG_PATH

## .bash_profile
## Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi



# Load necessary modules as much as possible

echo "Modules loading for Caffe ..."

module load cmake
echo "cmake loaded."

module load python/2.7.8
echo "python 2.7.8 loaded."

module load matlab/R2015a
echo "matlab R2015a loaded."

module load opencv
echo "opencv loaded."

module load mkl/11.2.0.90
echo "mkl/11.2.0.90 loaded."

module load gcc/4.7.1
echo "gcc/4.7.1 loaded."

#module load blas/3.5.0
#echo "blas/3.5.0 loaded."

module load hdf5/1.8.11
echo "hdf5/1.8.11 loaded."

module load gurobi/6.5.1
echo "gurobi/6.5.1 loaded."



# Always install dependencies in order:
## protobuf-devel from https://github.com/google/protobuf
git clone https://github.com/google/protobuf.git
cd protobuf
./autogen.sh
./configure --prefix=$(HOME)/usr
make
make install

## leveldb-devel from https://github.com/google/leveldb
git clone https://github.com/google/leveldb.git  --recursive
cd leveldb
make
cp --preserve=links libleveldb.* $(HOME)/usr/lib
cp -r include/leveldb $(HOME)/usr/include/  

## snappy-devel from https://github.com/google/snappy
git clone https://github.com/google/snappy.git  --recursive
cd snappy
./autogen.sh
./configure --prefix=$(HOME)/usr                      
make
make install

## boost-devel from https://sourceforge.net/projects/boost/files/boost/
curl -L -O http://sourceforge.net/projects/boost/files/boost/x.xx.x/boost_x_xx_x.tar.gz
tar zxvf boost_x_xx_x.tar.gz
cd boost_x_xx_x
./bootstrap.sh --libdir=$(HOME)/usr/lib --includedir=$(HOME)/usr/include                                                       
vi project-config.jam # edit the python dir if locally compiled
./b2
./b2 install

## gflags-devel from https://gflags.github.io/gflags/
git clone https://github.com/gflags/gflags.git --recursive
cd gflags
mkdir build
cd build
ccmake .. # change prefix field to $(HOME)/usr and then "configure"
vi CMakeCache.txt # change CMAKE_CXX_FLAGS:STRING+=-fPIC
make
make install

## glog-devel from https://github.com/google/glog
git clone https://github.com/google/glog.git --recursive
cd glog
mkdir build
cd build
ccmake .. # change prefix field to $(HOME)/usr and then "configure"
vi CMakeCache.txt # change CMAKE_CXX_FLAGS:STRING+=-fPIC
make
make install

OR

cd glog
./configure --prefix=$(HOME)/usr
make
make install

## lmdb-devel
git clone https://github.com/LMDB/lmdb  --recursive
cd lmdb/libraries/liblmdb
make
make install



# Makefile.config changes are:
CPU_ONLY := 1
BLAS := mkl
MATLAB_DIR := /usr/global/matlab/R2015a/
PYTHON_INCLUDE := $(HOME)/usr/include/python2.7
PYTHON_LIB := $(HOME)/usr/lib/python2.7
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include $(HOME)/usr/include /usr/global/opencv/2.4.2/include /usr/global/hdf5/1.8.11/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/lib /usr/local/lib $(HOME)/usr/lib /usr/global/opencv/2.4.2/lib /usr/global/hdf5/1.8.11/lib /usr/global/intel/composer_xe/2015.0.090/mkl/lib/intel64



# Caffe installation
make all
make test
make runtest




# Caffe

[![Build Status](https://travis-ci.org/BVLC/caffe.svg?branch=master)](https://travis-ci.org/BVLC/caffe)
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)

Caffe is a deep learning framework made with expression, speed, and modularity in mind.
It is developed by the Berkeley Vision and Learning Center ([BVLC](http://bvlc.eecs.berkeley.edu)) and community contributors.

Check out the [project site](http://caffe.berkeleyvision.org) for all the details like

- [DIY Deep Learning for Vision with Caffe](https://docs.google.com/presentation/d/1UeKXVgRvvxg9OUdh_UiC5G71UMscNPlvArsWER41PsU/edit#slide=id.p)
- [Tutorial Documentation](http://caffe.berkeleyvision.org/tutorial/)
- [BVLC reference models](http://caffe.berkeleyvision.org/model_zoo.html) and the [community model zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo)
- [Installation instructions](http://caffe.berkeleyvision.org/installation.html)

and step-by-step examples.

[![Join the chat at https://gitter.im/BVLC/caffe](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/BVLC/caffe?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Please join the [caffe-users group](https://groups.google.com/forum/#!forum/caffe-users) or [gitter chat](https://gitter.im/BVLC/caffe) to ask questions and talk about methods and models.
Framework development discussions and thorough bug reports are collected on [Issues](https://github.com/BVLC/caffe/issues).

Happy brewing!

## License and Citation

Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).
The BVLC reference models are released for unrestricted use.

Please cite Caffe in your publications if it helps your research:

    @article{jia2014caffe,
      Author = {Jia, Yangqing and Shelhamer, Evan and Donahue, Jeff and Karayev, Sergey and Long, Jonathan and Girshick, Ross and Guadarrama, Sergio and Darrell, Trevor},
      Journal = {arXiv preprint arXiv:1408.5093},
      Title = {Caffe: Convolutional Architecture for Fast Feature Embedding},
      Year = {2014}
    }
