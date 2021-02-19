# 怎么编译内核？

1. 确保编译环境，在ubuntu 20.04LST上，或者ubuntu 18.04LST.

2. 确保你拥有良好的网络以便更新工具

   ```shell
   #!/bin/bash
   sudo apt update
   sudo apt install -y bc build-essential ncurses* ncurses-dev libncurses5-dev bzip2 device-tree-compile -y
   sudo apt install bison build-essential curl flex git gnupg gperf liblz4-tool libncurses5-dev libsdl1.2-dev libxml2 libxml2-utils lzop pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev -y
   sudo apt install g++-multilib gcc-multilib lib32ncurses5-dev lib32z1-dev -y
   sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev libesd-java lib32readline-dev lib32readline6 -y
   sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev libesd-java lib32readline-dev lib32readline7 -y
   sudo apt install libffi-dev libssl-dev libxml2-dev libxslt1-dev libjpeg8-dev zlib1g-dev -y
   sudo apt install git ccache automake flex lzop bison gperf build-essential zip curl zlib1g-dev zlib1g-dev:i386 g++-multilib python-networkx libxml2-utils bzip2 libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl bc libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip -y
   sudo apt install git ccache automake flex lzop bison   gperf build-essential zip curl zlib1g-dev zlib1g-dev:i386   g++-multilib python-networkx libxml2-utils bzip2 libbz2-dev   libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush   schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev   pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl   libxml-simple-perl bc libc6-dev-i386 lib32ncurses5-dev   x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip   mosh htop lsof screen -y
   
   ```

   

3. 确保上述过程顺利进行，然后执行如下操作

   ```shell
   export ARCH=arm64
   export SUBARCH=arm64
   export HEADER_ARCH=arm64
   make clean
   make mrproper
   make ARCH=arm64 compile_defconfig
   ```

   这个配置（compile_defconfig）是LineageOS 17.1，oneplus 6 安卓源代码编译成功后，在out/out/target/product/enchilada/obj/KERNEL_OBJ中寻找到的.config复制而来，由于不确定官方用的是哪个defconfig，所以直接用现成的，确保和LineageOS官方的基本配置一致。

   

   如果需要自定义，执行make ARCH=arm64 menuconfig，最好执行完上述代码后再执行该命令。

   

4. 开始编译

   ```shell
   make -j$(nproc - 1) O=./out/ ARCH=arm64 SUBARCH=arm64 HEADER_ARCH=arm64
   CROSS_COMPILE=compile_tools/android10-aarch64/bin/aarch64-linux-android-
   CROSS_COMPILE_ARM32=compile_tools/android10-arm/bin/arm-linux-androideabi-
   ```

   

十分建议在编译的时候建立交换空间（swap虚拟内存），特别是使用笔记本电脑进行编译的时候，大小最好为物理内存的一倍。否则很容易死机。

## 刷入内核
编译成功后在out/arch/arm64/boot中找到image.gz-dtb，放入Anykernel3文件夹中，将Anykernel3下所有的文件打包成一个zip压缩包，在rec中刷入即可。（如果用到adb，可以用adb sideload YOUR_HOST_PATH/YOUR_ZIP_NAME.zip）

# 注意

刷人内核具有一定风险，可能造成各种系统功能异常，如果你没有做好心理准备不要胡乱刷入。本项目仅为学习所用，不承担因为刷机所造成的任何责任。

# 致谢

内核来源于[LineageOS](https://github.com/LineageOS/android_kernel_oneplus_sdm845)

目前存在的rtl8812au驱动来自[aricrack-ng](https://github.com/aircrack-ng/rtl8812au)
内核编译步骤参考[maozhifeng](https://github.com/maozhifeng/clang_and_gcc_auto_make_kernel)

