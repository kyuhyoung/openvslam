<p align="center">
<img src="https://raw.githubusercontent.com/xdspacelab/openvslam/master/docs/img/logo.png" width="435px">
</p>

# OpenVSLAM: A Versatile Visual SLAM Framework
[![Wercker Status](https://app.wercker.com/status/8b02a43f48216385658bb3857aae5fd8/s/)](https://app.wercker.com/project/byKey/8b02a43f48216385658bb3857aae5fd8)
[![Travis CI Status](https://travis-ci.org/xdspacelab/openvslam.svg)](https://travis-ci.org/xdspacelab/openvslam)
[![Documentation Status](https://readthedocs.org/projects/openvslam/badge/?version=master)](https://openvslam.readthedocs.io/en/master/?badge=master)
[![Docker Build Status](https://img.shields.io/docker/cloud/build/shinsumicco/openvslam.svg)](https://hub.docker.com/r/shinsumicco/openvslam)

## Overview

[<img src="https://raw.githubusercontent.com/xdspacelab/openvslam/master/docs/img/teaser.png" width="640px">](https://arxiv.org/abs/1910.01122)

[<img src="https://raw.githubusercontent.com/wiki/xdspacelab/openvslam/media/tracking.gif" width="640px">](https://www.youtube.com/watch?v=Ro_s3Lbx5ms)

[**[PrePrint]**](https://arxiv.org/abs/1910.01122) [**[YouTube]**](https://www.youtube.com/watch?v=Ro_s3Lbx5ms)

OpenVSLAM is a monocular, stereo, and RGBD visual SLAM system.
The notable features are:

- It is compatible with **various type of camera models** and can be easily customized for other camera models.
- Created maps can be **stored and loaded**, then OpenVSLAM can **localize new images** based on the prebuilt maps.
- The system is fully modular. It is designed by encapsulating several functions in separated components with easy-to-understand APIs.
- We provided **some code snippets** to understand the core functionalities of this system.

OpenVSLAM is based on an indirect SLAM algorithm with sparse features, such as ORB-SLAM, ProSLAM, and UcoSLAM.
One of the noteworthy features of OpenVSLAM is that the system can deal with various type of camera models, such as perspective, fisheye, and equirectangular.
If needed, users can implement extra camera models (e.g. dual fisheye, catadioptric) with ease.
For example, visual SLAM algorithm using **equirectangular camera models** (e.g. RICOH THETA series, insta360 series, etc) is shown above.

Some code snippets to understand the core functionalities of the system are provided.
You can employ these snippets for in your own programs.
Please see the `*.cc` files in `./example` directory or check [Simple Tutorial](https://openvslam.readthedocs.io/en/master/simple_tutorial.html) and [Example](https://openvslam.readthedocs.io/en/master/example.html).

We provided [documentation](https://openvslam.readthedocs.io/) for installation and tutorial.
Please contact us via [GitHub issues](https://github.com/xdspacelab/openvslam/issues) if you have any questions or notice any bugs about the software.

## Motivation

Visual SLAM is regarded as a next-generation technology for supporting industries such as automotives, robotics, and xR. 
We released OpenVSLAM as an opensource project with the aim of collaborating with people around the world to accelerate the development of this field.
In return, we hope this project will bring safe and reliable technologies for a better society.

## Installation

### For Ubuntu 16.04.

```
apt update -y
apt upgrade -y --no-install-recommends
# basic dependencies
apt install -y build-essential pkg-config cmake git wget curl unzip
apt install -y libyaml-cpp-dev libgoogle-glog-dev libgflags-dev
```

#### Skip this if Pangolin is installed already.
```
apt install -y libglew-dev
cd /path/to/working/dir
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build && cd build
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    ..
make -j4
make install
```

#### Skip this if Eigen is installed already.
```
cd /path/to/working/dir
wget -q http://bitbucket.org/eigen/eigen/get/3.3.4.tar.bz2
tar xf 3.3.4.tar.bz2
rm -rf 3.3.4.tar.bz2
cd eigen-eigen-5a0156e40feb
mkdir -p build && cd build
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    ..
make -j4
make install
```

#### Skip this if OpenCV is installed already.
```
apt install -y libgtk-3-dev
apt install -y ffmpeg
apt install -y libavcodec-dev libavformat-dev libavutil-dev libswscale-dev libavresample-dev

cd /path/to/working/dir
wget -q https://github.com/opencv/opencv/archive/3.4.0.zip
unzip -q 3.4.0.zip
rm -rf 3.4.0.zip
cd opencv-3.4.0
mkdir -p build && cd build
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DENABLE_CXX11=ON \
    -DBUILD_DOCS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_JASPER=OFF \
    -DBUILD_OPENEXR=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_TESTS=OFF \
    -DWITH_EIGEN=ON \
    -DWITH_FFMPEG=ON \
    -DWITH_OPENMP=ON \
    ..
make -j4
make install
```

#### latest g2o install.
```
# g2o dependencies
apt install -y libatlas-base-dev libsuitesparse-dev
cd /path/to/working/dir
git clone https://github.com/RainerKuemmerle/g2o.git
cd g2o
mkdir build && cd build
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DCMAKE_CXX_FLAGS=-std=c++11 \
    -DBUILD_SHARED_LIBS=ON \
    -DBUILD_UNITTESTS=OFF \
    -DBUILD_WITH_MARCH_NATIVE=OFF \
    -DG2O_USE_CHOLMOD=ON \
    -DG2O_USE_CSPARSE=ON \
    -DG2O_USE_OPENGL=OFF \
    -DG2O_USE_OPENMP=ON \
    ..
make -j4
make install
```

#### custom DBoW2 install.
```
cd /path/to/working/dir
git clone https://github.com/shinsumicco/DBoW2.git
cd DBoW2
mkdir build && cd build
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    ..
make -j4
make install
```

#### finally openvslam install.
```
cd /path/to/openvslam
mkdir build && cd build
cmake \
    -DBUILD_WITH_MARCH_NATIVE=OFF \
    -DUSE_PANGOLIN_VIEWER=ON \
    -DUSE_SOCKET_PUBLISHER=OFF \
    -DUSE_STACK_TRACE_LOGGER=ON \
    -DBOW_FRAMEWORK=DBoW2 \
    -DBUILD_TESTS=ON \
    ..
make -j4
```
 * If error message with "lib_opencv3 ~~~ " occurs, try this 
 ```
 sudo apt intall --reinstall ros-kinetic-opencv3
 ```
 * If error message with "libyaml-cpp ~~~ " occurs, try this 
 ```
 cd /path/to/working/dir
 git clone https://github.com/jbeder/yaml-cpp.git
 cd yaml-cpp
 mkdir build
 cd build
 cmake -DYAML_BUILD_SHARED_LIBS=ON -DBUILD_SHARED_LIBS=ON ..
 make
 make install
 ```
 * If error message saying "some shared library with extension so files are missing", copy the so files into the apporopriate directory if you can find them using "find / -name ~~~ ".  Otherwise, try to install using apt install. 
 

Please see [**Installation**](https://openvslam.readthedocs.io/en/master/installation.html) chapter in the [documentation](https://openvslam.readthedocs.io/).

[**The instructions for Docker users**](https://openvslam.readthedocs.io/en/master/docker.html) are also provided.

## Tutorial

Please see [**Simple Tutorial**](https://openvslam.readthedocs.io/en/master/simple_tutorial.html) chapter in the [documentation](https://openvslam.readthedocs.io/).

A sample ORB vocabulary file can be downloaded from [here](https://drive.google.com/open?id=1wUPb328th8bUqhOk-i8xllt5mgRW4n84).
Sample datasets are also provided at [here](https://drive.google.com/open?id=1A_gq8LYuENePhNHsuscLZQPhbJJwzAq4).

If you would like to run visual SLAM with standard benchmarking datasets (e.g. KITTI Odometry dataset), please see [**SLAM with standard datasets**](https://openvslam.readthedocs.io/en/master/example.html#slam-with-standard-datasets) section in the [documentation](https://openvslam.readthedocs.io/).

## Community

If you want to join our Slack community, please fill out the application form from the following site(s):

- [http://bit.ly/openvslam](http://bit.ly/openvslam)
- [http://bit.ly/openvslam-jp](http://bit.ly/openvslam-jp) (日本語: in Japanese)

## Currently working on

- IMU integration
- Python bindings
- Implementation of extra camera models
- Refactoring

Feedbacks, feature requests, and contribution are welcome!

## License

**2-clause BSD license** (see [LICENSE](./LICENSE))

The following files are derived from third-party libraries.

- `./3rd/json` : [nlohmann/json \[v3.6.1\]](https://github.com/nlohmann/json) (MIT license)
- `./3rd/popl` : [badaix/popl \[v1.2.0\]](https://github.com/badaix/popl) (MIT license)
- `./3rd/spdlog` : [gabime/spdlog \[v1.3.1\]](https://github.com/gabime/spdlog) (MIT license)
- `./src/openvslam/solver/pnp_solver.cc` : part of [laurentkneip/opengv](https://github.com/laurentkneip/opengv) (3-clause BSD license)
- `./src/openvslam/feature/orb_extractor.cc` : part of [opencv/opencv](https://github.com/opencv/opencv) (3-clause BSD License)
- `./src/openvslam/feature/orb_point_pairs.h` : part of [opencv/opencv](https://github.com/opencv/opencv) (3-clause BSD License)
- `./viewer/public/js/lib/dat.gui.min.js` : [dataarts/dat.gui](https://github.com/dataarts/dat.gui) (Apache License 2.0)
- `./viewer/public/js/lib/protobuf.min.js` : [protobufjs/protobuf.js](https://github.com/protobufjs/protobuf.js) (3-clause BSD License)
- `./viewer/public/js/lib/stats.min.js` : [mrdoob/stats.js](https://github.com/mrdoob/stats.js) (MIT license)
- `./viewer/public/js/lib/three.min.js` : [mrdoob/three.js](https://github.com/mrdoob/three.js) (MIT license)

Please use `g2o` as the dynamic link library because `csparse_extension` module of `g2o` is LGPLv3+.

## Contributors

- Shinya Sumikura ([@shinsumicco](https://github.com/shinsumicco))
- Mikiya Shibuya ([@MikiyaShibuya](https://github.com/MikiyaShibuya))
- Ken Sakurada ([@kensakurada](https://github.com/kensakurada))

## Citation

If OpenVSLAM helps your research, please cite the paper for OpenVSLAM. Here is a BibTeX entry:

```
@inproceedings{openvslam2019,
  author = {Sumikura, Shinya and Shibuya, Mikiya and Sakurada, Ken},
  title = {{OpenVSLAM: A Versatile Visual SLAM Framework}},
  booktitle = {Proceedings of the 27th ACM International Conference on Multimedia},
  series = {MM '19},
  year = {2019},
  isbn = {978-1-4503-6889-6/19/10},
  location = {Nice, France},
  numpages = {4},
  url = {https://doi.org/10.1145/3343031.3350539},
  doi = {10.1145/3343031.3350539},
  publisher = {ACM},
  address = {New York, NY, USA}
}
```

The preprint can be found [here](https://arxiv.org/abs/1910.01122).

## Reference

- Raúl Mur-Artal, J. M. M. Montiel, and Juan D. Tardós. 2015. ORB-SLAM: a Versatile and Accurate Monocular SLAM System. IEEE Transactions on Robotics 31, 5 (2015), 1147–1163.
- Raúl Mur-Artal and Juan D. Tardós. 2017. ORB-SLAM2: an Open-Source SLAM System for Monocular, Stereo and RGB-D Cameras. IEEE Transactions on Robotics 33, 5 (2017), 1255–1262.
- Dominik Schlegel, Mirco Colosi, and Giorgio Grisetti. 2018. ProSLAM: Graph SLAM from a Programmer’s Perspective. In Proceedings of IEEE International Conference on Robotics and Automation (ICRA). 1–9.
- Rafael Muñoz-Salinas and Rafael Medina Carnicer. 2019. UcoSLAM: Simultaneous Localization and Mapping by Fusion of KeyPoints and Squared Planar Markers. arXiv:1902.03729.
- Mapillary AB. 2019. OpenSfM. https://github.com/mapillary/OpenSfM.
- Giorgio Grisetti, Rainer Kümmerle, Cyrill Stachniss, and Wolfram Burgard. 2010. A Tutorial on Graph-Based SLAM. IEEE Transactions on Intelligent Transportation SystemsMagazine 2, 4 (2010), 31–43.
- Rainer Kümmerle, Giorgio Grisetti, Hauke Strasdat, Kurt Konolige, and Wolfram Burgard. 2011. g2o: A general framework for graph optimization. In Proceedings of IEEE International Conference on Robotics and Automation (ICRA). 3607–3613.
