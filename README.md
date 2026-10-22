# speedcrunch-nightlies
Repository to build macOS binaries from the official [SpeedCrunch repository](https://github.com/heldercorreia/speedcrunch).

Please check the [Releases](https://github.com/tsengf/speedcrunch-nightlies/releases) to download the latest builds.

# Building

Install Xcode with its command-line tools, CMake, Ninja, and Git before building. Run the following commands from the root of this repository.

## Qt
SpeedCrunch requires Qt 6.4 or newer. Choose one of the following ways to get Qt, then continue to the SpeedCrunch build instructions for that option.

### Option 1: Install Qt with MacPorts

If you use [MacPorts](https://www.macports.org/), install the required Qt libraries and tools:

```sh
sudo port install qt6-qtbase qt6-qttools
```

Skip this command if these ports are already installed. With the default MacPorts prefix, Qt is installed under `/opt/local/libexec/qt6`. This option uses shared Qt libraries; the SpeedCrunch packaging step bundles the required libraries into the application.

### Option 2: Build static Qt from source

To build Qt yourself with static libraries, install wget and download version 6.12.0 from [Qt Downloads](https://download.qt.io/official_releases/qt/).

```sh
wget https://download.qt.io/official_releases/qt/6.12/6.12.0/single/qt-everywhere-src-6.12.0.tar.xz
tar xf qt-everywhere-src-6.12.0.tar.xz
mkdir qt-build
cd qt-build
```

Run configure from the separate build directory with the following options:

* Install Qt into `qt-6.12.0-static` in the repository root
* Build static libraries to avoid runtime dependencies on Qt
* Build only the submodules required by SpeedCrunch

```sh
../qt-everywhere-src-6.12.0/configure -prefix "$PWD/../qt-6.12.0-static" -static -submodules qtbase,qttools,qtdeclarative
```

Return to the repository root, then build and install Qt.

```sh
cd ..
cmake --build qt-build --parallel
cmake --install qt-build
```

Qt will be installed into `qt-6.12.0-static` in the repository root.

## SpeedCrunch

Clone the SpeedCrunch source from the repository root if you have not already done so.

```sh
git clone https://github.com/heldercorreia/speedcrunch.git
```

Use the commands below for your chosen Qt option. Keep separate build directories when switching Qt installations so CMake does not reuse a cached Qt configuration.

### With MacPorts Qt

```sh
cmake -S speedcrunch/src -B build-macports \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_PREFIX_PATH=/opt/local/libexec/qt6
cmake --build build-macports --parallel
```

If MacPorts uses a custom installation prefix, replace `/opt/local` with that prefix.

Generate a SpeedCrunch package:

```sh
cmake --build build-macports --target package
```

Your package will be created as `build-macports/SpeedCrunch.dmg`.

### With static Qt built from source

Configure CMake to use the Qt installation built above, then build SpeedCrunch.

```sh
mkdir -p build
cmake -S speedcrunch/src -B build -DCMAKE_PREFIX_PATH="$PWD/qt-6.12.0-static"
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
