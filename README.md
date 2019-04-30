# compile-settings

# GROMACS
## 2016.1
### Dependencies
```console
~/src/gromacs/gromacs-2016.1/build$ gcc --version
gcc (Ubuntu 5.5.0-12ubuntu1) 5.5.0 20171010
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

~/src/gromacs/gromacs-2016.1/build$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Tue_Jan_10_13:22:03_CST_2017
Cuda compilation tools, release 8.0, V8.0.61
```

### Process
```console
cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/packages/gromacs/2016.1 -DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/packages/cuda/8.0

make -j 6

make check

sudo checkinstall
```