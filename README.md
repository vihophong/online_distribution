# Installation instruction (example)


## Install newbasic first

cd build/newbasic

/home/phong/projects/rcdaq/online_distribution/newbasic/autogen.sh --prefix=/home/phong/projects/rcdaq/install

make install

## Install pmonitor

cd build/pmonitor

cmake3 -DCMAKE_INSTALL_PREFIX=/home/phong/projects/rcdaq/install /home/phong/projects/rcdaq/online_distribution/pmonitorcmake

make install


## Install example of user analysis code

cd build/useranalysiscmake

cmake3 /home/phong/projects/rcdaq/online_distribution/useranalysiscmake

make




