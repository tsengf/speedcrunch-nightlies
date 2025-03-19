# speedcrunch-nightlies
Repository to build MacOS binaries from the official SpeedCrunch repository.

Instructions taken from
https://bitbucket.org/heldercorreia/speedcrunch/wiki/BuildingOSXPackage

Please check the [Releases](https://github.com/tsengf/speedcrunch-nightlies/releases) to download the latest builds.

# Setup

## Qt
SpeedCrunch depends on Qt. I was able to get only version 6.7.3 to successfully build on the latest MacOS.

        wget https://download.qt.io/official_releases/qt/6.7/6.7.3/single/qt-everywhere-src-6.7.3.tar.xz
        tar xf qt-everywhere-src-6.7.3.tar.xz
        cd qt-everywhere-6.7.3

Run configure with the following options.
* Specify the installation path 'speedcrunch-nightlies/qt-static'
* Build static binaries to avoid runtime dependencies to Qt
* Build only the submodules required by SpeedCrunch

        ./configure -prefix -static ../qt-static -submodules qtbase,qttools,qtdeclarative

Qt is now configured for building. Build it.

        cmake --build .

Install Qt to the installation path.

        cmake --install .

Qt will be installed into 'speedcrunch-nightlies/qt-static'

## SpeedCrunch

Pull the SpeedCrunch source.

        cd ..
        git submodule init
        git submodule update

Build SpeedCrunch.
* Point Cmake to Qt

        cd build
        cmake -DCMAKE_PREFIX_PATH=`realpath ../qt-static/lib/cmake` ../speedcrunch/src
        make

Generate a SpeedCrunch package.

        make package

Your package will be saved as 'speedcrunch-nightlies/SpeedCrunch.dmg'.
