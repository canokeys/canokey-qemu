# CanoKey QEMU

This repo can be used by [QEMU](https://github.com/canokeys/qemu) to provide a virtual CanoKey to the guest OS.

This is only for testing purpose. There is no warranty on the security.

## Install

This repo produces `canokey-qemu.h` and `libcanokey-qemu.so` (or `.a` on macOS).

```bash
git clone --recursive https://github.com/canokeys/canokey-qemu
cd canokey-qemu
cmake -S . -B build -D CMAKE_INSTALL_FULL_LIBDIR
cmake --build build
cmake --install build
```

## Doc

See <https://www.qemu.org/docs/master/system/devices/canokey.html>
