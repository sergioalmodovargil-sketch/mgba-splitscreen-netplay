# mGBA Splitscreen Netplay

Play link-cable Game Boy Advance games with 2–4 players in one libretro core. The original mGBA Splitscreen project provides local splitscreen. This build adds synchronization for RetroArch Netplay, including the player-count selector and the emulated link cable.

## Credits

- [Spuds0588 / mGBA Splitscreen](https://github.com/Spuds0588/mgba-splitscreen) created the original multiscreen core and local link-cable implementation.
- [endrift and the mGBA contributors](https://github.com/mgba-emu/mgba) created and maintain the emulator on which it is based.

This is an independent derivative build based on [upstream commit `a8cb708`](https://github.com/Spuds0588/mgba-splitscreen/commit/a8cb708e04be7d0ba8eabfd6b44349a43bc59607). The [MPL 2.0 license](LICENSE) and [source patch](mgba-splitscreen-netplay.patch) are included.

## Install on Windows x64

1. Download the Windows x64 ZIP from [Releases](../../releases) and extract it.
2. Close RetroArch or RetroBat. Back up an existing `mgba_splitscreen_libretro.dll`, then copy the new DLL into the configured **Cores** directory and the `.info` file into **Core Info**. In RetroBat, these are usually `emulators/retroarch/cores` and `emulators/retroarch/info` under the installation folder.
3. In RetroBat, use the existing GBA multiplayer system (`gba2players`). If its game menu does not show Netplay, put the included `es_features_gba2players_netplay.cfg` next to the active `es_features.cfg` and restart RetroBat. RetroArch can also host or join directly.
4. Use this same core build and the same ROM on every machine. The host selects the number of players. When prompted by the game, choose its link-cable multiplayer mode.

## Test coverage

The same source was tested with the Linux build on RetroArch 1.22.2 over a local network. A diagnostic GBA ROM confirmed controller input and emulated link-cable transfers with 2, 3 and 4 players, including clients joining after emulation started. The Windows package is a cross-compiled x86-64 build.

## Build the Windows DLL

Use Git, CMake and LLVM-MinGW on your PATH. From this repository, run the commands below in a shell.

```sh
git clone https://github.com/Spuds0588/mgba-splitscreen.git upstream
cd upstream
git checkout a8cb708e04be7d0ba8eabfd6b44349a43bc59607
git apply ../mgba-splitscreen-netplay.patch
cmake -S . -B build-netplay -DCMAKE_SYSTEM_NAME=Windows \
  -DCMAKE_C_COMPILER=x86_64-w64-mingw32-clang \
  -DCMAKE_CXX_COMPILER=x86_64-w64-mingw32-clang++ \
  -DCMAKE_RC_COMPILER=x86_64-w64-mingw32-windres \
  -DLIBMGBA_ONLY=ON -DBUILD_LIBRETRO_SPLITSCREEN=ON \
  -DCMAKE_BUILD_TYPE=Release -DUSE_ZLIB=OFF -DUSE_PNG=OFF
cmake --build build-netplay --target mgba_splitscreen_libretro --parallel
llvm-strip --strip-unneeded build-netplay/mgba_splitscreen_libretro.dll
```
