---
categories: Knowledge-Base
images: /assets/img
typora-copy-images-to: ./assets/img

lang: en-AU
layout: page
full-width: true
title: Enable Interface Graphing on IMC
subtitle: KB0009
author: Stephen Schwetz
tags: [KB0009,KB,Knowledge,IMC]
updated: 2023-10-07
tags: 
  - KB0009
  - NSX
  - Edge
  - Manager
  - Knowledge Base
  - KB
---

# KB0009 - Enable Interface Graphing on IMC

**^KB0009^**

## TLDR - Here is a video

<video src="assets/enable%20interface%20graphing%20in%20IMC.mp4"></video>

## Step-By-Step

1. Select `Resource` from the menu bar
   ![image-20230907144440360](assets/img/image-20230907144440360-6680302.png)
1. Select `Perfomance Monitoring`
   ![image-20230907154221491](assets/img/image-20230907154221491-6680302.png)
1. Select `Monitoring Settings`
   ![image-20230907154446441](assets/img/image-20230907154446441-6680302.png)
1. Click `Add Monitor`![image-20230907161108713](assets/img/image-20230907161108713-6680302.png)
1. You will be presented with a pop up window
   * Select `Add` under Select Index
     <img src="assets/img/image-20230907161834079-6680302.png" alt="image-20230907161834079" style="zoom:50%;" />
   * Expand System-Interface Statistics and select
     * Interface Receiving Rate (bps)
     * Interface Transmitting Rate (bps)<img src="assets/img/image-20230907162218225-6680302.png" alt="image-20230907162218225" style="zoom:50%;" />
     * Scroll to the bottom of the windows and click OK
   * Select `Add` under Select Device
     ![image-20230907162424623](assets/img/image-20230907162424623-6680302.png)
   * You will be presented with a new pop up window, click on `Device View` on the left hand tree
     ![image-20230907162601108](assets/img/image-20230907162601108-6680302.png)
   * Select the devices you wish to monitor presented on the right
     ![image-20230907162751618](assets/img/image-20230907162751618-6680302.png)
   * Click the `single down arrow` to add the devices to Selected Devices
     ![image-20230907163003065](assets/img/image-20230907163003065-6680302.png)
   * Scroll to the bottom of the screen and selecct `Up Physical Interfaces` and Click OK
     ![image-20231007230600750](assets/img/image-20231007230600750.png)
   * You will be shown the interfaces that have been added for monitoring
     ![image-20231007230615135](assets/img/image-20231007230615135.png)

