---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img
typora-root-url: ../

lang: en-AU

title: KB0006
subtitle: Deploying a New Edge Node
author: Stephen Schwetz
tags: 
  - KB0006
  - NSX
  - Edge
  - Knowledge Base
  - KB
updated: 2023-10-07

---

There will be times within the NSX lifecycle when you could be required to deploy a new edge node, including:

* The edge node has become cpu constrained;
* The NSX environment needs to have additional throughput, and you are already running large edge nodes; or
* The edge node has become corrupted and needs to be replaced to restore service.

## TLDR - Here is a Video

<video src="/assets/Video/Deploy%20a%20New%20NSX-T%20Edge%20Node.mp4"></video>

## Step-By-Step Guide

1. Log in to the NSX manager; the global or local manager is suitable. If you are using the global manager, you must select the local manager for the environment to which you will add the edge node.
1. From the NSX manager screen select `System` from the top menu bar.

<img src="assets/img/image-20230906223124140.png" alt="`image-20230906223124140`" style="zoom: 
   25%;" />

1. From the left hand menu click on `Fabric`.

   ![image-20230906223156853](../assets/img/image-20230906223156853.png)

1. From the menu that has extended from below the Fabric option select `Nodes`.

   <img src="assets/img/image-20230906223240360.png" alt="image-20230906223240360" style="zoom:50%;" />

1. Select `Edge Transport Nodes` from the secondary menu that has appeared on the right.

   <img src="assets/img/image-20230906223328229.png" alt="image-20230906223328229" style="zoom: 30%;" />

1. Select `+ Add Edge Node` from the action menu.

   <img src="assets/img/image-20230906223444988.png" alt="image-20230906223444988" style="zoom: 40%;" />

1. You will then be presented with the `Add Edge Node` wizard

   * Fill out the name of the node, this will be how it is displayed in the UI
   * Fill out the FQDN[^fn1]
   * Select the Form factor for the node. [^fn2]
   * Do not use Small unless it it for a lab environment
   * Then click next

   <img src="assets/img/image-20230906223604964.png" alt="image-20230906223604964" style="zoom:33%;" />

1. Fill in the User

   <img src="assets/img/image-20230906223749731.png" alt="image-20230906223749731" style="zoom:33%;" />

   <img src="assets/img/image-20230906223802771.png" alt="image-20230906223802771" style="zoom:33%;" />

1. test

   ![image-20230906223935148](assets/img/image-20230906223935148.png)

1.  test

    ![image-20230906224330505](assets/img/image-20230906224330505.png)

1.   test

     ![image-20230906224435131](assets/img/image-20230906224435131.png)

1.  test


   ![image-20230906224818524](assets/img/image-20230906224818524.png)

1.  test


![image-20230906224925219](assets/img/image-20230906224925219.png)

1.  test


<img src="assets/img/image-20230906225000802.png" alt="image-20230906225000802"  />

1. test


<img src="assets/img/image-20230906225055186.png" alt="image-20230906225055186"  />

1. test


<img src="assets/img/image-20230906225104376.png" alt="image-20230906225104376"  />

1. test


![image-20230906225422762](../assets/img/image-20230906225422762.png)

1. test


![image-20230906225849555](/assets/image-20230906225849555.png)

1. test


![image-20230906230322421](../../../../../../assets/image-20230906230322421.png)

1. test


[^fn1]: *F*ully *Q*ualified *D*omain *N*ame
[^fn2]: Ensure that you have selected the correct node sizing for the workload that you are expecting, as you cannot resize an edge node, you will have to deploy a new edge node of the correct size and  [replace it](KB0007 = Replacing an Edge Node.md) 
