# Build Instructions

## Windows x86.

### Prerequisites

Install and run [Visual Studio Installer](https://visualstudio.microsoft.com/downloads/). Select `Desktop development with C++` and ensure `MSVC` and a corresponding Windows SDK are installed. You may also install **cmake** separately from [cmake.org](https://cmake.org/download/).

### Opening command prompt

Run `Developer command prompt for VS` or a regular Windows `cmd` if **cmake** was installed separately.

Navigate to the hlsdk directory:
```
cd C:\Users\username\projects\hlsdk-portable
```

### Building

Configure the project:
```
cmake -A Win32 -B build
```
Compile the libraries:
```
cmake --build build --config Release
```
`hl.dll` and `client.dll` will appear in `build/dlls/Release` and `build/cl_dll/Release`.

To install to a mod directory, set **GAMEDIR** and **CMAKE_INSTALL_PREFIX**:
```
cmake -A Win32 -B build -DGAMEDIR=mod -DCMAKE_INSTALL_PREFIX="C:\Program Files (x86)\Steam\steamapps\common\Half-Life"
cmake --build build --config Release --target install
```

#### Choosing Visual Studio version

Specify a Visual Studio version:
```
cmake -G "Visual Studio 16 2019" -A Win32 -B build
```

### Editing code in Visual Studio

After configuration, `HLSDK-PORTABLE.sln` appears in `build`. Open it in Visual Studio to continue development.

## Linux x86. Portable steam-compatible build

### Prerequisites

Install compilers, cmake, and SDL2:
```
sudo apt install cmake build-essential gcc-multilib g++-multilib libsdl2-dev:i386
```

### Building

```
cmake -DCMAKE_BUILD_TYPE=Release -B build -S .
cmake --build build
```

For Steam compatibility, build statically:
```
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="${CMAKE_CXX_FLAGS} -static-libstdc++ -static-libgcc" -B build -S .
cmake --build build
```

## Android

### Prerequisites

1. Set up [Android Studio/Android SDK](https://developer.android.com/studio).

### Android Studio

Open the `android` folder and build.

### Command-line
```
cd android
./gradlew assembleRelease
```

## Nintendo Switch

### Prerequisites

1. Install [`dkp-pacman`](https://devkitpro.org/wiki/devkitPro_pacman).
2. Install dependencies:
```
sudo dkp-pacman -S switch-dev dkp-toolchain-vars switch-mesa switch-libdrm_nouveau switch-sdl2
```
3. Set `DEVKITPRO`:
```
export DEVKITPRO=/opt/devkitpro
```
4. Install libsolder:
```
source $DEVKITPRO/switchvars.sh
git clone https://github.com/fgsfdsfgs/libsolder.git
make -C libsolder install
```

### Building
```
mkdir build && cd build
aarch64-none-elf-cmake -G"Unix Makefiles" -DCMAKE_PROJECT_HLSDK-PORTABLE_INCLUDE="$DEVKITPRO/portlibs/switch/share/SolderShim.cmake" ..
make -j
```

## PlayStation Vita

### Prerequisites

1. Set up [VitaSDK](https://vitasdk.org/).
2. Install [vita-rtld](https://github.com/fgsfdsfgs/vita-rtld):
```
git clone https://github.com/fgsfdsfgs/vita-rtld.git && cd vita-rtld
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j2 install
```

### Building
```
./waf configure -T release --psvita
./waf build
```

