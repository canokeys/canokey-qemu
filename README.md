# CanoKey QEMU

This repo can be used by [QEMU](https://github.com/canokeys/qemu) to provide a virtual canokey to the guest OS

This is only for testing purpose. There is no warranty on the security.

## Install

This repo produces `canokey-qemu.h` and `libcanokey-qemu.so`

```bash
git clone --recursive https://github.com/canokeys/canokey-qemu
cd canokey-qemu
mkdir build; cd build
cmake .. -DCMAKE_INSTALL_PREFIX=${YOUR_PREFIX}
make -j`nproc`
make install
```

## Install (Mac)
Follow the regular install instructions, and then after cloning the repo and cd'ing into it, add the code below to the very top of the file `canokey-core/canokey-crypto/mbedtls/library/CMakeLists.txt`.  Then follow the rest of the regular instructions.:
```
if(${APPLE})
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-but-set-variable -Wno-unused-but-set-parameter")
endif()
```

## Doc

See <https://www.qemu.org/docs/master/system/devices/canokey.html>
