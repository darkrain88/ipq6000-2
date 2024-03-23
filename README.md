# 适用于 IPQ6000 设备的 OpenWrt 源码仓库

## 注意

1. **不要用 root 用户进行编译**
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1 密码 password

## 编译命令

1. 首先装好 Linux 系统， Ubuntu 20.04 LTS

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
   rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   git clone -b master --single-branch https://github.com/darkrain88/ipq6000-2.git
   cd ipq6000
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 后面是线程数，为便于排除错误推荐用单线程）

   ```bash
   make download -j8
   make -j1 V=s
   ```

5. 二次编译：

   ```bash
   cd ipq6000
   git fetch && git reset --hard origin/master
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make defconfig
   make V=s -j$(nproc)
   ```

6. 如果需要重新配置：

   ```bash
   rm -rf .config
   make menuconfig
   make V=s -j$(nproc)
   ```

7. 编译完成后输出路径：bin/targets

8. 更换go 版本

   rm -rf feeds/packages/lang/golang
   
   git clone https://github.com/sbwml/packages_lang_golang -b 22.x feeds/packages/lang/golang
   
10. 安装 mosdns
rm -rf feeds/packages/net/v2ray-geodata

git clone https://github.com/sbwml/luci-app-mosdns -b v5 package/mosdns

git clone https://github.com/sbwml/v2ray-geodata package/v2ray-geodata

make menuconfig # choose LUCI -> Applications -> luci-app-mosdns

make package/mosdns/luci-app-mosdns/compile V=s

10.安装ddns

git clone https://github.com/sirpdboy/luci-app-ddns-go.git package/ddns-go

   make menuconfig

11 添加pw feed

src-git passwall_packages https://github.com/xiaorouji/openwrt-passwall-packages.git;main

src-git passwall https://github.com/xiaorouji/openwrt-passwall.git;main



