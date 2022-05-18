cmake_minimum_required(VERSION 3.7)
project(canokey-qemu C)
include(GNUInstallDirs)

set(CMAKE_C_STANDARD 11)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -fno-omit-frame-pointer")

option(QEMU_DEBUG_OUTPUT "Control ENABLE_DEBUG_OUTPUT in core" OFF)

set(VIRTCARD ON CACHE BOOL "Enable VIRTCARD in core" FORCE)
if (NOT QEMU_DEBUG_OUTPUT)
    set(ENABLE_DEBUG_OUTPUT OFF CACHE BOOL "Disable ENABLE_DEBUG_OUTPUT in core" FORCE)
endif ()
set(USBD_PRODUCT_STRING "CanoKey QEMU")
add_subdirectory(canokey-core EXCLUDE_FROM_ALL)

set(LIBCANOKEY_QEMU_SO_VERSION 0)
set(LIBCANOKEY_QEMU_PREFIX ${CMAKE_INSTALL_PREFIX})
set(LIBCANOKEY_QEMU_LIBDIR ${CMAKE_INSTALL_FULL_LIBDIR})
set(LIBCANOKEY_QEMU_INCLUDEDIR ${CMAKE_INSTALL_FULL_INCLUDEDIR})

file(GLOB_RECURSE SRC Src/*.c)

add_library(canokey-qemu SHARED ${SRC}
        canokey-core/virt-card/device-sim.c
        canokey-core/virt-card/fabrication.c
        canokey-core/littlefs/bd/lfs_filebd.c)
set_target_properties(canokey-qemu PROPERTIES PUBLIC_HEADER Inc/canokey-qemu.h)
set_target_properties(canokey-qemu PROPERTIES SOVERSION ${LIBCANOKEY_QEMU_SO_VERSION})
target_include_directories(canokey-qemu SYSTEM PRIVATE Inc canokey-core/virt-card canokey-core)
target_compile_definitions(canokey-qemu PRIVATE HW_VARIANT_NAME="CanoKey QEMU")
target_link_libraries(canokey-qemu general canokey-core)
add_dependencies(canokey-qemu gitrev)
install(TARGETS canokey-qemu LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

configure_file(canokey-qemu.pc.in canokey-qemu.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/canokey-qemu.pc
        DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}/pkgconfig)
