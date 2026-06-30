# 🚀 About this repo

This repo (`sign` branch) is based on [Lineage OS 22.2 xiaomi sm8250 kernel source](https://github.com/LineageOS/android_kernel_xiaomi_sm8250).

The main purpose of maintaining and building this kernel is to fix [this battery stuck at 1% problem](https://github.com/liyafe1997/Xiaomi-fix-battery-one-percent), and provide [KernelSU Ultra-Legacy](https://github.com/backslashxx/KernelSU) (A KernelSU fork) integrated pre-built image (Flashable AnyKernel). Also provides a more intuitive and easy-to-use build script and build guide that allow you to try to build by yourself.

(The devices affected by the "1% battery bug" are: alioth, apollo, lmi, thyme, umi, pipa. Because they all use the PM8150, aka Qualcomm fuel gauge GEN4. For the other devices are not affected by that bug, you can use this kernel for KernelSU purpose, as a replacement of the orginal stock kernel. Also, as the people tested, this kernel NoKernelSU version is good for applying [APatch](https://github.com/bmax121/APatch)).

The pre-built kernel image/zip is built on the `sign` branch. It should works on both stock MIUI and third-party AOSP based Android 11-15 ROMs. You are welcomed to give feedback (Issues/Pull Requests)!

*⚙️ Note: The zip does not include the `dtbo.img` and it will not replace your `dtbo` partition. It is recommanded to keep the stock `dtbo` or the `dtbo` from your third-party rom (If the builder comfirmed it works well). Since there are some problems with the `dtbo.img` which built from this source, one of them is the screen will suddently goes to the highest brightness when shut try to shut off the screen in the lock screen. If you had flashed any other third-party kernels, and you got some weird problem, you should keep an eye to check your `dtbo` has been replaced or not.*

Supported devices:
| Code Name | Device Name                          |
|-----------|--------------------------------------|
| psyche    | Xiaomi Mi 12X                        |
| thyme     | Xiaomi Mi 10S                        |
| umi       | Xiaomi Mi 10                         |
| munch     | Poco F4 / Redmi K40S                 |
| lmi       | Redmi K30 Pro                        |
| cmi       | Xiaomi Mi 10 Pro                     |
| cas       | Xiaomi Mi 10 Ultra                   |
| apollo    | Xiaomi Mi 10T / Redmi K30S Ultra     |
| alioth    | Xiaomi Mi 11X / POCO F3 / Redmi K40  |
| elish     | Xiaomi Pad 5 Pro                     |
| enuma     | Xiaomi Pad 5 Pro 5G                  |
| dagu      | Xiaomi Pad 5 Pro 12.4                |
| pipa      | Xiaomi Pad 6                         |

Other Features/Improvement of this Kernel:
1. Support USB Serial (CH340/FTDI/PL2303/OTI6858/TI/SPCP8X5/QT2/UPD78F0730/CP210X).
2. Support EROFS.
3. F2FS realtime discard enabled for better TRIM the flash.
4. Support CANBus and USB CAN adapter (Like CANable).
5. Support LZ4, LZ4HC, ZSTD compression algorithms for ZRAM.


# 🛠️ How to build

1. Prepair the basic build environment.

    You have to have the basic common toolchains, such as `git`, `make`, `curl`, `bison`, `flex`, `zip`, etc, and some other packages.
    In Debian/Ubuntu, you can
    ```
    sudo apt install build-essential git curl wget bison flex zip bc cpio libssl-dev ccache
    ```
    And also, you have to have `python` (Only `python3` is not enough). you can install the apt package `python-is-python3`.

    In RHEL/RPM based OS, you can
    ```
    sudo yum groupinstall 'Development Tools'
    sudo yum install wget bc openssl-devel ccache
    ```

    Notice: `ccache` is enabled in `sign.sh` for speed up the compiling. `CCACHE_DIR` has been set as `$HOME/.cache/ccache_mikernel` in `sign.sh`. If you don't like you can remove or modify it.

2. Download [ReClang] compiler toolchain

    You have to have `aarch64-linux-gnu`, `arm-linux-gnueabi`, `clang`. [ReClang](https://github.com/SaleOff/ReClang/) is a good prebuilt clang cross compiler toolchain.

    The toolchain path is `$HOME/ReClang/ReClang-Signed/bin` which hasn't been set in `sign.sh`. Please change `TOOLCHAIN_PATH` in `sign.sh` before using it.

    ```
    mkdir ReClang
    cd ReClang
    wget https://github.com/SaleOff/ReClang/archive/refs/tags/Signed.zip
    unzip Signed.zip
    cd ..
    ```

3. Build

    Build without KernelSU: 
    ```
    bash sign.sh TARGET_DEVICE
    ```
    
    Build with KernelSU:
    ```
    bash sign.sh TARGET_DEVICE ksu
    ```

    For example, build for lmi (Redmi K30 Pro/POCO F2 Pro) without KernelSU:
    ```
    bash sign.sh lmi
    ````

    For example, build for umi (Mi 10) with KernelSU:
    ```
    bash sign.sh umi ksu
    ```

    Thanks for watching.

