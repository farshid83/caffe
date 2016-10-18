# How to install Caffe on a Redhat Linux Server (lionxv.rcc.psu.edu) LOCALLY without SUDO/ROOT access




# ~/.bashrc and ~/.bash_profile look like as:

## .bashrc
## Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
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



# Makefile.config changes are:
CPU_ONLY := 1
BLAS := mkl
MATLAB_DIR := /usr/global/matlab/R2015a/
PYTHON_INCLUDE := $(HOME)/usr/include/python2.7
PYTHON_LIB := $(HOME)/usr/lib/python2.7
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include $(HOME)/usr/include /usr/global/opencv/2.4.2/include /usr/global/hdf5/1.8.11/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/lib /usr/local/lib $(HOME)/usr/lib /usr/global/opencv/2.4.2/lib /usr/global/hdf5/1.8.11/lib /usr/global/intel/composer_xe/2015.0.090/mkl/lib/intel64



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

leveldb-devel 
snappy-devel 
boost-devel 
gflags-devel 
glog-devel 
lmdb-devel

# glog
wget https://google-glog.googlecode.com/files/glog-0.3.3.tar.gz
tar zxvf glog-0.3.3.tar.gz
cd glog-0.3.3
./configure
make && make install
# gflags
wget https://github.com/schuhschuh/gflags/archive/master.zip
unzip master.zip
cd gflags-master
mkdir build && cd build
export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1
make && make install
# lmdb
git clone https://github.com/LMDB/lmdb
cd lmdb/libraries/liblmdb
make && make install





