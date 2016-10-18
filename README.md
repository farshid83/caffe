# How to install Caffe on a Redhat Linux Server (lionxv.rcc.psu.edu) LOCALLY without SUDO/ROOT access



# ~/.bashrc and ~/.bash_profile look like as:

<p># .bashrc
<p>if [ -f /etc/bashrc ]; then </p>
<p>       . /etc/bashrc </p>
<p>fi</p>
<p># User specific aliases and functions
<p>HOME=/gpfs/home/fuf111
<p>export PATH=$HOME/usr/bin/:$HOME/usr/include/:$HOME/usr/lib/:$PATH
<p>export PATH=$HOME/usr/python/Python-2.7.12/:$PATH
<p>export LD_LIBRARY_PATH=$HOME/usr/lib/:$HOME/usr/protobuf/src/.libs:$LD_LIBRARY_PATH
<p>export PKG_CONFIG_PATH=$HOME/usr/lib/pkgconfig:$PKG_CONFIG_PATH


<p># .bash_profile
<p># Get the aliases and functions
<p>if [ -f ~/.bashrc ]; then
<p>        . ~/.bashrc
<p>fi



# Load necessary modules as much as possible

module load cmake

module load python/2.7.8

module load matlab/R2015a

module load opencv

module load mkl/11.2.0.90

module load gcc/4.7.1

#module load blas/3.5.0

module load hdf5/1.8.11

module load gurobi/6.5.1



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




