# compile-settings
A log of the installation dependencies and processes of annoying packages, as well as said packages.


## Contents
- [Useful notes](#notes)
- [GROMACS](#gmx)
- [AMBER](#amber)

<a name="notes"/>

## Useful notes
### checkinstall
[checkinstall](https://wiki.debian.org/CheckInstall) is a useful tool that replaces `make install`. It builds a .deb package that is easily removable with:

```console
lily@gavle:$ dpkg -r yourpackagename
```

**Warning**

If you name your package the same as any package in the repositories accessible by `apt`, it is prone to getting removed and 'updated' the next time Cron or you run an apt upgrade! 


<a name="gmx"/>

## GROMACS
### 2016.1
#### Dependencies

gcc=5.5

cuda=8.0

```console
lily@gavle:$ gcc --version
gcc (Ubuntu 5.5.0-12ubuntu1) 5.5.0 20171010
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

lily@gavle:$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Tue_Jan_10_13:22:03_CST_2017
Cuda compilation tools, release 8.0, V8.0.61
```

#### Process
```console
lily@gavle:$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/packages/gromacs/2016.1 -DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/packages/cuda/8.0

lily@gavle:$ make -j 6

lily@gavle:$ make check

lily@gavle:$ sudo checkinstall
```

### 2018.1
#### Dependencies

gcc=6.5

cuda=10.1

```console
lily@gavle:$ gcc --version
gcc (Ubuntu 6.5.0-2ubuntu1) 6.5.0 20181026
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

lily@gavle:$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Fri_Feb__8_19:08:17_PST_2019
Cuda compilation tools, release 10.1, V10.1.105

```

#### Process
```console
lily@gavle:$ cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON -DGMX_GPU=on -DCMAKE_INSTALL_PREFIX=/packages/gromacs/2016.1 -DCMAKE_CXX_FLAGS=-fPIC -DCUDA_TOOLKIT_ROOT_DIR=/packages/cuda/8.0

lily@gavle:$ make -j 6

lily@gavle:$ make check

lily@gavle:$ sudo checkinstall
```

<a name="amber"/>

## AMBER
### 18
#### Dependencies

gcc=8.3.0

cuda=10.1

```console
lily@gavle:$ gcc --version
gcc (Ubuntu 8.3.0-6ubuntu1) 8.3.0
Copyright (C) 2018 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

lily@gavle:$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Fri_Feb__8_19:08:17_PST_2019
Cuda compilation tools, release 10.1, V10.1.105
```

#### Process
```console
lily@gavle:$ cd 

lily@gavle:$ ./configure -cuda gnu

lily@gavle:$ sudo checkinstall
```

**Note:** I ran into trouble with AMBER not finding $CUDA_HOME. After you've made sure it is set with echo $CUDA_HOME, one solution is:

```console
lily@gavle:$ echo $CUDA_HOME && checkinstall
```

However, if you are installing at a system level and require root permissions, `echo $CUDA_HOME && sudo checkinstall` *did not* work for me. The only way to get around it was to start a root session.

```console
lily@gavle:$ sudo su

root@gavle:$ export CUDA_HOME=/packages/cuda/10.1

root@gavle:$ echo $CUDA_HOME && checkinstall
```

I also symlinked `/bin/cpptraj.cuda` to `/bin/cpptraj` .