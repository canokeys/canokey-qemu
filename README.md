# CanoKey QEMU

[![Build](https://github.com/canokeys/canokey-qemu/actions/workflows/build.yml/badge.svg)](https://github.com/canokeys/canokey-qemu/actions/workflows/build.yml)

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

## Compatibility Warning

The storage layout used by v1 builds is not fully compatible with v0
CanoKey QEMU images.

The main breaking change is CTAP discoverable credential storage:

- v0: resident keys were stored in `ctap_rk` as `CTAP_residentKey`
  records.
- v1: discoverable credentials are stored in `ctap_dc` as
  `CTAP_discoverable_credential` records, with extra metadata in `ctap_dm`.
- New code does not migrate or read the old `ctap_rk` file.

Impact:

- After upgrading from an old image to a new image, previously registered FIDO2
  resident/discoverable credentials will no longer be visible to the new code.
- This is expected incompatibility, not on-disk corruption.

Other format changes are additive only:

- New PIV files may be created for new slots and objects.
- CTAP adds `SM2_ATTR` to `ctap_cert`.
- Pass adds a new `pass` file for the new applet.

OATH also drops `ATTR_DEFAULT_RECORD`. Old data with that attribute is ignored,
but "default credential" behavior may differ from older builds.

## Versioning And Tagging

Because the storage layout changed incompatibly, this repository now installs
`libcanokey-qemu.so.1` instead of `.so.0`.

## Doc

See <https://www.qemu.org/docs/master/system/devices/canokey.html>
