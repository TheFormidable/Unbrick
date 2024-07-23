# Unbrick - Poco x6 Pro - Redmi k70e (duchamp)

The poco x6 pro 'duchamp' uses the chipset MT6897, These chipsets use a new protocol called V6 and the bootrom is patched, thus you need a valid DA (Download_Agent) via -- loader option.

For all devices with  `SLA` (Serial Link Authentication) and `DAA` (Download Agent Authentication) and Remote-Auth activated no public solution currently exists (for various reasons).

## Flashing Eng preloader on `duchamp` and "unbrick" it

## Getting Started

As we have seen in the past [" begonia CFW Megathread"](https://xdaforums.com/t/guide-info-psa-redmi-note-8-pro-megathread-cfw.4056527/), MTK devices are pretty easy to brick. All it takes is one wrong move and you are **FUCKED!**.

The same concept of a brick also applies to `duchamp` devices ["Example of what a brick looks like"](https://imgur.com/a/zMw2udb).

!!! note
    If you are already in this state and have not flashed the Engineering preloader images, the only way out is to visit an authorised Xiaomi Center.

### So what do we do to avoid a brick?

It's not that easy to avoid bricking these devices. Just follow the instructions and don't do anything stupid.

However, by flashing the factory `preloader` image, you can save yourself a lot of headaches.

### Why do we need to flash the Engineering `preloader` image and how it works?

Usually, MTK devices follow this boot pattern:

    `BootROM` -> `preloader` -> `Little Kernel (lk)` -> `kernel`

Preloader runs after BROM, and does not require any security verification to write partitions. Normal HyperOs `preloader` has download disabled. When you brick, you have Preloader starting and rebooting constantly. There's no way to talk to the Normal HyperOs `preloader`. The Engineering `preloader` A.K.A  `ENG preloader` on the other hand, has download enabled. so, after flashing the Engineering `preloader` image, (with ever boot) the `preloader` exposes an insecure `VCOM` port with `SLA` (Serial Link Authentication) and `DAA` (Download Agent Authentication) checks disabled, allowing you to flash images with SP Flash tool V6 without worrying about having an authorised Mi account.

that means, if something goes wrong, as long as the Factory Engineering Preloader is present you CAN unbrick!

If you use the normal HyperOs `preloader` image, the only "download" mode you can access in case of a brick is `BootROM` (which is burnt into the SoC). This requires an authorised Mi account to access and write partitions from it.

There is currently no way to bypass these checks on `duchamp`, as `BootROM` has a bunch of checks to prevent unauthorised attacks.

### Can I revert to the normal HyperOs `preloader` image?

Of course, at your own risk :P.

## Flashing process

* Download the correct `preloader` image:
  
    * [Engineering Hyperos preloader] (you can find it in this repository inside the docs/preloader folder)

There are two versions, one installable by twrp and the other by fastboot
FlashablePreloader.zip (twrp)
preloader_duchamp_eng.bin(fastboot)

* Reboot your device into fastboot mode by holding down the appropriate button combination (`Volume Down` + `Power`) until the word `FASTBOOT` in ORANGE appears on the screen.
* Open a `ADB & Fastboot tools` window on your PC and flash the `preloader` image you downloaded before.

``` bash
    # Mention the path of the images before running the commands (Mention the path of the images before running the command)
    # Ex: fastboot flash preloader1 C:/home/USER/duchamp/preloader_duchamp.bin
    fastboot flash preloader_a <preloader_duchamp_eng.bin>
    fastboot flash preloader_b <preloader_duchamp_eng.bin>
```

* Reboot your device by holding the `Power` button.
* You are good to go :D

* ...

## How to unbrick (with the `ENG preloader` image)

So you have managed to brick your device and you have previously flashed the factory `preloader` image? You can easily restore it by following  these simple steps:

!!! warning
    This method only works properly if executed on Slot A.
    If it doesn't works, you are probably on Slot B, ence the only way out is to use the `Format all + Download` option instead of `Download only`, which **ERASES** the whole device UFS.
    **Always make a backup of your partitions**

* Turn off your device.
* Open the SP Flash tool V6.
  ![1](https://github.com/TheFormidable/Unbrick/blob/e6b13b04c2d908221029d0bf230d6faba08cfd53/docs/images/1.png)
* 
* On `BROM Connection`, make sure is set on `auto`.
* Load the Fastboot ROM of your choice by pressing the `Download XML` button and selecting the `images/download_agent/flash.xml` file.
* Deselect the `preloader` option.
* 
![2](https://github.com/TheFormidable/Unbrick/blob/e6b13b04c2d908221029d0bf230d6faba08cfd53/docs/images/2.png)
* 
* Press the `Download` button.
* Connect the device to your PC (if it doesn't detect, press and hold the `Power` button for 8-10 seconds).
* The flash process should start.

You can get the latest SP Flash tool V6 from [here](https://spflashtools.com/windows/sp-flash-tool-v6-2316) and the latest `duchamp` Fastboot ROM from [here](https://mifirm.net/model/duchamp.ttt) (make sure you choose the right MIUI Fastboot ROM for your device :D).
