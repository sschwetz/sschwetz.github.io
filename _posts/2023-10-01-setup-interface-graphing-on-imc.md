---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: ../assets/img/${filename}
typora-root-url: ../

lang: en-AU

title: Setup Interface Graphine on IMC
subtitle: 
author: stephen
tags: [KB,Knowledge]
updated: 2023-10-07
---

If you have HPE or Aruba switches you can optionally purchase HPE Integrated Management Center (IMC) to mange your switches through their lifecyles. The product allows for monitoring, management, and backing up of the suite of the switches whether they are Provision, Comware, Aruba OS, or Aruba CX.

One function that can be enabled is interface throughput graphing and alarming if an interface exceeds a percentage of it's maximum throughput (by default this is set to 80% warn and 90% critical)

## Step-By-Step

1. Select `Resource` from the menu bar
   ![image-20230907144440360](/../../../../../assets/image-20230907144440360.png)
1. Select `Perfomance Monitoring`
   ![image-20230907154221491](/../../../../../assets/image-20230907154221491.png)
1. Select `Monitoring Settings`
   ![image-20230907154446441](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907154446441.png)
1. Click `Add Monitor`![image-20230907161108713](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907161108713.png)
1. You will be presented with a pop up window
   * Select `Add` under Select Index
     <img src="/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907161834079.png" alt="image-20230907161834079" style="zoom:50%;" />
   * Expand System-Interface Statistics and select
     * Interface Receiving Rate (bps)
     * Interface Transmitting Rate (bps)<img src="/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907162218225.png" alt="image-20230907162218225" style="zoom:50%;" />
     * Scroll to the bottom of the windows and click OK
   * Select `Add` under Select Device
     ![image-20230907162424623](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907162424623.png)
   * You will be presented with a new pop up window, click on `Device View` on the left hand tree
     ![image-20230907162601108](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907162601108.png)
   * Select the devices you wish to monitor presented on the right
     ![image-20230907162751618](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907162751618.png)
   * Click the `single down arrow` to add the devices to Selected Devices
     ![image-20230907163003065](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907163003065.png)
   * Scroll to the bottom of the screen and selecct `Up Physical Interfaces` and Click OK
     ![image-20230907163214332](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907163214332.png) 
   * You will be shown the interfaces that have been added for monitoring
     ![image-20230907163312076](/assets/img/2023-10-01-setup-interface-graphing-on-imc/image-20230907163312076.png)

