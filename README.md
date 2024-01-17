# FXLS89xx_LinuxModulePatch
This patch offers users to use NXP FXLS89xx newer devices in Linux module. The default Linux kernel module only offers the function to FXLS8962AF and FXLS8964AF due to WHO_AM_I detection. This patch expands the range of support to FXLS8961AF, FXLS8967AF, FXLS8971CF, and FXLS8974CF.

## Supported devices in this patch and datasheet Information
Device Information|Datasheet
---|---
[FXLS8961AF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-accelerometer:FXLS8961AF)|[FXLS8961AF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8961AF.pdf)
[FXLS8967AF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-accelerometer:FXLS8967AF)|[FXLS8967AF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8967AF.pdf)
[FXLS8971CF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-accelerometer:FXLS8971CF)|[FXLS8971CF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8971CF.pdf)
[FXLS8974CF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-iot-accelerometer:FXLS8974CF)|[FXLS8974CF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8974CF.pdf)

## Supported devices in default kernel module
Device Information|Datasheet
---|---
[FXLS8962AF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-accelerometer:FXLS8962AF) (Obsolete)|[FXLS8962AF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8962AF.pdf)
[FXLS8964AF](https://www.nxp.com/products/sensors/accelerometers/2g-4g-8g-16g-low-power-12-bit-digital-accelerometer:FXLS8964AF)|[FXLS8964AF.pdf](https://www.nxp.com/docs/en/data-sheet/FXLS8964AF.pdf)

## How to use
Put this patch into the same directory of header/source to use FXLS8962AF Linux driver files and use patch command to them. The default directory is drivers/iio/accel. Example command to use this patch is below:
```
# cd <Directory to kernel source>/drivers/iio/accel
# cp <File path to this patch> .
# patch < fxls8962af.patch
```

If you wish to build the module and install it into your Raspberry PI Zero/Zero W, then the example commands are like below after patching. For kernel/kernel module build, please refer to [Official documents](https://www.raspberrypi.com/documentation/computers/linux_kernel.html).
```
# cd <Directory to kernel source>
# KERNEL=kernel
# make bcmrpi_defconfig
# make menuconfig
--Check <M> at NXP FXLS8962AF/FXLS8964AF Accelerometer I2C Driver
or NXP FXLS8962AF/FXLS8964AF Accelerometer SPI Driver
in Device Driver > Industrial I/O support > Accelerometers--
# make modules_prepare
# make M=drivers/iio/accel modules
# make M=drivers/iio/accel modules_install
# depmod
```

If you also would like to use FXLS8974CF in I2C mode, then you need to register I2C address of 0x18 as FXLS8974CF. The system will bind the device to FXLS8962AF_I2C as FXLS8974CF. The example commands are like below:
```
# raspi-config
--Enable I2C in raspi-config--
# modprobe fxls8962af-i2c
# echo -n fxls8974cf 0x18 > /sys/bus/i2c/devices/i2c-1/new_device
```

You will see the device data in /sys/bus/i2c/drivers/fxls8962af_i2c/1-0018 by sysfs. For example, you can get the raw Z acceleration data by the commands below:
```
$ cd /sys/bus/i2c/drivers/fxls8962af_i2c/1-0018
$ cat iio\:device0/in_accel_z_raw
```
