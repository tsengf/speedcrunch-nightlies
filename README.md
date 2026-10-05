# speedcrunch-nightlies
Repository to build macOS binaries from the official [SpeedCrunch repository](https://github.com/heldercorreia/speedcrunch).

Please check the [Releases](https://github.com/tsengf/speedcrunch-nightlies/releases) to download the latest builds.

# Building

Install Xcode with its command-line tools, CMake, Ninja, Git, and wget before building. Run the following commands from the root of this repository.

## Qt
SpeedCrunch requires Qt. These instructions use version 6.11.2 from [Qt Downloads](https://download.qt.io/official_releases/qt/).

```sh
wget https://download.qt.io/official_releases/qt/6.11/6.11.2/single/qt-everywhere-src-6.11.2.tar.xz
tar xf qt-everywhere-src-6.11.2.tar.xz
mkdir qt-build
cd qt-build
```

Run configure from the separate build directory with the following options:

* Install Qt into `qt-6.11.2-static` in the repository root
* Build static libraries to avoid runtime dependencies on Qt
* Build only the submodules required by SpeedCrunch

```sh
../qt-everywhere-src-6.11.2/configure -prefix "$PWD/../qt-6.11.2-static" -static -submodules qtbase,qttools,qtdeclarative
```

Return to the repository root, then build and install Qt.

```sh
cd ..
cmake --build qt-build --parallel
cmake --install qt-build
```

Qt will be installed into `qt-6.11.2-static` in the repository root.

## SpeedCrunch

Clone the SpeedCrunch source from the repository root.

```sh
git clone https://github.com/heldercorreia/speedcrunch.git
```

Create the build directory, configure CMake to use the installed Qt, and build SpeedCrunch.

```sh
mkdir -p build
cmake -S speedcrunch/src -B build -DCMAKE_PREFIX_PATH="$PWD/qt-6.11.2-static"
cmake --build build --parallel
```

Generate a SpeedCrunch package.

```sh
cmake --build build --target package
```

Your package will be created as `build/SpeedCrunch.dmg`.

# Installation

Install 'SpeedCrunch.dmg'.

In a terminal, type

        xattr -c /Applications/SpeedCrunch.app
