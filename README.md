# CanoKey QEMU

This repo can be used by [QEMU](https://github.com/canokeys/qemu) to provide a virtual canokey to the guest OS

This is only for testing purpose. There is no warranty on the security.

## Install

This repo produces `canokey-qemu.h` and `libcanokey-qemu.so`

```bash
git clone https://github.com/canokeys/canokey-qemu
cd canokey-qemu
mkdir build; cd build
cmake .. -DCMAKE_INSTALL_PREFIX=${YOUR_PREFIX}
make -j`nproc`
make install
```

## Install (Mac)
Add the following to the very top of the file `canokey-core/canokey-crypto/mbedtls/library/CMakeLists.txt`:
```
if(${APPLE})
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-but-set-variable -Wno-unused-but-set-parameter")
endif()
```
When building QEMU with the `--enable-canokey` option add the additional option to point to the static library: `--extra-ldflags='-L/usr/local/lib -lcanokey-qemu'`.  Note: if your libraries from `make install` for canokey-qemu went into a directory other than `/usr/local/lib` update the `--extra-ldflags` path as needed.

## Doc

See <https://www.qemu.org/docs/master/system/devices/canokey.html>
