some tv box firmware for h96 max x3

### linux 4.9.269 kernel image from coreelec 
  git clone https://github.com/CoreELEC/linux-amlogic.git -b amlogic-4.9-19

### linux 5.4.219 kernel image from google
  git clone https://android.googlesource.com/kernel/hikey-linaro -b android-amlogic-bmeson-5.4

### linux 5.10.150 kernel image from google
  !!! NOT SUPPORT SIMPLE HDMI NOT WORK OKAY, PLUG HDMI WILL REJECT BOOT !!!

### linux 5.15.x kernel image from google
  !!! NOT SUPPORT SIMPLE HDMI NOT WORK OKAY, ALWAYS NOT HDMI OUTPUT !!!

### armbian dist image

### toolchain
  gcc-arm-10.3-2021.07-x86_64-aarch64-none-elf

### create hostapd use nmcli
```
nmcli con add type wifi ifname wlan0 con-name s905x3 autoconnect yes ssid S905X3_SSID
nmcli con modify s905x3 802-11-wireless.mode ap 802-11-wireless.band a 802-11-wireless.channel 36 ipv4.method shared
nmcli con modify s905x3 wifi-sec.key-mgmt wpa-psk
nmcli con modify s905x3 wifi-sec.psk "12345678"
nmcli con up s905x3
```
NOTE: this tvbox could not support 802.11ac when use channel 149~165, so please use channel 36-64

### bug fixed dtsi for wireless card if speed is slow

```
diff --git a/arch/arm64/boot/dts/amlogic/meson-g12-common.dtsi b/arch/arm64/boot/dts/amlogic/meson-g12-common.dtsi
index 1ef0f2b..3cc89ce 100644
--- a/arch/arm64/boot/dts/amlogic/meson-g12-common.dtsi
+++ b/arch/arm64/boot/dts/amlogic/meson-g12-common.dtsi
@@ -2318,13 +2323,14 @@
                                clocks = <&xtal>, <&clkc CLKID_UART0>, <&xtal>;
                                clock-names = "xtal", "pclk", "baud";
                                status = "disabled";
                                fifo-size = <128>;
                        };
                };

                sd_emmc_a: sd@ffe03000 {
                        compatible = "amlogic,meson-axg-mmc";
                        reg = <0x0 0xffe03000 0x0 0x800>;
-                       interrupts = <GIC_SPI 189 IRQ_TYPE_EDGE_RISING>;
+                       interrupts = <GIC_SPI 189 IRQ_TYPE_LEVEL_HIGH>;
                        status = "disabled";
                        clocks = <&clkc CLKID_SD_EMMC_A>,
                                 <&clkc CLKID_SD_EMMC_A_CLK0>,
```
make -j$(nproc --all) O=out \
                      ARCH=arm64 \
                      CC=clang \
                      CLANG_TRIPLE=aarch64-linux-gnu- \
                      CROSS_COMPILE=aarch64-linux-android- \
                      CROSS_COMPILE_ARM32=arm-linux-androideabi-
