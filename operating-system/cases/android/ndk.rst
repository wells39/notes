========================================
Android NDK
========================================


.. contents:: 目錄


介紹 (Introduction)
========================================

Android NDK 裡面會附上 Cross Compile 用的程式，
例如 ``toolchains/`` 資料夾裡面就包含不同平台的 GCC Cross Compiler 的執行檔，
另外則含有 Clang 的 Makefile 設定。
(GCC 的 Cross Compile 需要先編成不同的 GCC Binary，Clang 則是有更好的支援，使用 ``-target`` 參數選擇即可)



安裝 (Installation)
========================================

環境變數設定：

.. code-block:: sh

    ...




Build Standalone Clang Cross Compile Toolchain
==============================================

.. code-block:: sh

    $ android-ndk-r10e/build/tools/make-standalone-toolchain.sh \
        --toolchain=arm-linux-androideabi-clang3.6 \
        --llvm-version=3.6 \
        --arch=arm \
        --abis=armeabi-v7a-hard \
        --platform=android-21 \
        --package-dir=/tmp
    Auto-config: --toolchain=arm-linux-androideabi-4.8, --llvm-version=3.6
    Copying prebuilt binaries...
    Copying sysroot headers and libraries...
    Copying c++ runtime headers and libraries...
    Creating package file: /tmp/arm-linux-androideabi-4.8.tar.bz2
    Cleaning up...
    Done.
    $ file /tmp/arm-linux-androideabi-4.8.tar.bz2
    /tmp/arm-linux-androideabi-4.8.tar.bz2: bzip2 compressed data, block size = 900k



C++ 支援
========================================

* libc++
    - 來自 LLVM，會是未來唯一的選擇
* gnustl
    - GNU C++ Library，在其他平台叫做 libstdc++，Android 上因為避免混淆而重新命名
* STLport
    - STLport 的 Android porting，已經沒有再持續開發
* system
    - /system/lib/libstdc++.so



Cross Compile 注意事項
========================================

* 可以先參考其他平台的編譯設定
    - `Termux packages <https://github.com/termux/termux-packages>`_
    - `Buildroot <https://github.com/buildroot/buildroot/>`_
    - `Arch Linux PKGBUILD <https://www.archlinux.org/packages/>`_
* Android 的 libc 叫 Bionic，功能有刪減過，所以部份函式不存在
    - librt、libpthread、libresolv 都包含在裡面，編譯時不要加上 ``-lrt`` 和 ``-lpthread``
    - 沒有 iconv 函式
    - 沒有 SYS V 的 shm.h/sem.h
    - 沒有和 pwd.h 和 getpwuid_r() 等等
    - 沒有 locale.h
    - 沒有 IPv4/Ipv6 headers/support 例如 ``arpa/*.h``
    - 沒有 intl/gettext 函式
    - 沒有 i18n 支援和 wchar_t
* Android 的 linker
    - 只支援 PIE (Position Independent Executables)
        + Jelly Bean 加入 PIE 支援
        + Lollipop 移除 non-PIE 支援
* 編譯時注意 PIC 設定要一致，例如都是用 ``-fPIC`` ，而不是 ``-fpic`` 和 ``-fPIC`` 混雜
    - 1: ``-fpic`` ，Small-model
    - 2: ``-fPIC`` ，Large-model
* pkg-config 設定



各 Build System 的 Cross Compile
========================================

幸運的狀況是環境變數設好之後就可以成功編起來，
麻煩一點的狀況是要加額外的設定參數，
更麻煩的是要直接改專案的程式碼。


單純的 Makefile
------------------------------

CMake
------------------------------

autotools
------------------------------

CPython Extension
------------------------------

設定 ``_PYTHON_HOST_PLATFORM="linux-aarch64"`` ，
這會直接改變 ``disutils.utils`` 裡 ``get_platform`` 的結果。

* https://github.com/python/cpython/blob/master/Lib/distutils/util.py
* https://github.com/python/cpython/commit/1abe1c5fe13ca64c54e16db25de7c4dd5578a7c1
* ``PYTHON_FOR_BUILD``



compiler-rt
========================================

.. code-block:: sh

    $ ls -al arm-linux-androideabi-4.8/lib/clang/3.6/lib/linux/
    total 7332
    -rw-r----- 1 dv dv 2323826 Apr  1 06:26 libclang_rt.asan-arm-android.a
    -rw-r----- 1 dv dv   36437 Apr  1 06:27 libclang_rt.asan-arm-android.a.syms
    -rwxr-x--- 1 dv dv 1599248 Apr  1 06:27 libclang_rt.asan-arm-android.so*
    -rw-r----- 1 dv dv   16596 Apr  1 06:26 libclang_rt.asan_cxx-arm-android.a
    -rw-r----- 1 dv dv      73 Apr  1 06:26 libclang_rt.asan_cxx-arm-android.a.syms
    -rw-r----- 1 dv dv   14064 Apr  1 06:34 libclang_rt.asan_cxx-i686-android.a
    -rw-r----- 1 dv dv 1986448 Apr  1 06:34 libclang_rt.asan-i686-android.a
    -rwxr-x--- 1 dv dv 1504988 Apr  1 06:35 libclang_rt.asan-i686-android.so*
    -rw-r----- 1 dv dv    2100 Apr  1 06:26 libclang_rt.asan-preinit-arm-android.a
    -rw-r----- 1 dv dv    1844 Apr  1 06:34 libclang_rt.asan-preinit-i686-android.a
    $ file android-ndk-r10e/toolchains/llvm-3.6/prebuilt/linux-x86_64/lib/clang/3.6/lib/linux/libclang_rt.asan-arm-android.so
    android-ndk-r10e/toolchains/llvm-3.6/prebuilt/linux-x86_64/lib/clang/3.6/lib/linux/libclang_rt.asan-arm-android.so: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, BuildID[sha1]=414a26fe76bb09de2d0d101e3ed6200da5d1cadc, not stripped



Build LLVM From Source For Android
========================================



"cpufeatures" Library
========================================

* `The cpufeatures Library <https://developer.android.com/ndk/guides/cpu-features.html>`_



Build Your NDK
========================================



CrystaX NDK
========================================

:Site: https://www.crystax.net/en/

CrystaX NDK 是一套第三方的 Android NDK，
目的是要提供更多的功能來輔助 Android 相關開發。



Reference
========================================

* `Android NDK ABIs <https://developer.android.com/ndk/guides/abis.html>`_
* `Google Code Review - NDK <https://android-review.googlesource.com/#/q/project:platform/ndk>`_
* `AOSP - NDK <https://android.googlesource.com/platform/ndk>`_
* `AOSP - Prebuilts NDK <https://android.googlesource.com/platform/prebuilts/ndk>`_
* `NDK - Release Notes <https://developer.android.com/ndk/downloads/index.html#rel>`_
* `NDK - C++ support <https://developer.android.com/ndk/guides/cpp-support.html>`_

* `AOSP - LLVM <https://android.googlesource.com/platform/external/llvm/>`_
* `AOSP - compiler-rt <https://android.googlesource.com/platform/external/compiler-rt/>`_
* `AOSP - Clang <https://android.googlesource.com/platform/external/clang/>`_

* `Wikipedia - Bionic (software) <https://en.wikipedia.org/wiki/Bionic_(software)>`_
* `Wikipedia - Position-independent code <https://en.wikipedia.org/wiki/Position-independent_code>`_
