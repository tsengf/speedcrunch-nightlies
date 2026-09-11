# speedcrunch-nightlies
Repository to build MacOS binaries from the official [SpeedCrunch repository](https://bitbucket.org/heldercorreia/speedcrunch).

Please check the [Releases](https://github.com/tsengf/speedcrunch-nightlies/releases) to download the latest builds.

# Building

## Qt
SpeedCrunch requires Qt. I am using version 6.11.2. For SpeedCrunch 0.12, I was able to get only version 6.7.3 to successfully build on the latest MacOS.

        wget https://download.qt.io/official_releases/qt/6.11/6.11.2/single/qt-everywhere-src-6.11.2.tar.xz
        tar xf qt-everywhere-src-6.11.2.tar.xz
        cd qt-everywhere-6.7.3

Run configure with the following options.
* Specify the installation path 'speedcrunch-nightlies/qt-static'
* Build static binaries to avoid runtime dependencies to Qt
* Build only the submodules required by SpeedCrunch

        ./configure -prefix ../qt-static -static -submodules qtbase,qttools,qtdeclarative

Qt is now configured for building. Build it.

        cmake --build .

Install Qt to the installation path.

        cmake --install .

Qt will be installed into 'speedcrunch-nightlies/qt-static'

## SpeedCrunch

Clone the SpeedCrunch source.

        cd ..
        git clone git@bitbucket.org:heldercorreia/speedcrunch.git

Build SpeedCrunch.
* Point Cmake to Qt

        mkdir build
        cd build
        cmake -DCMAKE_PREFIX_PATH=`realpath ../qt-static/lib/cmake` ../speedcrunch/src
        make

Generate a SpeedCrunch package.

        make package

Your package will be created as 'build/SpeedCrunch.dmg'.

# Installation

Install 'SpeedCrunch.dmg'.

In a terminal, type

        attr -c /Applications/SpeedCrunch.app
