---
images: assets/img
typora-copy-images-to: assets/img

lang: en-AU

title: Enable Interface Graphing on IMC
author: Stephen Schwetz
tags: [KB0009,KB,Knowledge,IMC]
updated: 2023-10-07
---

# KB0009 - Enable Interface Graphing on IMC

**^KB0009^**

## TLDR - Here is a video

<video src="assets/enable%20interface%20graphing%20in%20IMC.mp4"></video>

## Step-By-Step

1. Select `Resource` from the menu bar
   ![image-20230907144440360](assets/img/image-20230907144440360.png)
1. Select `Perfomance Monitoring`
   ![image-20230907154221491](assets/img/image-20230907154221491.png)
1. Select `Monitoring Settings`
   ![image-20230907154446441](assets/img/image-20230907154446441.png)
1. Click `Add Monitor`![image-20230907161108713](assets/img/image-20230907161108713.png)
1. You will be presented with a pop up window
   * Select `Add` under Select Index
     <img src="assets/img/image-20230907161834079.png" alt="image-20230907161834079" style="zoom:50%;" />
   * Expand System-Interface Statistics and select
     * Interface Receiving Rate (bps)
     * Interface Transmitting Rate (bps)<img src="assets/img/image-20230907162218225.png" alt="image-20230907162218225" style="zoom:50%;" />
     * Scroll to the bottom of the windows and click OK
   * Select `Add` under Select Device
     ![image-20230907162424623](assets/img/image-20230907162424623.png)
   * You will be presented with a new pop up window, click on `Device View` on the left hand tree
     ![image-20230907162601108](assets/img/image-20230907162601108.png)
   * Select the devices you wish to monitor presented on the right
     ![image-20230907162751618](assets/img/image-20230907162751618.png)
   * Click the `single down arrow` to add the devices to Selected Devices
     ![image-20230907163003065](assets/img/image-20230907163003065.png)
   * Scroll to the bottom of the screen and selecct `Up Physical Interfaces` and Click OK
     ![image-20230907163214332](../../../../../../../assets/image-20230907163214332.png) 
   * You will be shown the interfaces that have been added for monitoring
     ![image-20230907163312076](../../../../../../../assets/image-20230907163312076.png)
