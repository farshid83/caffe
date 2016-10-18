# How to install Caffe on a Redhat Linux Server (lionxv.rcc.psu.edu) LOCALLY without SUDO/ROOT access
# The instructions are inspired from http://autchen.github.io/guides/2015/04/03/caffe-install.html



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



# Your ~/.bashrc will look like (~/.bash_profile should just exe ~/.bashrc):

# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
# User specific aliases and functions
HOME=/gpfs/home/fuf111
export PATH=$HOME/usr/bin/:$HOME/usr/include/:$HOME/usr/lib/:$PATH
export PATH=$HOME/usr/python/Python-2.7.12/:$PATH
export LD_LIBRARY_PATH=$HOME/usr/lib/:$HOME/usr/protobuf/src/.libs:$LD_LIBRARY_PATH
export PKG_CONFIG_PATH=$HOME/usr/lib/pkgconfig:$PKG_CONFIG_PATH

# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi






