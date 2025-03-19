# speedcrunch-nightlies
Repository to build MacOS binaries from the official SpeedCrunch repository.

Instructions taken from
https://bitbucket.org/heldercorreia/speedcrunch/wiki/BuildingOSXPackage

Please check the [Releases list](https://github.com/tsengf/speedcrunch-nightlies/releases) to download the latest builds.

# Building

## Qt
SpeedCrunch depends on Qt. I was able to get only version 6.7.3 to successfully build on the latest MacOS.

    wget https://download.qt.io/official_releases/qt/6.7/6.7.3/single/qt-everywhere-src-6.7.3.tar.xz
    tar xf qt-everywhere-src-6.7.3.tar.xz
    cd qt-everywhere-6.7.3

Only build the submodules required by SpeedCrunch. Build static binaries to avoid QT dependencies at runtime.

    ./configure -static -prefix ../qt-static -submodules qtbase,qttools,qtdeclarative

Qt is now configured for building. Just run 'cmake --build . --parallel'

    cmake --build . --parallel

Once everything is built, you must run 'cmake --install .'

    cmake --install .

Qt will be installed into 'speedcrunch-nightlies/qt-static'

## SpeedCrunch

Pull the SpeedCrunch source

    git submodule init
    git submodule update

Build SpeedCrunch

    cd ../build
    cmake -DCMAKE_PREFIX_PATH=`realpath ../qt-static/lib/cmake` ../speedcrunch/src
    make

Generate a SpeedCrunch package

    make package

Your package will be found in 'speedcrunch-nightlies/SpeedCrunch.dmg'.
