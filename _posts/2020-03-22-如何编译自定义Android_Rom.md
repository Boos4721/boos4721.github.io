# 如何编译自定义 Android ROM

## 前言


## 准备工作
1. 一台服务器，要求如下：
   * 一个 x86_64 架构的 CPU，性能越强越好。
   * 至少 8G 运行内存（Android 10 要求 16G）。
   * 有至少 200G 的空闲磁盘空间。

2. 有 Git 基本常识。

## 开始（以使用 Ubuntu 18.04 编译 LineageOS 17.1 为例）
1. 安装编译 Android 所用到的软件包：
   ```bash
   apt-get update && apt-get upgrade -y
   apt-get install -y openjdk-8-jdk git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip python bc imagemagick ccache schedtool libssl-dev repo patchelf
   ```

2. 设置 Git 信息。
   ```bash
   git config --global user.name "在此处替换成你的名字"
   git config --global user.email "在此处替换成你的邮箱"
   ```

3. 建立一个文件夹并进入。
   ```bash
   mkdir LineageOS && cd LineageOS
   ```

3. 使用 LineageOS 的树初始化本地仓库
   ```bash
   repo init --no-clone-bundle -u git://github.com/LineageOS/android.git -b lineage-17.1
   ```
   如果你的网络状况不佳或只想编译 ROM，请在上面的命令后面加入 `--depth=1` 参数来仅拉取一层提交历史。

4. 开始同步源代码。
   ```bash
   repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
   ```

5. 开始构建。
   加载编译环境：
   ```bash
   source build/envsetup.sh
   ```

   接下来分为三种情况：
   1. 有官方维护：
      ```bash
      brunch <设备代号>
      ```
      即可。

   2. 无官方维护，但已有非官方源码：
      1. 去将编译需要用的源码（device，kernel，vendor）从 Git 托管代码站 Clone 到相应位置（一般是 `<device/kernel/vendor>/设备厂商名/设备代号`）。
      2. 使用 `brunch <设备代号>` 命令开始编译。

   3. 无官方维护，且无非官方源码，但是有其他自定义 ROM 的源码：
      1. 去将编译需要用的源码（device，kernel，vendor）从 Git 托管代码站 Clone 到相应位置（一般是 `<device/kernel/vendor>/设备厂商名/设备代号`）。
      2. 在此基础上作 ify（[参考这里](https://github.com/GuaiYiHu/android_device_xiaomi_clover-oss/commit/c9426d95559576e61f7544443a24df42786b7aab)）。
      3. 使用 `brunch <设备代号>` 命令开始编译。

6. 编译完成
编译完成后，你可以在 `out/target/product/<设备代号>` 下找到你编译出来的刷机包，刷入即可。

## 注
1. 不要使用任何没有名誉的开发者的源码。
2. 建议使用第三方 ROM 官方设备仓库的源码。

先写这点，后面再继续补充。。

[原文](https://akr-developers.com/d/107-android-rom "原文")
