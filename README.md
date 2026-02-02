# SRG_PICO-IMX8PL-BSP
Building Debian Images with Flexbuild
  
# Build image for EVK
$ mkdir YOUR_FLEXBUILD_DIR  
$ cd YOUR_FLEXBUILD_DIR  
$ git clone https://github.com/NXP/flexbuild.git  
$ cd flexbuild/  
$ . setup.env  
$ bld docker  
$ . setup.env  
$ bld host-dep  
$ bld -m imx8mpevk  
  
# Build image for SRG-IMX8PL
Clone the patch & config files  
$ cd YOUR_FLEXBUILD_DIR/flexbuild  
$ git clone https://github.com/BSP-Dev/SRG_PICO-IMX8PL-BSP.git  
 
Copy configuration file  
$ cd YOUR_FLEXBUILD_DIR/flexbuild  
$ cp YOUR_FLEXBUILD_DIR/flexbuild/SRG_PICO-IMX8PL-BSP/imx8mpevk.conf configs/board/imx8mpevk.conf  

Patch BSP component “atf“  
$ cd YOUR_FLEXBUILD_DIR/flexbuild/components_lsdk2506/bsp/atf/  
$ cp YOUR_FLEXBUILD_DIR/flexbuild/SRG_PICO-IMX8PL-BSP/srg_imx8pl_atf_8_128.patch .  
$ git apply srg_imx8pl_atf_8_128.patch  
  
Patch BSP component uboot  
$ cd YOUR_FLEXBUILD_DIR/flexbuild/components_lsdk2506/bsp/uboot  
$ cp YOUR_FLEXBUILD_DIR/flexbuild/SRG_PICO-IMX8PL-BSP/srg_imx8pl_uboot_8_128.patch .  
$ git apply srg_imx8pl_uboot_8_128.patch  
$ cp YOUR_FLEXBUILD_DIR/flexbuild/SRG_PICO-IMX8PL-BSP/imx8mp_evk_defconfig configs/imx8mp_evk_defconfig  

Patch kernel linux  
$ cd YOUR_FLEXBUILD_DIR/flexbuild/components_lsdk2506/linux/linux  
$ cp YOUR_FLEXBUILD_DIR/flexbuild/SRG_PICO-IMX8PL-BSP/srg_imx8pl_linux_8_128.patch .  
$ git apply srg_imx8pl_linux_8_128.patch  

After patching, please run the following commands to rebuild the components  
$ bld atf -m imx8mpevk -b sd  
$ bld uboot -m imx8mpevk  
$ bld boot  
$ bld -m imx8mpevk  
  
# Creating an image of SD card  
Download flex-installer  
$ wget http://www.nxp.com/lgfiles/sdk/lsdk2412/flex-installer  
$ chmod +x flex-installer; sudo mv flex-installer /usr/bin  
  
Generate "sdcard.wic"  
$ cd YOUR_FLEXBUILD_DIR/flexbuild/build_lsdk2506/images  
$ flex-installer -i mkwic -m imx8mpevk -f firmware_imx8mpevk_sdboot.img -b boot_IMX_arm64_lts_6.6.52.tar.zst -r rootfs_lsdk2506_debian_desktop_arm64.tar.zst  
  
This command will generate an image file “sdcard.wic”, You could use “dd” or balenaEtcher (https://etcher.balena.io/) to flash SD card.  

Please note that the rootfs here is the Debian Desktop provided by NXP Flexbuild.  

